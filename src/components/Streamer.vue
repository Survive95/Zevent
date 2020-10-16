<template>
  <div @click="show = !show" class="list_item">
    <div class="user">
      <div class="user-content">
        <img class="avatar" :src="stream.profileUrl" alt="" />
        <h3 class="name">{{ stream.display }}</h3>
        <button @click.prevent.stop="addTwitch" class="addTwitch"><i class="fas fa-plus"></i></button>
      </div>
      <div class="status" :class="{ active: stream.online }"></div>
    </div>
    <div class="games">
      <h4 class="play">{{ stream.game }}</h4>
      <p v-if="stream.viewersAmount.formatted != 0" class="viewers">
        {{ stream.viewersAmount.formatted }}
        <i class="fas fa-user-alt"></i>
      </p>
    </div>
    <a
      class="link"
      target="_blank"
      :href="`https://www.twitch.tv/${stream.twitch}`"
      ><i class="fab fa-twitch"></i
    ></a>
    <ul v-if="show && stream.donationGoal != ''" class="donations_list">
      <h1 class="money">{{stream.donationGoal.donationAmount.formatted}}</h1>
      <li :key="index" v-for="(goal, index) in stream.donationGoal.goals">
        <p>{{ goal.title }}</p>
        <p class="number">{{ goal.amountRequired.formatted }}</p>
        <meter
          :min="
            index > 0
              ? stream.donationGoal.goals[index - 1].amountRequired.number
              : 0
          "
          :value="stream.donationGoal.donationAmount.number"
          :max="goal.amountRequired.number"
          class="pregress"
        ></meter>
      </li>
    </ul>
    <i
      v-if="stream.donationGoal != '' && show === false"
      class="fas fa-angle-down arrow"
    ></i>
    <i
      v-if="stream.donationGoal != '' && show === true"
      class="fas fa-angle-up arrow"
    ></i>
  </div>
</template>

<script>

export default {
  props: ["stream"],
  data() {
    return {
      show: false,
    };
  },
  methods : {
    addTwitch(){
      this.$emit('addTwitch', this.stream.twitch)
    }
  }
};
</script>

<style lang='scss'>
.list_item {
  * {
    user-select: none;
  }
  display: flex;
  flex-direction: column;
  background-color: transparent;
  border: 1px solid var(--borderColor);
  margin-bottom: 20px;
  border-radius: 5px;
  text-decoration: none;
  position: relative;
  transition: 0.2s;
  &:hover {
    background-color: var(--grey_color);
  }
}

.addTwitch{
  height: 30px;
  width: 30px;
  background-color: #9147FF;
  color: white;
  border: none;
  border-radius: 100px;
  margin-left: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  outline: none;
  cursor: pointer;
  &:hover{
    background-color: #7b3bdb;
  }
}

.money{
  color: var(--color);
  text-align: center;
}

.viewers {
  i {
    margin-left: 5px;
    font-size: 16px;
  }
}

.link {
  padding: 0 20px;
  margin-bottom: 10px;
  color: var(--color);
  font-size: 20px;
  width: 30px;
  &:hover {
    color: #4bba31;
  }
}
.user {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 20px 20px 0 20px;
}
.user-content {
  display: flex;
  align-items: center;
}
.status {
  width: 15px;
  height: 15px;
  background-color: rgb(80, 80, 80);
  display: block;
  border-radius: 100px;
  &.active {
    background-color: #4bba31;
  }
}

.arrow {
  text-align: center;
  margin-bottom: 10px;
  color: var(--color);
}

.avatar {
  width: 50px;
  height: 50px;
  object-fit: cover;
  border-radius: 5px;
}

.name {
  margin-left: 20px;
  color: var(--color);
}

.games {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: space-between;
  margin-top: 10px;
  padding: 10px 20px 20px 20px;
  color: var(--color);
}

.donations_list {
  list-style: none;
  padding: 0 20px 20px 20px;
  animation: list 0.5s;
  transform-origin: top;
  li {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin: 15px 0;
    color: var(--color);
    background-color: transparent;
    border: 1px solid #4bba31;
    // border-radius: 5px;
    padding: 10px;
    position: relative;
    overflow: hidden;
    p {
      position: relative;
      z-index: 10;
    }
    .number {
      text-align: right;
      white-space: nowrap;
      margin-left: 20px;
    }
  }
  .pregress {
    position: absolute;
    top: 0;
    left: 0;
    border-radius: 0;
    height: 100%;
    width: 100%;
    &::-webkit-meter-bar {
      border-radius: 0;
      background: transparent;
      height: 1000px;
      border: none;
    }
    &::-webkit-meter-optimum-value{
      background-color: #4bba31;
    }
  }
}

@keyframes list {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
</style>