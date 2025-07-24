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
        <h1 class="title">
          <span class="title-icon">🐦</span>
          {{ appTitle }}
        </h1>
        <p class="subtitle">
          <span class="subtitle-icon">💭</span>
          {{ subtitle }}
        </p>
        
        <!-- タブナビゲーション -->
        <nav class="tab-nav">
          <button 
            @click="switchTab('posts')" 
            :class="['tab-btn', { active: activeTab === 'posts' }]"
          >
            <span class="tab-icon">📝</span>
            投稿一覧
          </button>
          <button 
            @click="switchTab('post')" 
            :class="['tab-btn', { active: activeTab === 'post' }]"
          >
            <span class="tab-icon">✍️</span>
            投稿する
          </button>
        </nav>
      </div>
    </header>
    
    <main class="main-content">
      <div class="container">
        <!-- 投稿一覧タブ -->
        <div v-if="activeTab === 'posts'" class="tab-content">
          <h2>📝 投稿一覧</h2>
          <p>投稿一覧がここに表示されます</p>
        </div>
        
        <!-- 投稿フォームタブ -->
        <div v-if="activeTab === 'post'" class="tab-content">
          <h2>✍️ 投稿する</h2>
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
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
}

.title-icon {
  font-size: 2rem;
  animation: bounce 2s infinite;
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
  font-size: 1rem;
  animation: pulse 2s infinite;
}

@keyframes pulse {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.1);
  }
  100% {
    transform: scale(1);
  }
}

.tab-nav {
  display: flex;
  justify-content: center;
  gap: 1rem;
  margin-bottom: 1rem;
  flex-wrap: wrap;
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
  display: flex;
  align-items: center;
  gap: 0.5rem;
  min-width: 140px;
  justify-content: center;
}

.tab-btn:hover {
  background: #1da1f2;
  color: white;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(29, 161, 242, 0.3);
}

.tab-btn.active {
  background: #1da1f2;
  color: white;
  box-shadow: 0 4px 12px rgba(29, 161, 242, 0.3);
  transform: translateY(-2px);
}

.tab-icon {
  font-size: 1.1rem;
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
  animation: fadeIn 0.5s ease-in-out;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* レスポンシブ対応 */
@media (max-width: 768px) {
  .title {
    font-size: 2rem;
  }
  
  .title-icon {
    font-size: 1.5rem;
  }
  
  .subtitle {
    font-size: 1rem;
  }
  
  .tab-nav {
    flex-direction: column;
    align-items: center;
  }
  
  .tab-btn {
    width: 200px;
  }
  
  .tab-content {
    padding: 1rem;
    margin: 0.5rem;
  }
}

@media (max-width: 480px) {
  .title {
    font-size: 1.8rem;
  }
  
  .title-icon {
    font-size: 1.3rem;
  }
  
  .tab-content {
    padding: 0.75rem;
    margin: 0.25rem;
  }
  
  .tab-btn {
    width: 180px;
    padding: 0.6rem 1.2rem;
    font-size: 0.9rem;
  }
}
</style>
