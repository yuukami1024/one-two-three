<template>
  <div id="app">
    <Header></Header>
    <Counter v-if="visible_game_screen" :renzoku_seikai="renzoku_seikai" 
    :isRight="isRight" :isNotRight="isNotRight" :number="number" :theme="theme"/>
    <Screen v-if="visible_game_screen" @touch="onTouch" :theme="theme"/>
    <Result v-if="!visible_game_screen" @restart="restart" :max_renzoku_seikai="max_renzoku_seikai" :end_num="END_NUM"
    :result="result" :score="score" :is_best_record="is_best_record" :record_time="time.recordTime" />
  </div>
</template>

<script>
import Counter from './components/Counter.vue'
import Screen from './components/Screen.vue'
import Header from './components/Header.vue'
import Result from './components/Result.vue'

export default {
  name: 'app',
  components: {
    Header,
    Counter,
    Screen,
    Result
  },
  data(){
    return{
      number:1,
      isRight:null,
      isNotRight:null,
      renzoku_seikai:0,
      max_renzoku_seikai:0,
      theme:"100までの3の倍数or3が付く数字",
      true_condition:false,
      result:"",
      END_NUM:100,
      visible_game_screen:true,
      time:{
        isStarted:false,
        startTime:Object,
        recordTime:Object
      },
      is_best_record:false
    }
  },
  computed: {
    score : function(){
      return 1000 + this.max_renzoku_seikai*500 - Math.round(this.time.recordTime*0.05)
    }
  },
  methods:{  
    onTouch : function(isRight){
      if(this.number == 1){

        this.time.startTime=performance.now()
        this.time.isStarted=true

      }

      this.judge(isRight);
      if(this.number < this.END_NUM){
        this.number++
      }else{
        this.complete()
      }
    },
    judge: function(input){

      //inoutが条件を満たすかを判定して真偽を返す。

      this.true_condition = (this.number%3 == 0 || (this.number+'').search(/3/) !== -1)

      //以下判定機
      if(this.true_condition == input){
        //正解のとき

        //正解のcssアニメ
        this.isRight = true
        this.isNotRight = false

        this.result="正解！"
        this.renzoku_seikai++

        this.max_renzoku_seikai = Math.max(this.max_renzoku_seikai, this.renzoku_seikai)

      }else{
        //不正解のとき

        //不正解のcssアニメ
        
        this.isRight = false
        this.isNotRight = true

        this.result="不正解"
        this.renzoku_seikai = 0

      }
        //0.1秒でCSSのクラスを消去
        setTimeout(()=>{
          this.isRight = false
          this.isNotRight = false
        },100)

      return this.true_condition

    },
    restart: function(){
      this.number=1
      this.renzoku_seikai=0
      this.max_renzoku_seikai=0
      this.true_condition=false
      this.result=""
      this.visible_game_screen=true
      this.time.isStarted=false
      this.recordTime = null
    },
    readTheme: function(){

    },
    randomInt: function(min, max){
      let interval = max - min + 1;
      return ~~(Math.random() * interval + min);
    },
    complete:function(){
      let endTime = performance.now()

      this.time.recordTime = endTime - this.time.startTime//時間を記録

      this.time.isStarted = false//終了フラグ

      this.visible_game_screen = false//リザルト画面へ移る

      let myRecord = JSON.parse(localStorage.getItem("myRecord"))//ストレージを読み込む

      let gameId = "1"//3の倍数のゲームID

      if(myRecord !== null){
        //既存の記録があるとき
        if(this.score > myRecord[gameId]){
          //記録が更新されたとき
          this.is_best_record=true

          myRecord[gameId] = this.score

          localStorage.setItem("myRecord",JSON.stringify(myRecord))

        }

      }else{
        //新規記録の作成
        myRecord = {}

        myRecord[gameId] = this.score

        localStorage.setItem("myRecord",JSON.stringify(myRecord))

      }

    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin:0;
  margin-top: 60px;
}

body{
  margin:0;
}
</style>
