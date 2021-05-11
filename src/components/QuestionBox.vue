<template>
<!----bootstrap vueのV4components start---->
    <div class="question-box-container">
        <b-jumbotron >
            <!----bootstrap vueのsimple hero unit start---->
           <template v-slot:lead>
               {{currentQuestion.question}}
           </template>

            <hr class="my-4">
            <!----bootstrap vueのList group start---->
            <b-list-group>
                <b-list-group-item v-for ="(answer,index) in answers"
                    :key="index" @click.prevent="selectAnswer(index)"
                    :class="answerClass(index)"
                >
                {{ answer }}
                </b-list-group-item>
            </b-list-group>
            <!----bootstrap vueのsimple hero unit end---->
            <b-button variant="primary"
            @click="submitAnswer"
            :disabled="selectedIndex === null || answered"

            >Submit</b-button>
            <b-button @click="next" variant="success" href="#">Next</b-button>
            <!----bootstrap vueのsimple hero unit end---->
        </b-jumbotron>
    </div>
    <!----bootstrap vueのV4components end---->
    
</template>

<script>
import _ from 'lodash'


export default {
    props: {
        currentQuestion: Object,
        next: Function,
        increment: Function
    },
    data(){
        return {
            selectedIndex: null,
            correctIndex:null,
            shuffledAnswers: [ ],
            answered :false
        }
    },
    computed:{
        answers(){
            //spread演算子で正解と不正解のオブジェクトを配列なら分解して配列に格納できる
            //shuffleする必要があるね
            let answers = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer)
            //console.log(answers)
            return answers
        }
    },
    //propsからのデータを監視する
    watch: {
        currentQuestion:{
            immediate: true,
            handler() {
                this.selectedIndex = null
                this.answered = false
                this.shuffleAnswers()
            }
        }
    },
    methods: {
        selectAnswer(index){
            this.selectedIndex = index
            //console.log(index)
            //console.log(answers)
        },
        submitAnswer(){
            let isCorrect = false
            
            if (this.selectedIndex === this.correctIndex){
                isCorrect = true
            }
            this.answered = true
            this.increment(isCorrect)
        },
        shuffleAnswers(){
            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            //console.log(this.answers)
            this.shuffledAnswers = _.shuffle(answers)
            this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
            
        },
        answerClass(index){
            let answerClass = ''

            if(!this.answered && this.selectedIndex === index){
                answerClass = 'selected'
            }else if (this.answered && this.correnctIndex === index){
                answerClass = 'correct'
            } else if (this.answered && 
            this.selectedIndex === index && 
            this.correctIndex !== index
            ) {
            answerClass = 'incorrect'
            }
            return answerClass
        }
    }
}
</script>

<style scoped>
    .list-group {
        margin-bottom: 15px;
    }
    .list-group-item:hover {
       background:#EEE;
       cursor: pointer;
    }
    .btn {
        margin: 0 5px;
    }
    .selected {
        background-color: lightblue;
    }
    .correct {
        background-color: lightgreen;
    }
    .incorrect {
        background-color: red;
    }
</style>
 