<template>
  <div id="main">
    <div id="stage">
      <div id="snake">
        <div ref="snakeHead" class="snakeHead"></div>
        <div v-for="snake in bodyLength" :key="snake" class="snakeBody" ref="snakeBod"></div>
      </div>
      <div id="food" ref="food">
        <div></div>
        <div></div>
        <div></div>
        <div></div>
      </div>
    </div>
    <div id="score-panel">
      <div>
        SCORE: <span id="score">{{ score }}</span>
      </div>
      <div>
        LEVEL: <span id="level">{{ level }}</span>
      </div>
    </div>
    <div class="controller" v-if="isMobile">
      <div>
        <button @click="mobileController('ArrowUp')" id="up">上</button>
        <button @click="mobileController('ArrowDown')" id="down">下</button>
        <button @click="mobileController('ArrowLeft')" id="left">左</button>
        <button @click="mobileController('ArrowRight')" id="right">右</button>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup >
import { ref,  onMounted, nextTick } from 'vue';
import type { Ref } from 'vue';

const emits = defineEmits<{
  (e: 'gameOver', overString: string): void
}>()
const isMobile: Ref<boolean> = ref(false)
const isStart: Ref<boolean> = ref(false);
const isOver: Ref<boolean> = ref(false);

const score: Ref<number> = ref(0);
const level: Ref<number> = ref(1);
const upScore = 5
const maxLevel = 10;
const direction: Ref<string> = ref('');
const snakeHead: Ref<HTMLElement | null> = ref(null);
const bodyLength: Ref<number> = ref(0);
const food: Ref<HTMLElement | null> = ref(null);

const changePosition = ():void => {
  const top: number = Math.round(Math.random() * 29) * 10
  const left: number= Math.round(Math.random() * 29) * 10

  if(food.value === null) return
  food.value.style.left = left + 'px'
  food.value.style.top = top + 'px'
}

const addScore = ():void => {
  score.value += 1
  if(score.value % upScore === 0){
    levelUp()
  }
}

const levelUp = ():void => {
  if(level.value > maxLevel) return
  level.value += 1
}

const mobileController = (buttonDirection: string):void => {
  if(isStart.value === false){
    isStart.value = true
    run()
  }
  direction.value = buttonDirection
}
const keydownHandler = (e: KeyboardEvent):void => {
  direction.value = e.key
  if(isStart.value === false){
    isStart.value = true
    run()
  }
}

const run = async(): Promise<void> => {
  let left: number = snakeHead.value?.offsetLeft || 0
  let top: number = snakeHead.value?.offsetTop || 0
  let tempLeft: number = left
  let tempTop: number = top

  switch (direction.value) {
    case 'ArrowUp':
      top -= 10
      break;
    case 'ArrowDown':
      top += 10
      break;
    case 'ArrowLeft':
      left -= 10
      break;
    case 'ArrowRight':
      left += 10
      break;
    default:
      break;
  }

  // 當碰到牆壁的時候
  if(left < 0 || left > 290 || top < 0 || top > 290){
    isOver.value = true
    emits('gameOver','撞到牆壁了');
    return
  }
  // 當吃到食物的時候
  if(food.value?.offsetLeft === left && food.value?.offsetTop === top){
    bodyLength.value += 1
    changePosition()
    addScore()
    await nextTick()
  }
  // 蛇頭移動
  if(snakeHead.value === null) return
  snakeHead.value.style.left = left + 'px'
  snakeHead.value.style.top = top + 'px'

  const snakeBody = document.querySelectorAll('.snakeBody')

  //撞到身體
  if(bodyLength.value > 0){
    try{
      snakeBody.forEach((item) => {
        const element = item as HTMLElement;
        if(element.offsetLeft === snakeHead.value?.offsetLeft && 
          element.offsetTop === snakeHead.value?.offsetTop){
            throw new Error()
          }
      })
    }catch(e){
      isOver.value = true
      emits('gameOver','撞到身體了');
      return
    }
  }
  // 蛇身移動
  snakeBody.forEach((item) => {
    const element = item as HTMLElement;
    const Left = element.offsetLeft
    const Top = element.offsetTop
    element.style.left = tempLeft + 'px';
    element.style.top = tempTop + 'px';
    tempLeft = Left;
    tempTop = Top;
  })

  setTimeout(() => {
    run()
  }, 300 - (level.value - 1) * 30);
}
onMounted(() => {
  if (/Mobi|Android|iPhone/i.test(navigator.userAgent)) {
    isMobile.value = true
  }else{
    document.addEventListener('keydown', keydownHandler);
  }
  
  changePosition()
});
</script>

<style lang="scss">

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#main {
  width: 360px;
  background-color: rgb(165, 92, 249);
  margin: 30px auto 30px;
  border: 10px solid rgb(255, 219, 101);
  border-radius: 10px;
  font-size: 20px;
  font-weight: 700;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-around;
  gap: 20px;
  padding: 10px 0 20px;
  #stage{
    width: 304px;
    height: 304px;
    border: 2px solid #595959;
    position: relative;
    background: black;
    #snake{
      &>div{
        width: 10px;
        height: 10px;
        border: 1px solid #b7d4a8;
        background-color: black;
        position: absolute;
      }
      .snakeBody{
        background: #aaff83;
      }
      .snakeHead{
        background: #00d335;
        z-index: 10;
        left: 40px;
        top: 40px
      }
    }
    &>#food{
      position: absolute;
      width: 10px;
      height: 10px;
      display: flex;
      flex-wrap: wrap;
      & > div{
        width: 5px;
        height: 5px;
        background-color: red;
        animation: rotate ease-in 2s infinite;
      }
    }
  }

  #score-panel{
    width: 100%;
    display: flex;
    justify-content: space-around;
    align-items: center;
    color: wheat;
  }

  .controller{
    display: flex;
    width: 100%;
    justify-content: center;
    &>div{
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      grid-template-rows: repeat(3, 1fr);
      gap: 10px;
      #up{
        grid-column: 2/3;
        grid-row: 1/2;
      }
      #down{
        grid-column: 2/3;
        grid-row: 3/4;
      }
      #left{
        grid-column: 1/2;
        grid-row: 2/3;
      }
      #right{
        grid-column: 3/4;
        grid-row: 2/3;
      }
    }
    button{
      background: #95ff9c;
      color: #008521;
      padding: 10px 15px;
    }
  }
}


@keyframes rotate {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg)
  }
}
</style>
