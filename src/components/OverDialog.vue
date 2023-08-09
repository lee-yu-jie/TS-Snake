<template>
  <div class="over-container">
    <div class="over-dialog">
      <p>{{ props.overString }}</p>
      <button @click="reStart">重新開始</button>
    </div>
  </div>
</template>

<script setup lang="ts">
const props = defineProps<{
  reStart: () => void;
  overString: string;
}>()
</script>

<style lang="scss" scoped>
.over-container{
  position: absolute;
  top: 200px ;
  right: 0;
  bottom: 0;
  left: 0;
  display: flex;
  justify-content: center;
  align-items: start;
  z-index: 20;
  .over-dialog{
    background: rgba(255, 255, 255, 0.387);;
    padding: 20px 40px;
    border-radius: 10px;
    display: flex;
    flex-direction: column;
    gap: 20px;
    align-items: center;
    color: white;
  }
  button{
  background: rgb(88, 227, 255);
  padding: 5px 10px;
  border: 1px skyblue solid;
  color: rgb(0, 109, 242);
}
}
</style>