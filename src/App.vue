<template>
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
