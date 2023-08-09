<template>
  <div class="sign">
    <p>請按下鍵盤的'上','下','左','右'鍵開始遊戲並控制蛇的移動</p>
  </div>
  <SnakeGame 
    :key="changeTime"
  />
  <div class="button-box">
    <button @click="reStart">重新開始</button>
  </div>
</template>

<script lang="ts" setup>
import SnakeGame from './components/SnakeGame.vue';
import { ref } from 'vue';
import type { Ref } from 'vue';

let changeTime: Ref<number> = ref(new Date().getTime())
const reStart = ():void => {
  changeTime.value = new Date().getTime()
}
</script>

<style lang="scss">
.button-box{
  display: flex;
  justify-content: center;
}
.sign{
  display: flex;
  justify-content: center;
  font-size: 20px;
  color: red;
  padding-top: 30px;
  p{
    padding: 10px 20px;
    background: rgb(255, 186, 186);
    border-radius: 10px;
  }
}
button{
  background: rgb(88, 227, 255);
  padding: 5px 10px;
  border-radius: 5px;
  border: 1px skyblue solid;
  color: rgb(0, 109, 242);
  cursor: pointer;
  font-size: 20px;
}
</style>
