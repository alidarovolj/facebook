<template>
  <div id="app">
    <regLog v-if="currentUser == null" />
    <div v-else>
      <p
        @click="logout()"
        class="
          absolute
          top-5
          right-5
          p-2
          rounded-lg
          bg-slate-200
          text-black text-lg
          font-bold
          hover:cursor-pointer hover:bg-black hover:text-white
        "
      >
        Выйти
      </p>
      <router-view />
    </div>
  </div>
</template>

<script>
import regLog from "./components/RegistrationLogin.vue";
export default {
  components: {
    regLog,
  },
  data() {
    return {
      currentUser: localStorage.getItem("loggedUser"),
    };
  },
  methods: {
    logout() {
      localStorage.removeItem("loggedUser");
      this.$router.go();
    },
  },
};
</script>