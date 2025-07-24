<script setup>
import { ref, onMounted } from 'vue'

const posts = ref([])
const loading = ref(true)
const error = ref('')

const fetchPosts = async () => {
  loading.value = true
  error.value = ''
  
  try {
    posts.value = []
  } catch (err) {
    console.error('投稿取得エラー:', err)
    error.value = '投稿の取得に失敗しました: ' + err.message
  } finally {
    loading.value = false
  }
}

onMounted(() => {
  fetchPosts()
})
</script>

<template>
  <div class="post-list">
    <div class="list-header">
      <h2 class="list-title">投稿一覧</h2>
      <button @click="fetchPosts" class="btn btn-secondary refresh-btn">
        更新
      </button>
    </div>
    
    <div v-if="loading" class="loading">
      <div class="loading-spinner"></div>
      <p>投稿を読み込み中...</p>
    </div>
    
    <div v-else-if="error" class="error-message">
      <p>{{ error }}</p>
      <button @click="fetchPosts" class="btn btn-primary">
        再試行
      </button>
    </div>
    
    <div v-else-if="posts.length > 0" class="posts-container">
      <div v-for="post in posts" :key="post.id" class="post-card">
        <div class="post-content">
          {{ post.content }}
        </div>
      </div>
    </div>
    
    <div v-else class="empty-state">
      <div class="empty-icon">📝</div>
      <h3>まだ投稿がありません</h3>
      <p>最初の投稿をしてみましょう！</p>
    </div>
  </div>
</template>

<style scoped>
.post-list {
  max-width: 800px;
  margin: 0 auto;
}

.list-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 2rem;
}

.list-title {
  font-size: 1.8rem;
  color: #1a202c;
  margin: 0;
}

.refresh-btn {
  padding: 0.5rem 1rem;
  font-size: 0.9rem;
}

.loading {
  text-align: center;
  padding: 3rem 0;
}

.loading-spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #e2e8f0;
  border-top: 4px solid #1da1f2;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin: 0 auto 1rem;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.error-message {
  text-align: center;
  padding: 2rem;
  background: #fef2f2;
  border: 1px solid #fecaca;
  border-radius: 0.75rem;
  color: #dc2626;
}

.error-message p {
  margin-bottom: 1rem;
}

.posts-container {
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.post-card {
  background: white;
  border: 1px solid #e2e8f0;
  border-radius: 1rem;
  padding: 1.5rem;
  transition: all 0.3s ease;
}

.post-card:hover {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  transform: translateY(-2px);
}

.post-content {
  font-size: 1.1rem;
  line-height: 1.6;
  color: #1a202c;
  white-space: pre-wrap;
  word-break: break-word;
}

.empty-state {
  text-align: center;
  padding: 4rem 2rem;
  color: #64748b;
}

.empty-icon {
  font-size: 4rem;
  margin-bottom: 1rem;
}

.empty-state h3 {
  font-size: 1.5rem;
  margin-bottom: 0.5rem;
  color: #1a202c;
}

.empty-state p {
  font-size: 1.1rem;
}
</style>
