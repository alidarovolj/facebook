<template>
  <div>
    <div class="bg-slate-100">
      <div class="container mx-auto">
        <div class="w-full h-80 rounded-xl">
          <img class="w-full h-full object-cover" :src="activeUser[0].background" alt="">
        </div>
        <div class="flex items-center justify-between py-3">
          <div class="flex items-center">
            <img class="h-20 w-20 object-cover mr-4" :src="activeUser[0].avatar" alt="">
            <div>
              <p class="text-3xl font-bold">{{ activeUser[0].name + " " + activeUser[0].surname }}</p>
              <p class="font-bold">Друзья: {{ activeUser[0].friends.length }}</p>
            </div>
          </div>
          <p class="font-bold bg-slate-200 p-2 rounded-lg hover:cursor-pointer">Редактировать профиль</p>
        </div>
      </div>
    </div>
    <div class="container mx-auto mt-5">
      <div class="flex items-start justify-between">
        <div class="w-1/3 bg-slate-100 p-4 rounded-lg">
        <h2 class="text-xl font-bold mb-4">Краткая информация</h2>
          <p>Работа: 
            <span class="font-bold" v-if="activeUser[0].profileInfo.working != ''">{{ activeUser[0].profileInfo.working }}</span>
            <span class="font-bold" v-if="activeUser[0].profileInfo.working === ''">Данные отсутствуют</span></p>
          <p>Информация о себе: 
            <span class="font-bold" v-if="activeUser[0].profileInfo.aboutMe != ''">{{ activeUser[0].profileInfo.aboutMe }}</span>
            <span class="font-bold" v-if="activeUser[0].profileInfo.aboutMe === ''">Данные отсутствуют</span></p>
          <p>Место проживания: 
            <span class="font-bold" v-if="activeUser[0].profileInfo.city != ''">{{ activeUser[0].profileInfo.city }}</span>
            <span class="font-bold" v-if="activeUser[0].profileInfo.city === ''">Данные отсутствуют</span></p>
        </div>
        <div class="w-2/3">
          <!-- <div v-for="post of posts">

          </div> -->
        </div>
      </div>
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
      posts: null,
      currentUser: localStorage.getItem('loggedUser')
    }
  },
  computed: {
    activeUser() {
      if(this.users === null) {
        return console.log('...')
      } else {
        return this.users.filter((e) => e.email === this.currentUser)
      }
    },
    userPosts() {
      if(this.posts === null) {
        return console.log('...')
      } else {
        return this.posts.filter((e) => e.user_login === this.currentUser)
      }
    }
  },
  async created() {
    let res = await axios.get('https://6282500ded9edf7bd882691b.mockapi.io/users')
    this.users = res.data

    let posts = await axios.get('https://6282500ded9edf7bd882691b.mockapi.io/posts')
    this.posts = posts.data
    // localStorage.clear()
  },
}
</script>