<template>
  <div class="home bg-slate-100 py-10">
    <div class="container mx-auto">
      <div class="bg-white p-3 mb-4 rounded-lg" v-for="(post, index) of friendsPost" :key="post.id">
        <p class="font-semibold">{{ post.user_login }}</p>
        <p>{{ post.createdAt }}</p>
        <p>{{ post.text }}</p>
        <p @click="setLike(index)">{{ post.likes.length }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "HomeView",
  data() {
    return {
      users: null,
      posts: null,
      currentUser: localStorage.getItem("loggedUser"),
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
      return this.users.filter((i) =>
        this.activeUser[0].friends.includes(i.email)
      );
    },
    friendsPost() {
      if(this.posts != null) {
        let a = this.posts.filter((post) => this.activeUser[0].friends.includes(post.user_login))
        return a
      } else {
        return console.log('sss')
      }
    }
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
  },
};
</script>
