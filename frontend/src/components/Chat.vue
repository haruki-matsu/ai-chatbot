<template>
  <div class = "chat-container">
    <h2 class=" title">チャットボット</h2>
    <div class="messages">
      <div v-for="(msg, i) in messages" :key="i" class="message">
        <div class="bubble"><strong>{{ msg.role}}:</strong> {{ msg.content }}</div>
      </div>
    </div>
    <div class="input-area">
    <input v-model="input" @keyup.enter="sendMessage" placeholder="メッセージを入力" />
    <button @click="sendMessage">送信</button>
  </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const input = ref('')
const messages = ref([])

const sendMessage = async () => {
  if (!input.value.trim()) return

  messages.value.push({ role: 'あなた', content: input.value })

  const userMessage = input.value
  input.value = ''

    try {
      const res = await fetch('http://localhost:8000/chat', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ message: userMessage }),
      })

      const data = await res.json()
      messages.value.push({ role: 'ai', content: data.response })

  } catch (err) {
      console.error('エラー:', err)
      messages.value.push({ role: 'ai', content: 'エラーが発生しました。' })
  }

}
</script>
<style scoped>
.chat-container {
  width: 600px;
  margin: 40px auto;
  padding: 20px;
  background: #f4f6f8;
  border-radius: 12px;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1);
  font-family: 'Segoe UI', sans-serif;
}

.title {
  text-align: center;
  margin-bottom: 20px;
  color: #1e1e2f;
}

.messages {
  height: 300px;
  overflow-y: auto;
  background: #fff;
  padding: 10px;
  border-radius: 8px;
  margin-bottom: 16px;
  border: 1px solid #ddd;
}

.message {
  margin: 8px 0;
  display: flex;
}

.message.user {
  justify-content: flex-end;
}

.message.ai {
  justify-content: flex-start;
}

.bubble {
  max-width: 75%;
  padding: 10px 14px;
  border-radius: 18px;
  background-color: #e0e0e0;
  color: #333;
  word-break: break-word;
}

.message.user .bubble {
  background-color: #007bff;
  color: white;
  border-bottom-right-radius: 4px;
}

.message.ai .bubble {
  background-color: #eeeeee;
  color: #222;
  border-bottom-left-radius: 4px;
}

.input-area {
  display: flex;
  gap: 8px;
}

.input-area input {
  flex: 1;
  padding: 10px;
  font-size: 1rem;
  border: 1px solid #ccc;
  border-radius: 6px;
  outline: none;
}

.input-area button {
  padding: 10px 16px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: background 0.3s;
}

.input-area button:hover {
  background-color: #0056b3;
}
</style>