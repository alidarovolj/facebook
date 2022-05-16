<template>
  <div id="app">
    <regLog v-if="currentUser == null"/>
    <router-view v-else />
  </div>
</template>

<script>
import regLog from './components/RegistrationLogin.vue'
export default {
  components: {
    regLog
  },
  data() {
    return {
      currentUser: localStorage.getItem('loggedUser')
    }
  }
}
</script>