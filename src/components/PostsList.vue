<template>
  <div class="posts-list">
    <div class="filter-section mb-4">
      <div class="row align-items-center">
        <div class="col-md-4 mb-3 mb-md-0">
          <select v-model="selectedAuthor" class="form-control">
            <option value="">Все авторы</option>
            <option v-for="user in users" :key="user.id" :value="user.id">
              {{ user.name }}
            </option>
          </select>
        </div>
        <div class="col-md-8">
          <div class="input-group">
            <div class="input-group-prepend">
              <span class="input-group-text">
                <i class="fas fa-search"></i>🔍
              </span>
            </div>
            <input
              type="text"
              class="form-control"
              v-model="searchQuery"
              placeholder="Поиск по заголовку..."
            >
          </div>
        </div>
      </div>
    </div>

    <div class="posts-counter mb-4">
      <div class="alert alert-info">
        Найдено статей: <strong>{{ filteredPosts.length }}</strong>
        <span class="text-muted ml-2" v-if="totalPosts !== filteredPosts.length">
          (всего: {{ totalPosts }})
        </span>
      </div>
    </div>
    
    <div class="row">
      <div v-for="post in filteredPosts" :key="post.id" class="col-md-6 mb-4">
        <div class="card h-100 cursor-pointer" @click="openPost(post)">
          <div class="card-body">
            <h5 class="card-title" v-html="highlightText(post.title)"></h5>
            <p class="card-text">{{ truncateText(post.body, 100) }}</p>
            <p class="card-text">
              <small class="text-muted"><strong>Автор:</strong> {{ getUserName(post.userId) }}</small>
            </p>
          </div>
        </div>
      </div>
    </div>

    <PostModal
      :post="selectedPost"
      :users="users"
      :show="showModal"
      @close="closeModal"
    />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted, computed } from 'vue'
import axios from 'axios'
import PostModal from './PostModal.vue'

interface Post {
  id: number
  title: string
  body: string
  userId: number
}

interface User {
  id: number
  name: string
}

export default defineComponent({
  name: 'PostsList',
  components: {
    PostModal
  },
  setup() {
    const posts = ref<Post[]>([])
    const users = ref<User[]>([])
    const selectedAuthor = ref('')
    const searchQuery = ref('')
    const showModal = ref(false)
    const selectedPost = ref<Post | null>(null)

    const fetchPosts = async () => {
      try {
        const response = await axios.get<Post[]>('https://jsonplaceholder.typicode.com/posts')
        posts.value = response.data
      } catch (error) {
        console.error('Error fetching posts:', error)
      }
    }

    const fetchUsers = async () => {
      try {
        const response = await axios.get<User[]>('https://jsonplaceholder.typicode.com/users')
        users.value = response.data
      } catch (error) {
        console.error('Error fetching users:', error)
      }
    }

    const totalPosts = computed(() => posts.value.length)

    const filteredPosts = computed(() => {
      let result = posts.value

      // Фильтрация по автору
      if (selectedAuthor.value) {
        result = result.filter(post => post.userId === Number(selectedAuthor.value))
      }

      // Фильтрация по поисковому запросу (только по заголовкам)
      if (searchQuery.value.trim()) {
        const query = searchQuery.value.toLowerCase().trim()
        result = result.filter(post =>
          post.title.toLowerCase().includes(query)
        )
      }

      return result
    })

    const getUserName = (userId: number): string => {
      const user = users.value.find(u => u.id === userId)
      return user ? user.name : 'Неизвестный автор'
    }

    const truncateText = (text: string, length: number): string => {
      if (text.length <= length) return text
      return text.substring(0, length) + '...'
    }

    const openPost = (post: Post) => {
      selectedPost.value = post
      showModal.value = true
    }

    const closeModal = () => {
      showModal.value = false
      selectedPost.value = null
    }

    const highlightText = (text: string): string => {
      if (!searchQuery.value.trim()) return text
      
      const searchTerm = searchQuery.value.trim().toLowerCase()
      const regex = new RegExp(`(${searchTerm})`, 'gi')
      
      return text.replace(regex, '<span class="highlight">$1</span>')
    }

    onMounted(() => {
      fetchPosts()
      fetchUsers()
    })

    return {
      posts,
      users,
      selectedAuthor,
      searchQuery,
      filteredPosts,
      totalPosts,
      getUserName,
      truncateText,
      showModal,
      selectedPost,
      openPost,
      closeModal,
      highlightText
    }
  }
})
</script>

<style lang="scss" scoped>
.posts-list {
  padding: 20px;
  
  .filter-section {
    .input-group-text {
      background-color: #fff;
    }
  }

  .posts-counter {
    .alert {
      margin-bottom: 0;
      
      strong {
        font-size: 1.1rem;
      }
      
      .text-muted {
        font-size: 0.9rem;
      }
    }
  }

  .card {
    transition: transform 0.2s;
    cursor: pointer;
    
    &:hover {
      transform: translateY(-5px);
    }

    .card-title {
      font-size: 1.1rem;
      font-weight: 500;
      margin-bottom: 1rem;

      :deep(.highlight) {
        background-color: #fff3cd;
        padding: 0.1rem 0.2rem;
        border-radius: 3px;
        font-weight: bold;
        color: #856404;
      }
    }

    .card-text {
      color: #6c757d;
      
      &:last-child {
        margin-bottom: 0;
      }
    }
  }
}
</style> 