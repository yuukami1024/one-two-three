<template>
  <div class="screen"> 
      <div class="btn btn-left" @click="$emit('touch',0)"><p class="btn-text">違う</p></div>
      <div class="btn btn-right" @click="$emit('touch',1)"><p class="btn-text">{{ theme }}</p></div>
  </div>
</template>

<script>
export default {
  name: 'Screen',
  props:["theme"]
}
</script>

<style scoped>
.screen{
  display: flex;
  height:50vh;
  width:100%;
}
.btn-text{
  font-size:17px;
  color:white;
}

.btn{
height: 100%;
width:50%;
}
.btn-left{
  background-color: #00aaff;
}
.btn-right{
  background-color: #ffaa00;
}

</style>
