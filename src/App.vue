<template>
  <div id="app">
    <h1>COMMENTS</h1>
    <ul v-for="(comment, idx) in fileData" :key="idx">
      <Comment :comment="comment" :id="Date.now()" />
    </ul>
    <div id="loader">
      <div id="loading" v-show="loading"></div>
    </div>
  </div>
</template>

<script>
import jsonFile from "../feed 1.json";
import Comment from "./components/Comment.vue";
export default {
  name: "App",
  components: {
    Comment,
  },
  data() {
    return {
      fileData: [],
      count: 0,
      countRange: 10,
      loading: false,
    };
  },

  methods: {
    getComments() {
      for (this.count; this.count < jsonFile.length; this.count++) {
        if (this.countRange > jsonFile.length) break;
        if (this.count >= this.countRange) {
          this.countRange += 10;
          break;
        }
        console.log("count", this.count);
        this.fileData.push(jsonFile[this.count]);
      }
    },
    scroll() {
      window.onscroll = () => {
        const bottomScreen =
          document.documentElement.scrollTop + window.innerHeight ===
          document.documentElement.offsetHeight;
        console.log("json=", jsonFile.length, " ,count=");
        if (
          bottomScreen &&
          this.count <= jsonFile.length - 1 &&
          this.loading === false
        ) {
          console.log("start");
          this.loading = true;
          setTimeout(() => {
            console.log("timeout");
            this.getComments();
            this.loading = false;
          }, 4000);
          console.log("end");
        }
      };
    },
    addComment: () => {
      console.log(event);
    },
  },
  beforeMount() {
    this.getComments();
  },
  mounted() {
    this.scroll();
  },
};
</script>

<style>
body {
  background-color: honeydew;
  margin: 3%;
}

#loader {
  display: flex;
  justify-content: center;
  align-items: center;
}
#loading {
  border: 5px solid white;
  border-top: 5px solid rgba(0, 0, 255, 0.5);
  border-radius: 50%;
  width: 25px;
  height: 25px;
  animation: spin 1s linear infinite;
}
@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>
