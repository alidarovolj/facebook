<template>
  <div class="container mx-auto">
    <div class="w-full h-80 bg-black rounded-xl"></div>
    <div class="flex items-center justify-between">
      <!-- <img class="h-20 w-20 object-cover" :src="activeUser[0].avatar" alt=""> -->
      {{ activeUser[0].avatar }}
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: "ProfileView",
  data() {
    return {
      users: null,
      currentUser: localStorage.getItem('loggedUser')
    }
  },
  computed: {
    activeUser() {
      return this.users.filter((e) => e.email === this.currentUser)
    }
  },
  async mounted() {
    let res = await axios.get('https://6282500ded9edf7bd882691b.mockapi.io/users')
    this.users = res.data
    // localStorage.clear()
  },
}
</script>