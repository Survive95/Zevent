<template>
  <div id="app">
    <header>
      <div class="header-content">
        <h1>
          <img class="logo" src="../src/assets/logo.png" alt="zevent_logo" />
        </h1>
        <h1 v-if="streamersData !== null" class="globalMoney">{{streamersData.donationAmount.formatted}}</h1>
        <button class="dark_button" @click="toogleDark">
          <i v-if="dark === false" class="far fa-lightbulb"></i>
          <i v-if="dark === true" class="fas fa-lightbulb"></i>
          </button>
      </div>
    </header>
    <main>
      <h1 v-if="streamersData !== null" class="section_title">En live <span>{{streamersData.live.filter((o) => o.online === true).length}}</span></h1>
      <form @submit.prevent="" class="formTwitch">
        <input v-model="twitch" ref="inputTwitch" value="https://www.multitwitch.tv/" type="text">
        <button @click="openExt" class="external"><i class="fas fa-external-link-alt"></i></button>
      </form>
      <ul v-if="streamersData !== null" class="streamers_list">
        <Streamer
          @addTwitch="multiTwitch"
          :key="index"
          v-for="(data, index) in streamersData.live.filter(
            (o) => o.online === true
          )"
          :stream="data"
        ></Streamer>
      </ul>
      <h1 v-if="streamersData !== null" class="section_title">Hors ligne <span>{{streamersData.live.filter((o) => o.online === false).length}}</span></h1>
      <ul v-if="streamersData !== null" class="streamers_list">
        <Streamer
          @addTwitch="multiTwitch"
          :key="index"
          v-for="(data, index) in streamersData.live.filter(
            (o) => o.online === false
          )"
          :stream="data"
        ></Streamer>
      </ul>
      <h1 v-if="streamersData !== null && showMoney" class="globalCagnote">{{streamersData.donationAmount.formatted}}</h1>
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
      dark : false,
      showMoney : false,
      twitch : 'https://www.multitwitch.tv/'
    };
  },
  watch : {
    twitch : function(value){
      value.includes('https://www.multitwitch.tv/') ? "" : this.twitch = "https://www.multitwitch.tv/"

    }
  },
  methods : {

    openExt(){
      window.open(this.twitch, '_blank')
      this.twitch = ''
    },

    multiTwitch(val){
      console.log(val);
      let oldVal = this.$refs.inputTwitch.value
      let newVal = oldVal + val + "/"
      this.twitch = newVal
    },
    toogleDark(){
      let html = document.querySelector('html')
      if(html.getAttribute('data-theme') === 'dark'){
        html.setAttribute('data-theme', 'white')
        this.dark = false  
      }
      else{
        html.setAttribute('data-theme', 'dark')
        this.dark = true
      }
    }
  },
  mounted() {
    setInterval(() => {
      console.log("Update !!!");
      axios.get("https://zevent.fr/api/data.json").then((res) => {
        this.streamersData = res.data;
      });
    }, 60000);
    axios.get("https://zevent.fr/api/data.json").then((res) => {
      this.streamersData = res.data;
    });
    window.addEventListener('scroll', () => {
      if(window.scrollY > 0){
        this.showMoney = true
      }
      else{
        this.showMoney = false
      }
    })
  },
};
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: Arial, Helvetica, sans-serif;
  user-select: none;
}

html {
  --bg_color:white;
  --color:rgb(49, 49, 49);
  --grey_color:#f7f7f7;
  --borderColor:#e6e6e6;
}

html[data-theme='dark'] {
  --bg_color:#161616;
  --color:white;
  --grey_color:#1c1c1c;
  --borderColor:#1c1c1c;
}

body {
  background-color: var(--bg_color);
}

.external{
  background-color: #9147FF;
  color: white;
  height: 40px;
  width: 40px;
  border: none;
  margin-left: 10px;
  outline: none;
  cursor: pointer;
  &:hover{
    background-color: #7b3bdb;
  }
}

.formTwitch{
  margin-bottom: 20px;
  display: flex;
  align-items: center;
  position: sticky;
  top: 0;
  background-color: var(--bg_color);
  z-index: 9999999;
  input{
    height: 40px;
    width: 100%;
    padding-left: 10px;
    background-color: transparent;
    border: 1px solid var(--borderColor);
    color: var(--color);
    outline: none;
    cursor: text;
    font-size: 16px;
  }
}

.globalCagnote{
  position: fixed;
  right: 40px;
  bottom: 40px;
  background-color: #4bba31;
  padding: 10px 30px;
  border-radius: 5px;
  color: white;
  z-index: 9999999;
  animation: popin 1s ;
}

@keyframes popin{
  0%{opacity: 0;}
  100%{opacity: 1;}
}

.user_live{
  position: absolute;
  top: 0;
  right: 0;
  text-align: right;
}

.dark_button{
  font-size: 20px;
  background-color: transparent;
  height: 40px;
  width: 40px;
  margin-left: 20px;
  color: var(--color);
  outline: none;
  border: none;
  cursor: pointer;
  &:hover{
    background-color: var(--borderColor);
  }
}

header {
  .header-content {
    padding: 40px 0;
    width: 60vw;
    margin-inline-start: auto;
    margin-inline-end: auto;
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    justify-content: space-between;
    @media (max-width: 640px) {
      align-items: center;
      justify-content: center;
    }
  }
  .globalMoney{
    color: var(--color);
    @media(max-width: 640px){
      margin-left: 20px;
    }
  }
  .logo {
    width: 100px;
  }
  nav {
    display: flex;
    flex-wrap: wrap;
    margin-left: 20px;
    @media (max-width: 640px) {
      margin: 0;
      margin-top: 20px;
    }
    a {
      padding: 10px 10px;
      border-radius: 5px;
      color: white;
      text-decoration: none;
      &:hover {
        color: #4bba31;
      }
    }
  }
}

main {
  width: 60vw;
  margin-inline-start: auto;
  margin-inline-end: auto;
  @media (max-width: 640px) {
    width: 90vw;
  }
  .streamers_list {
    list-style: none;
    margin-bottom: 80px;
    // display: grid;
    // grid-template-columns: repeat(2, 1fr);
    // gap: 20px;
    // align-items: flex-start;
  }
  .main_item {
    padding: 0 0 20px 0;
    margin-bottom: 20px;
    .letter {
      color: var(--color);
      margin-bottom: 20px;
      display: flex;
      align-items: center;
      &::after {
        content: "";
        width: 100%;
        height: 1px;
        background-color: #4bba31;
        display: block;
        margin-left: 20px;
      }
    }
  }
  .section_title {
    color: var(--color);
    margin: 0px 0 20px 0;
    padding: 0 5px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    span{
      margin-left: 10px;
      font-weight: 500;
      color: var(--color);
    }
  }
}
</style>
