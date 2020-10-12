<template>
 <a target="_blank" class="list_item" :href="`https://www.twitch.tv/${stream.twitch}`">
     <div class="user">
         <div class="user-content">
             <img class="avatar" :src="stream.profileUrl" alt="">
             <p class="name">{{stream.display}}</p>
         </div>
         <div class="status" :class="{active : stream.online}"></div>
     </div>
     <div class="games">
         <p class="play">{{stream.game}}</p>
         <p class="viewers">{{stream.viewersAmount.number}}</p>
     </div>
 </a>
</template>

<script>
export default {
  props: ["stream"],
};
</script>

<style lang='scss'>

.list_item{
    display: flex;
    flex-direction: column;
    background-color: rgb(22, 22, 22);
    margin-bottom: 20px;
    border-radius: 10px;
    text-decoration: none;
}
.user{
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 20px 20px 0 20px;
}
.user-content{
    display: flex;
    align-items: center;
}
.status{
    width: 15px;
    height: 15px;
    background-color: rgb(80, 80, 80);
    display: block;
    border-radius: 100px;
    &.active{
        background-color: #ff0000;
    }
}

.avatar{
    width: 40px;
}

.name{
    margin-left: 20px;
    color: white;
}

.games{
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-top: 10px;
    padding: 10px 20px 20px 20px;
    color: white;
}

</style>