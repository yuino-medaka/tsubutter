<script setup>
import { ref } from 'vue'
import { initializeDatabase, resetDatabase } from '../supabase.js'

const isInitializing = ref(false)
const isResetting = ref(false)
const message = ref('')
const messageType = ref('info')

const initDatabase = async () => {
  if (!confirm('データベースを初期化しますか？\n（いいねとコメントのテーブルが作成されます）')) {
    return
  }

  isInitializing.value = true
  message.value = ''
  
  try {
    const result = await initializeDatabase()
    
    if (result.success) {
      message.value = '✅ ' + result.message
      messageType.value = 'success'
    } else {
      message.value = '❌ ' + result.message
      messageType.value = 'error'
    }
  } catch (error) {
    message.value = '❌ 初期化エラー: ' + error.message
    messageType.value = 'error'
  } finally {
    isInitializing.value = false
  }
}

const resetDb = async () => {
  if (!confirm('⚠️ 本当にデータベースをリセットしますか？\n\nこの操作により、いいねとコメントのテーブルが削除されます。\nこの操作は取り消せません。')) {
    return
  }

  isResetting.value = true
  message.value = ''
  
  try {
    const result = await resetDatabase()
    
    if (result.success) {
      message.value = '✅ ' + result.message
      messageType.value = 'success'
    } else {
      message.value = '❌ ' + result.message
      messageType.value = 'error'
    }
  } catch (error) {
    message.value = '❌ リセットエラー: ' + error.message
    messageType.value = 'error'
  } finally {
    isResetting.value = false
  }
}

const clearMessage = () => {
  message.value = ''
}
</script>

<template>
  <div class="admin-panel">
    <div class="admin-header">
      <h3 class="admin-title">
        <svg class="admin-icon" viewBox="0 0 24 24" fill="currentColor">
          <path d="M12 2l3.09 6.26L22 9.27l-5 4.87 1.18 6.88L12 17.77l-6.18 3.25L7 14.14 2 9.27l6.91-1.01L12 2z"/>
        </svg>
        データベース管理
      </h3>
      <p class="admin-description">
        いいねとコメント機能のテーブルを管理します
      </p>
    </div>

    <div class="admin-actions">
      <button 
        @click="initDatabase"
        :disabled="isInitializing"
        class="admin-btn init-btn"
      >
        <svg class="btn-icon" viewBox="0 0 24 24" fill="currentColor">
          <path d="M19 13h-6v6h-2v-6H5v-2h6V5h2v6h6v2z"/>
        </svg>
        {{ isInitializing ? '初期化中...' : 'データベース初期化' }}
      </button>

      <button 
        @click="resetDb"
        :disabled="isResetting"
        class="admin-btn reset-btn"
      >
        <svg class="btn-icon" viewBox="0 0 24 24" fill="currentColor">
          <path d="M6 19c0 1.1.9 2 2 2h8c1.1 0 2-.9 2-2V7H6v12zM19 4h-3.5l-1-1h-5l-1 1H5v2h14V4z"/>
        </svg>
        {{ isResetting ? 'リセット中...' : 'データベースリセット' }}
      </button>
    </div>

    <div v-if="message" :class="['message', messageType]">
      <span class="message-text">{{ message }}</span>
      <button @click="clearMessage" class="close-message">
        <svg viewBox="0 0 24 24" fill="currentColor">
          <path d="M19 6.41L17.59 5 12 10.59 6.41 5 5 6.41 10.59 12 5 17.59 6.41 19 12 13.41 17.59 19 19 17.59 13.41 12z"/>
        </svg>
      </button>
    </div>

    <div class="admin-info">
      <h4>機能説明</h4>
      <ul>
        <li><strong>データベース初期化</strong>: いいねとコメントのテーブルを作成し、セキュリティポリシーを設定します</li>
        <li><strong>データベースリセット</strong>: いいねとコメントのテーブルを削除します（投稿は残ります）</li>
      </ul>
      
      <div class="warning">
        <strong>⚠️ 注意:</strong> リセット機能は開発・テスト用です。本番環境では使用しないでください。
      </div>
    </div>
  </div>
</template>

<style scoped>
.admin-panel {
  background: white;
  border-radius: 1rem;
  padding: 2rem;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.1);
  margin-top: 1rem;
}

.admin-header {
  text-align: center;
  margin-bottom: 2rem;
}

.admin-title {
  font-size: 1.5rem;
  color: #1a202c;
  margin: 0 0 0.5rem 0;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 0.5rem;
}

.admin-icon {
  width: 1.5rem;
  height: 1.5rem;
  color: #1da1f2;
}

.admin-description {
  color: #64748b;
  margin: 0;
}

.admin-actions {
  display: flex;
  gap: 1rem;
  justify-content: center;
  margin-bottom: 2rem;
  flex-wrap: wrap;
}

.admin-btn {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.75rem 1.5rem;
  border: none;
  border-radius: 0.5rem;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  min-width: 200px;
  justify-content: center;
}

.admin-btn:disabled {
  opacity: 0.6;
  cursor: not-allowed;
}

.init-btn {
  background: #10b981;
  color: white;
}

.init-btn:hover:not(:disabled) {
  background: #059669;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(16, 185, 129, 0.3);
}

.reset-btn {
  background: #ef4444;
  color: white;
}

.reset-btn:hover:not(:disabled) {
  background: #dc2626;
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(239, 68, 68, 0.3);
}

.btn-icon {
  width: 1.2rem;
  height: 1.2rem;
}

.message {
  padding: 1rem;
  border-radius: 0.5rem;
  margin-bottom: 1.5rem;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.message.success {
  background: #dcfce7;
  color: #166534;
  border: 1px solid #bbf7d0;
}

.message.error {
  background: #fef2f2;
  color: #dc2626;
  border: 1px solid #fecaca;
}

.message.info {
  background: #dbeafe;
  color: #1e40af;
  border: 1px solid #bfdbfe;
}

.message-text {
  flex: 1;
}

.close-message {
  background: none;
  border: none;
  cursor: pointer;
  padding: 0.25rem;
  border-radius: 0.25rem;
  color: inherit;
  opacity: 0.7;
  transition: opacity 0.2s ease;
}

.close-message:hover {
  opacity: 1;
}

.close-message svg {
  width: 1rem;
  height: 1rem;
}

.admin-info {
  background: #f8fafc;
  padding: 1.5rem;
  border-radius: 0.75rem;
  border-left: 4px solid #1da1f2;
}

.admin-info h4 {
  margin: 0 0 1rem 0;
  color: #1a202c;
  font-size: 1.1rem;
}

.admin-info ul {
  margin: 0 0 1rem 0;
  padding-left: 1.5rem;
}

.admin-info li {
  margin-bottom: 0.5rem;
  color: #374151;
}

.warning {
  background: #fef3c7;
  border: 1px solid #fde68a;
  color: #92400e;
  padding: 1rem;
  border-radius: 0.5rem;
  margin-top: 1rem;
}

@media (max-width: 768px) {
  .admin-panel {
    padding: 1.5rem;
  }
  
  .admin-actions {
    flex-direction: column;
    align-items: stretch;
  }
  
  .admin-btn {
    min-width: auto;
  }
  
  .admin-info {
    padding: 1rem;
  }
}
</style>
