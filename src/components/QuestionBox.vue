<template>
<div class="Question-box-container">
    <b-jumbotron header="BootstrapVue" lead="Bootstrap v4 Components for Vue.js 2">
        <template #header>BootstrapVue</template>

        <template slot=lead>
            {{currentQuestion.question}}

        </template>

        <hr class="my-4">
        <b-list-group>
            <b-list-group-item
            v-for="(answer , index) in answers"
             :key="index"
             @click="selectanswer(index)"
             :class="answerClass(index) "

            >
            {{answer}}
           </b-list-group-item>
           
        </b-list-group>

        

            <b-button 
            variant="primary"
            @click="submitAnswer"
            :disable="selectedIndex===null ||answered"
             >Submit</b-button>

            <b-button @click="Next" variant="success" href="#">
                Next</b-button>
    </b-jumbotron>
</div>
</template>

<script>
import  _ from 'lodash'
export default {
    props: {
        currentQuestion: Object,
        Next: Function,
        increment:Function
    },

    data :function(){
        return{
        //    answered=false,
            selectedIndex:null,
            currectIndex:null,
            shuffledAnswers:[]
            

        }
    },
    computed: {
        answers() {
            let answers = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answers)
            return answers
        }
    },

    watch:{
        currectQuestion:{
            immediate: true,
            handler(){
            this.answered=false
             this.selectedIndex=null
              this.shuffleAnswer()
            }
            
            
           
        }
    },

    methods:{
       selectanswer(index){
        this.electedIndex=index
        
       },

       submitAnswer(){
          let isCorrect = false

          if(this.selectedIndex===this.correctIndex){
              isCorrect=true
          }

          this.answered =true

          this.increment(isCorrect)


       },
       shuffleAnswers(){
           let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answers]
           this.shuffledAnswers=_.shuffle (answers)
           this.correctIndex=this.shuffledAnswers.indexOf(this.currentQuestion.correct_answers)
       },

       answerClass(index){
        let answerClass= ''
        if(! this.answered && this.selectedIndex===index){
            answerClass='selected'
        }
        else if(this.answered && this.correctIndex=== index){
            answerClass='correct'
        }
        // else if(this.answered && this.correctIndex=== index && this.correctIndex !==index){
        //     answerClass='incorrect'
        // }
        return answerClass
       }
      

        
      
    },
     
}
</script>

<style scoped>
.list-group{
    margin-bottom: 15px;
}
.list-group-item:hover{
    margin-bottom: 15px;
    background-color: aqua;
    cursor: pointer;
}
.selected{
    background-color: blue;
}
.correct{
    background-color: brown;
}
.btn{
    margin: opx;
}
</style>
