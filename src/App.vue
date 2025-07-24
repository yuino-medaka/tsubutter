<script setup>
import { ref } from 'vue'

const message = ref('ツブッター - Vue.js動作確認')
const count = ref(0)

const increment = () => {
  count.value++
}
</script>

<template>
  <div id="app" style="padding: 2rem; text-align: center; font-family: Arial, sans-serif; background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); min-height: 100vh; display: flex; align-items: center; justify-content: center;">
    <div style="background: white; padding: 2rem; border-radius: 1rem; box-shadow: 0 4px 12px rgba(0,0,0,0.1); max-width: 600px;">
      <h1 style="color: #1da1f2; margin-bottom: 1rem;">
        🐦 {{ message }}
      </h1>
      
      <p style="color: #4a5568; margin-bottom: 2rem;">
        Vue.jsが正常に動作しています！
      </p>
      
      <div style="margin-bottom: 2rem;">
        <p style="font-size: 1.2rem; color: #1a202c;">
          カウント: <strong>{{ count }}</strong>
        </p>
        <button 
          @click="increment"
          style="background: #1da1f2; color: white; border: none; padding: 0.75rem 1.5rem; border-radius: 0.5rem; font-size: 1rem; cursor: pointer; margin-top: 1rem;"
        >
          🎯 カウントアップ
        </button>
      </div>
      
      <div style="background: #f8fafc; padding: 1rem; border-radius: 0.5rem; border-left: 4px solid #1da1f2;">
        <p style="margin: 0; color: #4a5568; font-size: 0.9rem;">
          <strong>✅ Vue.js動作確認:</strong> リアクティブな機能が正常に動作しています
        </p>
      </div>
    </div>
  </div>
</template>

<style scoped>
#app {
  width: 100%;
  min-height: 100vh;
}

button:hover {
  background: #1a91da !important;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(29, 161, 242, 0.3);
  transition: all 0.3s ease;
}
</style>
