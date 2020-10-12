<template>
  <div id="app">
    <header>
      <div class="header-content">
        <h1>
          <img
            class="logo"
            src="../src/assets/logo.png"
            alt="zevent_logo"
          />
        </h1>
        <nav>
          <a :key="letter" v-for="letter in letters" :href="`#${letter}`">{{letter}}</a>
        </nav>
      </div>
    </header>
    <main>
      <div :id="letter" class="main_item" :key="letter" v-for="letter in letters">
        <ul v-if="streamersData !== null" class="streamers_list">
          <h3 class="letter">{{letter}}</h3>
          <Streamer
            :key="index"
            v-for="(data, index) in streamersData.live.filter(
              (w) => w.display.charAt(0) === letter
            )"
            :stream="data"
          ></Streamer>
        </ul>
      </div>
    </main>
  </div>
</template>

<script>
import Streamer from "./components/Streamer";
import axios from "axios";

export default {
  components: {
    Streamer,
  },
  data() {
    return {
      streamersData: null,
      letters: [
        "A",
        "B",
        "C",
        "D",
        "E",
        "F",
        "G",
        "H",
        "I",
        "J",
        "K",
        "L",
        "M",
        "N",
        "O",
        "P",
        "Q",
        "R",
        "S",
        "T",
        "U",
        "V",
        "W",
        "X",
        "Y",
        "Z",
      ],
    };
  },
  mounted() {
    setInterval(() => {
      console.log("Update !!!!");
      axios.get("https://zevent.fr/api/data.json").then((res) => {
        this.streamersData = res.data;
      });
    }, 60000);
    axios.get("https://zevent.fr/api/data.json").then((res) => {
      this.streamersData = res.data;
    });
  },
};
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}

body {
  background-color: #191919;
}

header {
  .header-content {
    padding: 40px 0;
    width: 80vw;
    margin-inline-start: auto;
    margin-inline-end: auto;
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    justify-content: space-between;
    @media (max-width: 460px){
      align-items: center;
      justify-content: center;
    }
  }
  .logo {
    width: 100px;
  }
  nav{
    display: flex;
    flex-wrap: wrap;
    margin-left: 20px;
    @media (max-width: 460px){
      margin: 0;
      margin-top: 20px;
    }
    a{
      padding: 10px 10px;
      border-radius: 5px;
      color: white;
      text-decoration: none;
      &:hover{
        color: #4bba31;
      }
    }
  }
}

main {
  width: 80vw;
  margin-inline-start: auto;
  margin-inline-end: auto;
  .streamers_list {
    list-style: none;
  }
  .main_item{
    padding: 0 0 20px 0;
    margin-bottom: 20px;
    .letter{
      color: white;
      margin-bottom: 20px;
      display: flex;
      align-items: center;
      &::after{
        content: '';
        width: 100%;
        height: 1px;
        background-color: #4bba31;
        display: block;
        margin-left: 20px;
      }
    }
  }
}
</style>
