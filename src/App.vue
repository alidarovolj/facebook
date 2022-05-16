<template>
  <div id="app">
    <regLog />
    <router-view/>
  </div>
</template>

<script>
import regLog from './components/RegistrationLogin.vue'
export default {
  components: {
    regLog
  }
}
</script>