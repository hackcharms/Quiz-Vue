<template>
<div>
  <b-jumbotron>
    <template v-slot:lead v-if="currentQuestion" >
        <!-- {{currentQuestion.question}} -->
        <p v-html="currentQuestion.question"></p>
    </template>

    <hr class="my-4">
    <b-list-group class="p-1 mb-2">
        <b-list-group-item  v-for="(answer, index) in answers" :key="index" 
        @click="selectAnswer(index)" v-html="answer"
        :class="answerClass(index)"
        >
        </b-list-group-item>
    </b-list-group>
            <b-button variant="info" :disabled="index<1" class="mr-2" @click="previous">Previous</b-button>
            <b-button variant="success" class="mr-2" @click="submitAnswer" :disabled="selectedIndex==null || answered">Submit</b-button>
            <b-button variant="primary" :disabled="index>8" @click="next">Next</b-button>
        </b-jumbotron>
    </div>
</template>
<script>
export default {
    props:{
        currentQuestion:Object,
        next:Function,
        previous:Function,
        increment:Function,
        index:Number,
    },
    data(){
        return{
            selectedIndex:null,
            correctIndex:null,
            answered:false,

        }
    }
    ,
    methods:{
        hi:function(){
            alert('Hey Bi');
        },
        selectAnswer(index){
            return this.selectedIndex=index
        },
        shuffle(array) {
            array.sort(() => Math.random() - 0.5);
            this.correctIndex=array.indexOf(this.currentQuestion.correct_answer)
            return array
            }
        ,
        submitAnswer(){
            let isCorrect=false
            if(this.selectedIndex==this.correctIndex){
                isCorrect=true
            }
            this.answered=true
            this.increment(isCorrect)
        },
        answerClass(index){
            let answeredClass=''
            if(!this.answered && this.selectedIndex===index ){
                    answeredClass='selected'
            }
            else if(this.answered && this.correctIndex===index){
                    answeredClass='correct'
                }
            else if( this.answered && this.selectedIndex===index && this.correctIndex!==index  ){
                console.log(index)
                answeredClass='incorrect';
            }
            return answeredClass;
        }
    },
    watch:{
        
        currentQuestion:{
            immediate:true,
            handler(){
                this.selectedIndex=null;
                this.answered=false
                return this.answers;
            }
        }
    }
    ,
  computed:{
        answers(){
            if(this.currentQuestion){
                let answers=[...this.currentQuestion.incorrect_answers]
                answers.push(this.currentQuestion.correct_answer)
            return this.shuffle(answers)
            }
            return 0
        }
    },
    created:function(){
        // this.selectedIndex=null;
    }
    
}
</script>
<style scoped>
    .list-group-item{
        background-color: #fafbfc;
    }
    .list-group-item:hover{
        background-color: rgb(146, 201, 223);
        cursor: pointer;
    }
    .selected{
        background-color: skyblue;
    }
    .correct{
        background-color: rgb(85, 176, 85);
    }
    .incorrect{
        background-color: rgb(253, 93, 93);
    }
</style>