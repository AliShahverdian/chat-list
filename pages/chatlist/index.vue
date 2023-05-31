<template>
  <div>
    <div
      v-for="chat in chats"
      :key="chat"
      @click="goToChat(chat.id)"
      class="d-flex align-center mb-8 chat-container"
    >
      <img :src="chat.image" class="contact-avatar" />
      <div class="ml-10">
        <div class="text-h4 mb-4">{{ chat.contactName }}</div>
        <div>{{ chat.messages[chat.messages.length - 1].content }}</div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      user: {},
      chats: [],
    }
  },

  created() {
    if (process.browser) {
      const userObject = localStorage.getItem('loginUser')
      this.user = JSON.parse(userObject)
      this.chats = this.user.chats
    }
  },
  methods: {
    goToChat(id) {
      this.$router.push(`/chatlist/${id}`)
    },
  },
}
</script>
<style>
.contact-avatar {
  border-radius: 50%;
  width: 100px;
  height: 100px;
}
.chat-container {
  border: solid 5px gray;
  padding: 16px;
  border-radius: 16px;
}
</style>
