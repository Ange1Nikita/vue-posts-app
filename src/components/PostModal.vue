<template>
  <div class="modal-wrapper" v-if="post" :class="{ 'show': show }">
    <div class="modal-dialog modal-lg" role="document">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title">{{ post.title }}</h5>
          <button type="button" class="close" @click="$emit('close')" aria-label="Close">
            <span aria-hidden="true">&times;</span>
          </button>
        </div>
        <div class="modal-body">
          <p class="lead mb-4">{{ post.body }}</p>
          <div class="author-info">
            <small class="text-muted"><strong>Автор:</strong> {{ authorName }}</small>
          </div>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" @click="$emit('close')">Закрыть</button>
        </div>
      </div>
    </div>
    <div class="modal-backdrop" @click="$emit('close')"></div>
  </div>
</template>

<script lang="ts">
import { defineComponent, computed, PropType } from 'vue'

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
  name: 'PostModal',
  props: {
    post: {
      type: Object as PropType<Post | null>,
      required: true
    },
    users: {
      type: Array as () => User[],
      required: true
    },
    show: {
      type: Boolean,
      required: true
    }
  },
  setup(props) {
    const authorName = computed(() => {
      if (!props.post) return 'Неизвестный автор'
      const user = props.users.find(u => u.id === props.post.userId)
      return user ? user.name : 'Неизвестный автор'
    })

    return {
      authorName
    }
  }
})
</script>

<style lang="scss" scoped>
@keyframes modalFadeIn {
  from {
    opacity: 0;
    visibility: hidden;
  }
  to {
    opacity: 1;
    visibility: visible;
  }
}

@keyframes modalSlideIn {
  from {
    transform: scale(0.8) translateY(-40px);
    opacity: 0;
  }
  to {
    transform: scale(1) translateY(0);
    opacity: 1;
  }
}

@keyframes modalBackdropIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

.modal-wrapper {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1050;
  opacity: 0;
  visibility: hidden;
  perspective: 1000px;
  
  &.show {
    animation: modalFadeIn 0.3s ease forwards;
    
    .modal-dialog {
      animation: modalSlideIn 0.4s cubic-bezier(0.16, 1, 0.3, 1) forwards;
    }
    
    .modal-backdrop {
      animation: modalBackdropIn 0.3s ease forwards;
    }
  }

  .modal-dialog {
    position: relative;
    z-index: 1051;
    margin: 1.75rem;
    max-width: 800px;
    width: 100%;
    opacity: 0;
    transform: scale(0.8) translateY(-40px);
    will-change: transform, opacity;
  }
  
  .modal-content {
    background-color: #fff;
    border: none;
    border-radius: 8px;
    box-shadow: 0 8px 30px rgba(0, 0, 0, 0.12);
    transform-origin: top;
    backface-visibility: hidden;
    
    .modal-header {
      background-color: #fff;
      border-bottom: 1px solid #eee;
      border-top-left-radius: 8px;
      border-top-right-radius: 8px;
      padding: 1.5rem;
      
      .modal-title {
        font-weight: 500;
        line-height: 1.4;
        margin: 0;
        color: #333;
      }

      .close {
        padding: 1rem;
        margin: -1rem -1rem -1rem auto;
        font-size: 1.5rem;
        color: #666;
        opacity: 0.5;
        transition: all 0.2s ease;
        transform-origin: center;

        &:hover {
          opacity: 1;
          transform: rotate(90deg);
        }
      }
    }

    .modal-body {
      background-color: #fff;
      padding: 1.5rem;
      
      .lead {
        font-size: 1.1rem;
        line-height: 1.6;
        color: #444;
        opacity: 0;
        animation: contentFadeIn 0.4s ease 0.2s forwards;
      }
    }

    .modal-footer {
      background-color: #fff;
      border-top: 1px solid #eee;
      border-bottom-left-radius: 8px;
      border-bottom-right-radius: 8px;
      padding: 1.5rem;

      .btn {
        transition: all 0.2s ease;
        
        &:hover {
          transform: translateY(-2px);
          box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }
      }
    }
  }
}

.modal-backdrop {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 1040;
  opacity: 0;
  backdrop-filter: blur(3px);
}

@keyframes contentFadeIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style> 