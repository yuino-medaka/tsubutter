<script setup>
import { ref } from 'vue'
import PostForm from './PostForm.vue'
import PostList from './PostList.vue'
import AdminPanel from './AdminPanel.vue'

const appTitle = ref('ツブッター')
const subtitle = ref('気持ちを投稿しよう')
const activeTab = ref('posts') // 'posts', 'post', 'admin'

const switchTab = (tab) => {
  activeTab.value = tab
}
</script>

<template>
  <div class="main-app">
    <header class="header">
      <div class="container">
        <h1 class="title">
          <svg class="title-icon" viewBox="0 0 24 24" fill="currentColor">
            <path d="M23.643 4.937c-.835.37-1.732.62-2.675.733.962-.576 1.7-1.49 2.048-2.578-.9.534-1.897.922-2.958 1.13-.85-.904-2.06-1.47-3.4-1.47-2.572 0-4.658 2.086-4.658 4.66 0 .364.042.718.12 1.06-3.873-.195-7.304-2.05-9.602-4.868-.4.69-.63 1.49-.63 2.342 0 1.616.823 3.043 2.072 3.878-.764-.025-1.482-.234-2.11-.583v.06c0 2.257 1.605 4.14 3.737 4.568-.392.106-.803.162-1.227.162-.3 0-.593-.028-.877-.082.593 1.85 2.313 3.198 4.352 3.234-1.595 1.25-3.604 1.995-5.786 1.995-.376 0-.747-.022-1.112-.065 2.062 1.323 4.51 2.093 7.14 2.093 8.57 0 13.255-7.098 13.255-13.254 0-.2-.005-.402-.014-.602.91-.658 1.7-1.477 2.323-2.41z"/>
          </svg>
          {{ appTitle }}
        </h1>
        <p class="subtitle">
          <svg class="subtitle-icon" viewBox="0 0 24 24" fill="currentColor">
            <path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-2 15l-5-5 1.41-1.41L10 14.17l7.59-7.59L19 8l-9 9z"/>
          </svg>
          {{ subtitle }}
        </p>
        
        <!-- タブナビゲーション -->
        <nav class="tab-nav">
          <button 
            @click="switchTab('posts')" 
            :class="['tab-btn', { active: activeTab === 'posts' }]"
          >
            <svg class="tab-icon" viewBox="0 0 24 24" fill="currentColor">
              <path d="M19 3H5c-1.1 0-2 .9-2 2v14c0 1.1.9 2 2 2h14c1.1 0 2-.9 2-2V5c0-1.1-.9-2-2-2zm-5 14H7v-2h7v2zm3-4H7v-2h10v2zm0-4H7V7h10v2z"/>
            </svg>
            投稿一覧
          </button>
          <button 
            @click="switchTab('post')" 
            :class="['tab-btn', { active: activeTab === 'post' }]"
          >
            <svg class="tab-icon" viewBox="0 0 24 24" fill="currentColor">
              <path d="M3 17.25V21h3.75L17.81 9.94l-3.75-3.75L3 17.25zM20.71 7.04c.39-.39.39-1.02 0-1.41l-2.34-2.34c-.39-.39-1.02-.39-1.41 0l-1.83 1.83 3.75 3.75 1.83-1.83z"/>
            </svg>
            投稿する
          </button>
          <button 
            @click="switchTab('admin')" 
            :class="['tab-btn', { active: activeTab === 'admin' }]"
          >
            <svg class="tab-icon" viewBox="0 0 24 24" fill="currentColor">
              <path d="M12 2l3.09 6.26L22 9.27l-5 4.87 1.18 6.88L12 17.77l-6.18 3.25L7 14.14 2 9.27l6.91-1.01L12 2z"/>
            </svg>
            管理 ⭐
          </button>
        </nav>
      </div>
    </header>
    
    <main class="main-content">
      <div class="container">
        <!-- 投稿一覧タブ -->
        <div v-if="activeTab === 'posts'" class="tab-content">
          <PostList />
        </div>
        
        <!-- 投稿フォームタブ -->
        <div v-if="activeTab === 'post'" class="tab-content">
          <PostForm />
        </div>
        
        <!-- 管理パネルタブ -->
        <div v-if="activeTab === 'admin'" class="tab-content">
          <AdminPanel />
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

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 1rem;
}

.title {
  font-size: 2.5rem;
  font-weight: bold;
  color: #1a202c;
  margin-bottom: 0.5rem;
  text-align: center;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
}

.title-icon {
  width: 2rem;
  height: 2rem;
  color: #1da1f2;
  animation: bounce 2s infinite;
}

.subtitle {
  font-size: 1.1rem;
  color: #4a5568;
  text-align: center;
  margin-bottom: 2rem;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
}

.subtitle-icon {
  width: 1rem;
  height: 1rem;
  color: #4a5568;
}

@keyframes bounce {
  0%, 20%, 50%, 80%, 100% {
    transform: translateY(0);
  }
  40% {
    transform: translateY(-10px);
  }
  60% {
    transform: translateY(-5px);
  }
}

.tab-nav {
  display: flex;
  justify-content: center;
  gap: 1rem;
  margin-bottom: 2rem;
  flex-wrap: wrap;
}

.tab-btn {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.75rem 1.5rem;
  border: none;
  border-radius: 0.75rem;
  background: rgba(255, 255, 255, 0.8);
  color: #4a5568;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  backdrop-filter: blur(10px);
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.tab-btn:hover {
  background: rgba(255, 255, 255, 0.95);
  transform: translateY(-2px);
  box-shadow: 0 8px 25px rgba(0, 0, 0, 0.15);
}

.tab-btn.active {
  background: #1da1f2;
  color: white;
  box-shadow: 0 8px 25px rgba(29, 161, 242, 0.3);
}

.tab-icon {
  width: 1.2rem;
  height: 1.2rem;
  color: currentColor;
}

.main-content {
  padding: 2rem 0;
}

.tab-content {
  background: white;
  border-radius: 1rem;
  padding: 2rem;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
  margin-top: 1rem;
}

@media (max-width: 768px) {
  .title {
    font-size: 2rem;
  }
  
  .title-icon {
    width: 1.5rem;
    height: 1.5rem;
  }
  
  .tab-nav {
    flex-direction: column;
    align-items: stretch;
  }
  
  .tab-btn {
    justify-content: center;
  }
  
  .tab-content {
    padding: 1.5rem;
    margin: 0.5rem;
  }
}
</style>
