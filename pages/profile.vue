<template>
  <div>
    <v-card v-if="showCard" class="mx-auto" max-width="400">
      <v-img
        class="align -end text-white"
        height="200"
        :src="user.avatar"
        cover
      >
      </v-img>

      <v-card-text class="text-center">
        <div>{{ user.name }}</div>
        <div>{{ user.email }}</div>
        <div>{{ user.mobile }}</div>
      </v-card-text>

      <v-card-actions class="d-flex justify-center">
        <v-btn color="orange" @click="editProfile">Edit </v-btn>
      </v-card-actions>
    </v-card>
    <v-form v-else @submit.prevent="editForm">
      <v-text-field v-model="user.name" outlined />
      <v-text-field v-model="user.email" outlined />
      <v-text-field v-model="user.mobile" outlined />
      <v-btn type="submit">submit</v-btn>
    </v-form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      user: {},
      showCard: true,
    }
  },
  created() {
    if (process.browser) {
      const userProfile = localStorage.getItem('loginUser')
      this.user = JSON.parse(userProfile)
    }
  },
  methods: {
    editProfile() {
      this.showCard = false
    },
    async editForm() {
      try {
        await this.$axios.put(
          `http://localhost:3000/users/${this.user.id}`,
          this.user
        )
      } catch (err) {
        console.log(err)
      }
    },
  },
}
</script>
