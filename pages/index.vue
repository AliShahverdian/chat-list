<template>
  <div>
    <v-text-field label="Username" outlined dark v-model="username" />
    <v-btn @click="login" :disabled="isValid">login</v-btn>
  </div>
</template>
<script>
export default {
  data() {
    return {
      username: '',
      isValid: false,
    }
  },
  methods: {
    async login() {
      if (this.username === '') {
        this.isValid = true
      } else {
        const users = await this.$axios.$get('http://localhost:3000/users')
        const user = users.find((el) => el.email === this.username)
        const jsonString = JSON.stringify(user)
        localStorage.setItem('loginUser', jsonString)
        this.$router.push('/chatlist')
      }
    },
  },
}
</script>
