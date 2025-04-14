<script lang="ts">
import { defineComponent } from 'vue'
import PostsList from './components/PostsList.vue'
import 'bootstrap/dist/css/bootstrap.min.css'

export default defineComponent({
  name: 'App',
  components: {
    PostsList
  }
})
</script>

<template>
  <header class="bg-primary text-white py-3 mb-4">
    <div class="container">
      <h1 class="h3 mb-0">Список публикаций</h1>
    </div>
  </header>

  <main class="container">
    <PostsList />
  </main>
</template>

<style lang="scss">
#app {
  min-height: 100vh;
  background-color: #f8f9fa;
}
</style>
