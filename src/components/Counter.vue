<template>
  <div class="counter_area" :class="{ bg_green:isRight, bg_red:isNotRight }">
    <h3>{{ theme }}</h3>
    <p class="counter">{{ number }}</p>
    <p class="jikkyou">連続正解数: {{ renzoku_seikai }}</p>
  </div>
</template>

<script>
export default {
  name: 'Counter',
  props: ["number","theme","isRight","isNotRight","renzoku_seikai"],
}
</script>

<style scoped>
.counter_area{
  padding-top:10px;
  height:40vh;
}
.title{
  font-size: 10px;
}
.counter{
  font-size: 35px;
  
}
.jikkyou{
  text-align: right;
  margin:auto 5px auto;
  background-color: gray;
  color:white;
  border-radius: 5%;
  padding:5px;
}

.bg_green{
animation-duration: 0.2s;
animation-name: grad_green;
animation-timing-function:ease-out; 
}
.bg_red{
animation-duration: 0.2s;
animation-name: grad_red;
animation-timing-function:ease-out; 
}

@keyframes grad_green {
0% { background-color: #c7ffc7;}
100% { background-color: #fff;}
}

@keyframes grad_red {
0% { background-color: #fcc3c3;}
100% { background-color: #fff;}
}

</style>
