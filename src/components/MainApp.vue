<script setup>
import { ref } from 'vue'

const appTitle = ref('ツブッター')
const subtitle = ref('気持ちを投稿しよう')
const activeTab = ref('posts')

const switchTab = (tab) => {
  activeTab.value = tab
}
</script>

<template>
  <div class="main-app">
    <header class="header">
      <div class="container">
        <h1 class="title">{{ appTitle }}</h1>
        <p class="subtitle">{{ subtitle }}</p>
        
        <nav class="tab-nav">
          <button 
            @click="switchTab('posts')" 
            :class="['tab-btn', { active: activeTab === 'posts' }]"
          >
            投稿一覧
          </button>
          <button 
            @click="switchTab('post')" 
            :class="['tab-btn', { active: activeTab === 'post' }]"
          >
            投稿する
          </button>
        </nav>
      </div>
    </header>
    
    <main class="main-content">
      <div class="container">
        <div v-if="activeTab === 'posts'" class="tab-content">
          <h2>投稿一覧</h2>
          <p>投稿一覧がここに表示されます</p>
        </div>
        
        <div v-if="activeTab === 'post'" class="tab-content">
          <h2>投稿フォーム</h2>
          <p>投稿フォームがここに表示されます</p>
        </div>
      </div>
    </main>
  </div>
</template>

<style scoped>
.main-app {
  min-height: 100vh;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}

.header {
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  border-bottom: 1px solid rgba(255, 255, 255, 0.2);
  padding: 1rem 0;
  position: sticky;
  top: 0;
  z-index: 100;
}

.title {
  font-size: 2.5rem;
  font-weight: bold;
  color: #1a202c;
  margin-bottom: 0.5rem;
  text-align: center;
}

.subtitle {
  font-size: 1.1rem;
  color: #4a5568;
  text-align: center;
  margin-bottom: 2rem;
}

.tab-nav {
  display: flex;
  justify-content: center;
  gap: 1rem;
  margin-bottom: 1rem;
}

.tab-btn {
  padding: 0.75rem 1.5rem;
  border: 2px solid #1da1f2;
  background: transparent;
  color: #1da1f2;
  border-radius: 2rem;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
}

.tab-btn:hover {
  background: #1da1f2;
  color: white;
  transform: translateY(-2px);
}

.tab-btn.active {
  background: #1da1f2;
  color: white;
  box-shadow: 0 4px 12px rgba(29, 161, 242, 0.3);
}

.main-content {
  padding: 2rem 0;
  min-height: calc(100vh - 200px);
}

.tab-content {
  background: white;
  border-radius: 1rem;
  padding: 2rem;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
  margin-top: 1rem;
}
</style>
