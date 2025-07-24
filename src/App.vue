<script setup>
import { ref } from 'vue'

const appTitle = ref('ツブッター')
const subtitle = ref('気持ちを投稿しよう')
const activeTab = ref('posts')
const content = ref('')
const isSubmitting = ref(false)
const message = ref('')

const switchTab = (tab) => {
  activeTab.value = tab
}

const submitPost = async () => {
  if (!content.value.trim()) {
    message.value = '投稿内容を入力してください'
    return
  }

  if (content.value.length > 280) {
    message.value = '投稿は280文字以内で入力してください'
    return
  }

  isSubmitting.value = true
  message.value = ''

  try {
    // ここでSupabaseに投稿を保存する処理を追加予定
    message.value = '投稿が完了しました！'
    content.value = ''
  } catch (error) {
    console.error('投稿エラー:', error)
    message.value = '投稿に失敗しました: ' + error.message
  } finally {
    isSubmitting.value = false
  }
}

const cancelPost = () => {
  if (content.value.trim()) {
    if (confirm('入力内容を削除して投稿をキャンセルしますか？')) {
      content.value = ''
      message.value = ''
    }
  } else {
    content.value = ''
    message.value = ''
  }
}

const clearMessage = () => {
  message.value = ''
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
          <h2 style="font-size: 1.8rem; color: #1a202c; margin-bottom: 2rem; text-align: center; display: flex; align-items: center; justify-content: center; gap: 0.5rem;">
            <span style="font-size: 1.5rem;">✍️</span>
            気持ちを投稿しよう
          </h2>
          
          <div style="max-width: 600px; margin: 0 auto;">
            <div style="position: relative; margin-bottom: 1.5rem;">
              <textarea
                v-model="content"
                placeholder="今の気持ちを280文字以内で投稿してください..."
                :maxlength="280"
                :disabled="isSubmitting"
                @input="clearMessage"
                style="width: 100%; min-height: 120px; padding: 1rem; border: 2px solid #e2e8f0; border-radius: 0.75rem; font-size: 1rem; font-family: inherit; resize: vertical; transition: border-color 0.3s ease;"
              ></textarea>
              
              <div :style="[
                'position: absolute; bottom: 0.5rem; right: 0.5rem; font-size: 0.875rem; background: rgba(255, 255, 255, 0.9); padding: 0.25rem 0.5rem; border-radius: 0.25rem; transition: color 0.3s ease;',
                content.length > 250 ? 'color: #f59e0b; font-weight: 600;' : 'color: #64748b;'
              ]">
                {{ content.length }}/280
              </div>
            </div>
            
            <div style="display: flex; justify-content: center; gap: 1rem; flex-wrap: wrap; margin-bottom: 1rem;">
              <button
                @click="cancelPost"
                :disabled="isSubmitting"
                style="padding: 0.75rem 2rem; font-size: 1.1rem; font-weight: 600; border-radius: 2rem; min-width: 150px; display: flex; align-items: center; justify-content: center; gap: 0.5rem; transition: all 0.3s ease; border: none; cursor: pointer; background: #6b7280; color: white;"
              >
                <span style="font-size: 1rem;">❌</span>
                キャンセル
              </button>
              
              <button
                @click="submitPost"
                :disabled="isSubmitting || !content.trim()"
                style="padding: 0.75rem 2rem; font-size: 1.1rem; font-weight: 600; border-radius: 2rem; min-width: 150px; display: flex; align-items: center; justify-content: center; gap: 0.5rem; transition: all 0.3s ease; border: none; cursor: pointer; background: #1da1f2; color: white;"
              >
                <span v-if="isSubmitting" style="font-size: 1rem;">⏳</span>
                <span v-else style="font-size: 1rem;">📤</span>
                <span v-if="isSubmitting">投稿中...</span>
                <span v-else>投稿する</span>
              </button>
            </div>
            
            <div v-if="message" :style="[
              'margin-top: 1rem; padding: 0.75rem 1rem; border-radius: 0.5rem; text-align: center; font-weight: 500; display: flex; align-items: center; justify-content: center; gap: 0.5rem;',
              message.includes('失敗') ? 'background-color: #fef2f2; color: #dc2626; border: 1px solid #fecaca;' : 'background-color: #dcfce7; color: #166534; border: 1px solid #bbf7d0;'
            ]">
              <span style="font-size: 1.1rem;">{{ message.includes('完了') ? '✅' : '⚠️' }}</span>
              {{ message }}
            </div>
            
            <div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); color: white; border-radius: 1rem; padding: 1.5rem; margin-top: 2rem;">
              <h3 style="margin-bottom: 1rem; font-size: 1.2rem; display: flex; align-items: center; gap: 0.5rem;">
                <span style="font-size: 1.1rem;">💡</span>
                投稿のヒント
              </h3>
              <ul style="list-style: none; padding: 0; margin: 0;">
                <li style="margin-bottom: 0.5rem; padding-left: 1.5rem; position: relative;">
                  <span style="position: absolute; left: 0;">✨</span>
                  今の気持ちや考えを自由に書いてみましょう
                </li>
                <li style="margin-bottom: 0.5rem; padding-left: 1.5rem; position: relative;">
                  <span style="position: absolute; left: 0;">✨</span>
                  短い文章でも大丈夫です
                </li>
                <li style="margin-bottom: 0.5rem; padding-left: 1.5rem; position: relative;">
                  <span style="position: absolute; left: 0;">✨</span>
                  絵文字も使えます 😊
                </li>
                <li style="margin-bottom: 0.5rem; padding-left: 1.5rem; position: relative;">
                  <span style="position: absolute; left: 0;">✨</span>
                  他の人の投稿も見てみましょう
                </li>
              </ul>
            </div>
          </div>
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

button:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(29, 161, 242, 0.3);
  transition: all 0.3s ease;
}

button:disabled {
  background-color: #cbd5e1 !important;
  cursor: not-allowed;
  transform: none;
  box-shadow: none;
}

textarea:focus {
  outline: none;
  border-color: #1da1f2;
  box-shadow: 0 0 0 3px rgba(29, 161, 242, 0.1);
}

textarea:disabled {
  background-color: #f1f5f9;
  cursor: not-allowed;
}
</style>
