<script setup>
import { ref } from 'vue'

const message = ref('ツブッター - 動作確認中')
</script>

<template>
  <div id="app" style="padding: 2rem; text-align: center; font-family: Arial, sans-serif; background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); min-height: 100vh; display: flex; align-items: center; justify-content: center;">
    <div style="background: white; padding: 2rem; border-radius: 1rem; box-shadow: 0 4px 12px rgba(0,0,0,0.1); max-width: 600px;">
      <h1 style="color: #1da1f2; margin-bottom: 1rem;">
        🐦 {{ message }}
      </h1>
      
      <p style="color: #4a5568; margin-bottom: 2rem;">
        アプリケーションが正常に動作しています！
      </p>
      
      <div style="background: #f8fafc; padding: 1rem; border-radius: 0.5rem; border-left: 4px solid #1da1f2;">
        <p style="margin: 0; color: #4a5568; font-size: 0.9rem;">
          <strong>✅ 動作確認:</strong> Vue.jsが正常に動作しています
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
</style>
