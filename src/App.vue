<template>
  <div class="sign">
    <p>- 請按 '上' , '下' , '左' , '右' 鍵開始遊戲 - </p>
  </div>
  <div class="button-box">
    <button @click="changeMode">切換{{ isEasy? '嚴格' : '簡單' }}模式</button>
  </div>
  <SnakeGame 
    :key="changeTime"
    :isEasy="isEasy"
    @gameOver="gameOver"
  />
  <OverDialog 
    :reStart="reStart"
    :overString="overString"
    v-if="isGameOver"
  />
</template>

<script lang="ts" setup>
import SnakeGame from './components/SnakeGame.vue';
import OverDialog from './components/OverDialog.vue';
import { ref } from 'vue';
import type { Ref } from 'vue';

const changeTime: Ref<number> = ref(new Date().getTime())
const isGameOver: Ref<boolean> = ref(false)
const overString: Ref<string> = ref('')
const isEasy: Ref<boolean> = ref(true)

const reStart = ():void => {
  changeTime.value = new Date().getTime()
  isGameOver.value = false
}
const gameOver = (isOver: string):void => {
  overString.value = isOver
  isGameOver.value = true
}
const changeMode = ():void => {
  isEasy.value = !isEasy.value
}
</script>

<style lang="scss">
.button-box{
  display: flex;
  justify-content: center;
  button{
    padding: 10px 20px;
    background: rgb(255, 186, 186);
    border-radius: 10px;
    border: none;
    color: red;
  }
}
.sign{
  display: flex;
  justify-content: center;
  font-size: 20px;
  color: red;
  padding: 10px 30px 0;
  p{
    padding: 20px 0;
    // background: rgb(255, 186, 186);
    border-radius: 10px;
  }
}
button{
  border-radius: 5px;
  cursor: pointer;
  font-size: 20px;
}
</style>
