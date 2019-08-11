<template>
  <div class="result"> 
    <p>{{ msg }}</p>
    <p>記録 : {{ Math.round(this.record_time) }} msec</p>
    <h3>Score : {{ this.score }} </h3>
    <p v-show="this.is_best_record">自己ベスト更新!</p>
    <div class="reset_btn" @click="$emit('restart')">もう一度チャレンジ！</div>
  </div>
</template>

<script>
export default {
  name: 'Result',
  props:["result","best_score","score","max_renzoku_seikai","end_num","is_best_record","record_time"],
  computed:{
    msg:function(){

      let msg = ""
      if(this.max_renzoku_seikai == this.end_num){
        msg = this.max_renzoku_seikai + "問全問正解!"
      }else{
        msg =  "最高で" + this.max_renzoku_seikai + "問連続正解!"
      }
      return msg
    }
}
}
</script>

<style scoped>
.result{
  text-align: center;
  font-size: 20px;
  width:100%;
  margin:25vh auto 0;
}
.title{
  font-size: 30px;
  color:black;
}
.reset_btn{
  padding:15px;
  margin:100px 20% auto;
  background-color: #66f;
  color: white;
  border-radius: 5%;
}
</style>
