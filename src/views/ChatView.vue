<template>
  <div class="my-10">
    <div class="container mx-auto">
      <p class="font-semibold mb-7">
        {{ chatUser[0].name + " " + chatUser[0].surname }}
      </p>
      <div class="bg-slate-100 p-5 rounded-lg">
        <div class="flex">
          <input
            v-model="message.text[0].msg"
            type="text"
            placeholder="Напишите сообщение"
            class="w-full p-3 rounded-full mx-1 border"
          />
          <button
            @click="sendMessage()"
            class="bg-main px-3 ml-3 text-white font-bold rounded-lg"
          >
            Отправить
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "ChatView",
  data() {
    return {
      users: null,
      posts: null,
      messages: null,
      currentUser: localStorage.getItem("loggedUser"),
      curID: this.$route.params.id,
      message: {
        personOne: null,
        personTwo: null,
        text: [
          {
            from: null,
            msg: null,
          },
        ],
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
    chatUser() {
      if (this.users === null) {
        return console.log("...");
      } else {
        return this.users.filter((e) => e.id === this.curID);
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
    currentChat() {
      if (this.messages != null) {
        return this.messages.filter(
          (e) =>
            e.personOne == this.currentUser ||
            (e.personOne == this.chatUser[0].email &&
              e.personTwo == this.chatUser[0].email) ||
            e.personTwo == this.currentUser
        );
      } else {
        return console.log("no messages");
      }
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

    let messages = await axios.get(
      "https://6282500ded9edf7bd882691b.mockapi.io/messages"
    );
    this.messages = messages.data;

    this.message.text[0].from = this.activeUser[0].email;
    this.message.personOne = this.currentUser;
    this.message.personTwo = this.chatUser[0].email;
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
    async sendMessage() {
      if (this.currentChat.length <= 0) {
        await axios.post(
          "https://6282500ded9edf7bd882691b.mockapi.io/messages",
          this.message
        );
        console.log("отправлено");
      } else {
        this.currentChat[0].text.push(this.message.text[0]);
        await axios.put(
          "https://6282500ded9edf7bd882691b.mockapi.io/messages/" +
            this.currentChat[0].id,
          this.currentChat[0]
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
