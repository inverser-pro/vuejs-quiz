<template>
  <div id="app">
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox
                  v-if="questions.length"
            :currentQuestion="questions[index]"
            :index="index2"
            :totalLength="totalQuestions()"
            :next="next"
          />
        </b-col>
      </b-row>
    </b-container>

  </div>
</template>

<script>

import QuestionBox from './components/QuestionBox.vue'

export default {
  name: 'app',
  components: {
      QuestionBox
  },
  data(){
    return {
        questions:[],
        index:0,
        index2:1,
        totalLength:0/* чтобы узнать, когда мы придем к концу опроса и показать форму обраттной связи */
    }
  },
  methods:{
    next(){
        if(this.questions.length!=this.index2){
            this.index++
            this.index2++
        }
    },
    totalQuestions(){
        return this.totalLength=this.questions.length
    }
  },
  mounted: function(){
      //fetch('https://opentdb.com/api.php?amount=10&type=multiple',{
      fetch('questions.json',{
          method: 'get'
      }).then((response)=>{
          return response.json()
      }).then((jsonData)=>{
          this.questions=jsonData.results
      })
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
  margin-top: 60px;
}
.nav-tabs{margin:10px 0}
</style>
