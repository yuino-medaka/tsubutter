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
  <div id="app" style="min-height: 100vh; background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);">
    <header style="background: rgba(255, 255, 255, 0.95); backdrop-filter: blur(10px); border-bottom: 1px solid rgba(255, 255, 255, 0.2); padding: 1rem 0; position: sticky; top: 0; z-index: 100;">
      <div style="width: 100%; max-width: 1200px; margin: 0 auto; padding: 0 1rem;">
        <h1 style="font-size: 2.5rem; font-weight: bold; color: #1a202c; margin-bottom: 0.5rem; text-align: center; display: flex; align-items: center; justify-content: center; gap: 0.5rem;">
          <span style="font-size: 2rem;">🐦</span>
          {{ appTitle }}
        </h1>
        <p style="font-size: 1.1rem; color: #4a5568; text-align: center; margin-bottom: 2rem; display: flex; align-items: center; justify-content: center; gap: 0.5rem;">
          <span style="font-size: 1rem;">💭</span>
          {{ subtitle }}
        </p>
        
        <!-- タブナビゲーション -->
        <nav style="display: flex; justify-content: center; gap: 1rem; margin-bottom: 1rem; flex-wrap: wrap;">
          <button 
            @click="switchTab('posts')" 
            :style="[
              'padding: 0.75rem 1.5rem; border: 2px solid #1da1f2; background: transparent; color: #1da1f2; border-radius: 2rem; font-size: 1rem; font-weight: 600; cursor: pointer; transition: all 0.3s ease; display: flex; align-items: center; gap: 0.5rem; min-width: 140px; justify-content: center;',
              activeTab === 'posts' ? 'background: #1da1f2; color: white; box-shadow: 0 4px 12px rgba(29, 161, 242, 0.3); transform: translateY(-2px);' : ''
            ]"
          >
            <span style="font-size: 1.1rem;">📝</span>
            投稿一覧
          </button>
          <button 
            @click="switchTab('post')" 
            :style="[
              'padding: 0.75rem 1.5rem; border: 2px solid #1da1f2; background: transparent; color: #1da1f2; border-radius: 2rem; font-size: 1rem; font-weight: 600; cursor: pointer; transition: all 0.3s ease; display: flex; align-items: center; gap: 0.5rem; min-width: 140px; justify-content: center;',
              activeTab === 'post' ? 'background: #1da1f2; color: white; box-shadow: 0 4px 12px rgba(29, 161, 242, 0.3); transform: translateY(-2px);' : ''
            ]"
          >
            <span style="font-size: 1.1rem;">✍️</span>
            投稿する
          </button>
        </nav>
      </div>
    </header>
    
    <main style="padding: 2rem 0; min-height: calc(100vh - 200px);">
      <div style="width: 100%; max-width: 1200px; margin: 0 auto; padding: 0 1rem;">
        <!-- 投稿一覧タブ -->
        <div v-if="activeTab === 'posts'" style="background: white; border-radius: 1rem; padding: 2rem; box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1); margin-top: 1rem;">
          <h2 style="font-size: 1.8rem; color: #1a202c; margin-bottom: 1rem; display: flex; align-items: center; gap: 0.5rem;">
            <span style="font-size: 1.5rem;">📝</span>
            投稿一覧
          </h2>
          <p style="color: #4a5568; font-size: 1.1rem;">
            投稿一覧がここに表示されます
          </p>
        </div>
        
        <!-- 投稿フォームタブ -->
        <div v-if="activeTab === 'post'" style="background: white; border-radius: 1rem; padding: 2rem; box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1); margin-top: 1rem;">
          <h2 style="font-size: 1.8rem; color: #1a202c; margin-bottom: 1rem; display: flex; align-items: center; gap: 0.5rem;">
            <span style="font-size: 1.5rem;">✍️</span>
            投稿する
          </h2>
          <p style="color: #4a5568; font-size: 1.1rem;">
            投稿フォームがここに表示されます
          </p>
        </div>
      </div>
    </main>
  </div>
</template>

<style scoped>
#app {
  width: 100%;
  min-height: 100vh;
}

button:hover {
  background: #1da1f2 !important;
  color: white !important;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(29, 161, 242, 0.3);
  transition: all 0.3s ease;
}
</style>
