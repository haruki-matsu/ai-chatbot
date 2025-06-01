<template>
  <div>
    <h2>チャットボット</h2>
    <div v-for="(msg, i) in messages" :key="i">
      <strong>{{ msg.role }}:</strong> {{ msg.content }}
    </div>
    <input v-model="input" @keyup.enter="sendMessage" placeholder="メッセージを入力" />
    <button @click="sendMessage">送信</button>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const input = ref('')
const messages = ref([])

function sendMessage() {
  if (!input.value.trim()) return
  messages.value.push({ role: 'あなた', content: input.value })

  // ダミーの応答（API連携は後ほど）
  messages.value.push({ role: 'AI', content: `「${input.value}」に対する応答が入る` })

  input.value = ''
}
</script>
