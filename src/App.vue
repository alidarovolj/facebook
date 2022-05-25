<template>
  <div id="app">
    <regLog v-if="currentUser == null" />
    <div v-else>
      <div class="fixed border-r py-5 h-full bg-white w-14 left-0 top-0">
        <i
          @click="$router.push({ name: 'profile' })"
          class="
            fa-solid fa-user
            block
            text-center text-2xl
            my-2
            hover:cursor-pointer hover:text-main
          "
        ></i>
        <i
          @click="$router.push({ name: 'home' })"
          class="
            fa-solid fa-house
            block
            text-center text-2xl
            my-2
            hover:cursor-pointer hover:text-main
          "
        ></i>
        <i
          @click="$router.push({ name: 'messages' })"
          class="
            fa-solid fa-message
            block
            text-center text-2xl
            my-2
            hover:cursor-pointer hover:text-main
          "
        ></i>
      </div>
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