<template>
  <div id="app">
    
          <Header
          :index="index"
          :totalQuestions="questions.length"
          :score="numCorrect"
          />
    <b-container   class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox :currentQuestion="questions[index]" :next="next"
          :index="index"
          :previous="previous"
          :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from './components/Header'
import QuestionBox from './components/QuestionBox';
export default {
  name: 'App',
  components: {
    Header,
    QuestionBox
  },
  data(){
    return {
      questions:[],
      index:0,
      score:0,
      numCorrect:0,
      numTotal:0,
    }
  },
  methods:{
        next:function(){
            this.index=(this.index+1)%this.questions.length;
        },
        previous:function(){
            this.index=(this.index+9)%this.questions.length;
        },
        increment(isCorrect){
          if(isCorrect){
            this.numCorrect++
          }
          this.numTotal++;
        }
  },
  mounted:function(){
    fetch("https://opentdb.com/api.php?amount=10&category=9&type=multiple",{
      method:'get'
    }).then(
      (res)=> {return res.json()}
    )
    .then(data=>{
      this.questions=data.results;
    }).
    catch(err=>console.log(err))
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
