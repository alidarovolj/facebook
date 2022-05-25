<template>
  <div class="my-10">
    <div class="container mx-auto">
      <div class="w-1/2 mx-auto bg-slate-100 p-5 rounded-lg">
        <h1 class="text-xl font-semibold">Сообщения</h1>
        <div
          class="my-4 flex items-center justify-between"
          v-for="user of userFriends"
          :key="user.id"
        >
          <div class="flex items-center">
            <img
              class="w-8 h-8 rounded-full object-cover mr-3"
              :src="user.avatar"
              alt=""
            />
            <p>{{ user.name + " " + user.surname }}</p>
          </div>
          <p
            @click="$router.push({ name: 'chat', params: { id: user.id } })"
            class="text-sm font-semibold text-main hover:cursor-pointer"
          >
            Перейти в чат <i class="fa-solid fa-arrow-right"></i>
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "MessagesView",
  data() {
    return {
      users: null,
      posts: null,
      currentUser: localStorage.getItem("loggedUser"),
      message: {
        id: 1,
        first: {},
        second: {},
        text: "",
        date: "",
        status: false,
      },
    };
  },
  computed: {
    activeUser() {
      if (this.users === null) {
        return console.log("...");
      } else {
        return this.users.filter((e) => e.email === this.currentUser);
      }
    },
    userPosts() {
      if (this.posts === null) {
        return console.log("...");
      } else {
        return this.posts.filter((e) => e.user_login === this.currentUser);
      }
    },
    userFriends() {
      if (this.users != null) {
        return this.users.filter((i) =>
          this.activeUser[0].friends.includes(i.email)
        );
      } else {
        return console.log("no");
      }
    },
    friendsPost() {
      if (this.posts) {
        let arr = this.posts.filter((e) =>
          this.activeUser[0].friends.includes(e.user_login)
        );
        let arrWithInfo = arr.map((post) => {
          post.info = this.userFriends.find(
            (user) => post.user_login == user.email
          );
          return post;
        });
        return arrWithInfo;
      }
      return [];
    },
    usersList() {
      return this.users.filter(
        (i) => !this.activeUser[0].friends.includes(i.email)
      );
    },
  },
  async created() {
    let res = await axios.get(
      "https://6282500ded9edf7bd882691b.mockapi.io/users"
    );
    this.users = res.data;

    let posts = await axios.get(
      "https://6282500ded9edf7bd882691b.mockapi.io/posts"
    );
    this.posts = posts.data;
  },
  methods: {
    async setLike(id) {
      let postID = this.friendsPost[id].id;
      if (this.friendsPost[id].likes.includes(this.currentUser)) {
        let myIndex = this.friendsPost[id].likes.indexOf(this.currentUser);
        this.friendsPost[id].likes.splice(myIndex, 1);
        await axios.put(
          "https://6282500ded9edf7bd882691b.mockapi.io/posts/" + postID,
          this.friendsPost[id]
        );
      } else {
        console.log(postID);
        this.friendsPost[id].likes.push(this.currentUser);
        await axios.put(
          "https://6282500ded9edf7bd882691b.mockapi.io/posts/" + postID,
          this.friendsPost[id]
        );
      }
    },
    async addToFriends(email) {
      this.activeUser[0].friends.push(email);
      await axios.put(
        "https://6282500ded9edf7bd882691b.mockapi.io/users/" +
          this.activeUser[0].id,
        this.activeUser[0]
      );
      this.$router.go();
    },
  },
};
</script>
