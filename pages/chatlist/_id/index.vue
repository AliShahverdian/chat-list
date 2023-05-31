<template>
  <div class="chatroom-container">
    <div
      v-for="messege in chat.messages"
      :key="messege"
      class="white--text messege d-flex my-4 px-4"
      :class="messege.sender === userName ? 'right' : 'left'"
    >
      <div
        class="messege-content pa-4"
        :class="
          messege.sender === userName
            ? 'blue rounded-left'
            : 'red rounded-right'
        "
      >
        {{ messege.content }}
      </div>
    </div>
    <div class="text-input d-flex align-center">
      <v-text-field placeholder="write here" v-model="text" outlined dark />
      <div @click="sendText" class="mb-6 ml-4">
        <v-icon x-large>mdi-send-circle</v-icon>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      chat: {},
      userName: '',
      text: '',
      user: {},
      id: '',
    }
  },
  created() {
    if (process.browser) {
      const userObject = localStorage.getItem('loginUser')
      this.user = JSON.parse(userObject)
      this.userName = this.user.name
      this.id = this.$route.params.id
      this.chat = this.user.chats.find((el) => el.id === Number(this.id))
    }
  },
  methods: {
    async sendText() {
      const newMessage = {
        sender: this.userName,
        content: this.text,
        timestamp: new Date(),
      }
      this.user.chats[Number(this.id) - 1].messages.push(newMessage)
      try {
        await this.$axios.put(
          `http://localhost:3000/users/${this.user.id}`,
          this.user
        )
        const jsonString = JSON.stringify(this.user)
        localStorage.setItem('loginUser', jsonString)
      } catch (err) {
        console.log(err)
      }
    },
  },
}
</script>
<style>
.right {
  justify-content: start;
}
.left {
  justify-content: end;
}
.messege {
  display: block;
}
.messege-content {
  width: fit-content;
}
.rounded-right {
  border-radius: 16px 16px 0 16px;
}
.rounded-left {
  border-radius: 16px 16px 16px 0;
}
.chatroom-container {
  position: relative;
}
.text-input {
  position: fixed;
  bottom: 20px;
  right: 15%;
  width: 50%;
}
</style>
