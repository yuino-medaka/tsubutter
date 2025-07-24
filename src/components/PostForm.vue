<script setup>
import { ref } from 'vue'
import { supabase } from '../supabase.js'

const content = ref('')
const isSubmitting = ref(false)
const message = ref('')

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
    const { data, error } = await supabase
      .from('posts')
      .insert([
        {
          content: content.value.trim(),
          user_id: (await supabase.auth.getUser()).data.user?.id || null
        }
      ])
      .select()

    if (error) throw error

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
  <div class="post-form">
    <h2 class="form-title">
      <span class="title-icon">✍️</span>
      気持ちを投稿しよう
    </h2>
    
    <div class="form-container">
      <div class="input-group">
        <textarea
          v-model="content"
          placeholder="今の気持ちを280文字以内で投稿してください..."
          class="content-input"
          :maxlength="280"
          :disabled="isSubmitting"
          @input="clearMessage"
        ></textarea>
        
        <div class="char-count" :class="{ 'char-count-warning': content.length > 250 }">
          {{ content.length }}/280
        </div>
      </div>
      
      <div class="form-actions">
        <button
          @click="cancelPost"
          :disabled="isSubmitting"
          class="btn btn-secondary cancel-btn"
        >
          <span class="btn-icon">❌</span>
          キャンセル
        </button>
        
        <button
          @click="submitPost"
          :disabled="isSubmitting || !content.trim()"
          class="btn btn-primary submit-btn"
        >
          <span v-if="isSubmitting" class="btn-icon">⏳</span>
          <span v-else class="btn-icon">📤</span>
          <span v-if="isSubmitting">投稿中...</span>
          <span v-else>投稿する</span>
        </button>
      </div>
      
      <div v-if="message" class="message" :class="{ error: message.includes('失敗') }">
        <span class="message-icon">{{ message.includes('完了') ? '✅' : '⚠️' }}</span>
        {{ message }}
      </div>
    </div>
    
    <div class="form-tips">
      <h3>
        <span class="tips-icon">💡</span>
        投稿のヒント
      </h3>
      <ul>
        <li>今の気持ちや考えを自由に書いてみましょう</li>
        <li>短い文章でも大丈夫です</li>
        <li>絵文字も使えます 😊</li>
        <li>他の人の投稿も見てみましょう</li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
.post-form {
  max-width: 600px;
  margin: 0 auto;
}

.form-title {
  font-size: 1.8rem;
  color: #1a202c;
  margin-bottom: 2rem;
  text-align: center;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
}

.title-icon {
  font-size: 1.5rem;
}

.form-container {
  background: #f8fafc;
  border-radius: 1rem;
  padding: 2rem;
  margin-bottom: 2rem;
}

.input-group {
  position: relative;
  margin-bottom: 1.5rem;
}

.content-input {
  width: 100%;
  min-height: 120px;
  padding: 1rem;
  border: 2px solid #e2e8f0;
  border-radius: 0.75rem;
  font-size: 1rem;
  font-family: inherit;
  resize: vertical;
  transition: border-color 0.3s ease;
}

.content-input:focus {
  outline: none;
  border-color: #1da1f2;
  box-shadow: 0 0 0 3px rgba(29, 161, 242, 0.1);
}

.content-input:disabled {
  background-color: #f1f5f9;
  cursor: not-allowed;
}

.char-count {
  position: absolute;
  bottom: 0.5rem;
  right: 0.5rem;
  font-size: 0.875rem;
  color: #64748b;
  background: rgba(255, 255, 255, 0.9);
  padding: 0.25rem 0.5rem;
  border-radius: 0.25rem;
  transition: color 0.3s ease;
}

.char-count-warning {
  color: #f59e0b;
  font-weight: 600;
}

.form-actions {
  display: flex;
  justify-content: center;
  gap: 1rem;
  flex-wrap: wrap;
}

.btn {
  padding: 0.75rem 2rem;
  font-size: 1.1rem;
  font-weight: 600;
  border-radius: 2rem;
  min-width: 150px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
  transition: all 0.3s ease;
  border: none;
  cursor: pointer;
}

.btn-primary {
  background: #1da1f2;
  color: white;
}

.btn-secondary {
  background: #6b7280;
  color: white;
}

.submit-btn:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(29, 161, 242, 0.3);
}

.cancel-btn:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(107, 114, 128, 0.3);
}

.submit-btn:disabled {
  background-color: #cbd5e1;
  cursor: not-allowed;
  transform: none;
  box-shadow: none;
}

.cancel-btn:disabled {
  background-color: #cbd5e1;
  cursor: not-allowed;
  transform: none;
  box-shadow: none;
}

.btn-icon {
  font-size: 1rem;
}

.message {
  margin-top: 1rem;
  padding: 0.75rem 1rem;
  border-radius: 0.5rem;
  text-align: center;
  font-weight: 500;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
}

.message-icon {
  font-size: 1.1rem;
}

.message:not(.error) {
  background-color: #dcfce7;
  color: #166534;
  border: 1px solid #bbf7d0;
}

.message.error {
  background-color: #fef2f2;
  color: #dc2626;
  border: 1px solid #fecaca;
}

.form-tips {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border-radius: 1rem;
  padding: 1.5rem;
}

.form-tips h3 {
  margin-bottom: 1rem;
  font-size: 1.2rem;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.tips-icon {
  font-size: 1.1rem;
}

.form-tips ul {
  list-style: none;
  padding: 0;
}

.form-tips li {
  margin-bottom: 0.5rem;
  padding-left: 1.5rem;
  position: relative;
}

.form-tips li::before {
  content: "✨";
  position: absolute;
  left: 0;
}

/* レスポンシブ対応 */
@media (max-width: 768px) {
  .form-container {
    padding: 1.5rem;
  }
  
  .form-title {
    font-size: 1.5rem;
  }
  
  .content-input {
    min-height: 100px;
  }
  
  .form-actions {
    flex-direction: column;
    align-items: center;
  }
  
  .btn {
    width: 100%;
    max-width: 300px;
  }
}

@media (max-width: 480px) {
  .form-container {
    padding: 1rem;
  }
  
  .form-tips {
    padding: 1rem;
  }
  
  .form-actions {
    gap: 0.75rem;
  }
  
  .btn {
    padding: 0.75rem 1.5rem;
    font-size: 1rem;
  }
}
</style>
