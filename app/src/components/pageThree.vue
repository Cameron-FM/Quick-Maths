<template>
  <div>
    <img src="@/assets/cutOut.svg">
    <button class="backBtn" v-on:click = "back"><i class="material-icons">{{icon}}</i></button>

    <div class="questionContainer">
      <div class="questionText">{{question}}</div>
    </div>

    <div class="optionBtnContainer">
      <ol>
        <li v-for="ans in answerArray">
          <button class="optionBtn" v-on:click="checkAnswer(ans)">{{ans}}</button>
        </li>
      </ol>
    </div>

    <template v-if="msg">
      <div class="messageContainer">
        <div class="messageText">{{msg}}</div>
      </div>

      <div class="nextContainer">
        <button class="nextBtn" v-on:click="checkGameFinshed">Next</button>
      </div>
    </template>

  </div>
</template>

<script>
  export default{
    name: 'pageThree',
    props: ['score', 'questionNum'],

    mounted(){
      this.genQuestion();
    },

    data: () => {
      return {
        icon: "arrow_back_ios",
        symbolArray: ["+","-","/","*"],
        question: "",
        answer: "",
        answerArray: Array(3).fill(""),
        mulitplier: 4,
        msg: ""
      }
    },

    methods: {
      //Emits to app.vue to change page
      back: function(){
        this.$emit("scoreEmitP3", 0)
        this.$emit("questEmitP3", 0)
        this.$emit("openPage", 2)
      },

      refresh: function(){
        this.msg = ""
        this.question = ""
        this.answer  = ""
        this.answerArray = [],
        this.answerArray = Array(3).fill("")
      },

      randomNum: (min,max) => {
        min = Math.ceil(min);
        max = Math.floor(max);
        return Math.floor(Math.random() * (max - min)) + min;
      },

      randomSym: function(minIndex, maxIndex){
        return this.symbolArray[this.randomNum(minIndex, maxIndex)];
      },

      //Generates random question
      genQuestion: function(){
        //Get a random symbol and number between 1 and 12
        let symbol = this.randomSym(0,3)
        let firstNum = this.randomNum(2,13).toString()
        //If the symbol is a '-'
        if (symbol === this.symbolArray[1] || symbol === this.symbolArray[2]){
          //Make the second number smaller, so a postive answer is produced
          //Concatenate all components of the question
          this.question = firstNum + symbol + this.randomNum(1, firstNum).toString();
        }else{
          //Else concatenate all components, with any number 1-12 as the second number
          this.question = firstNum + " " + symbol + " " + this.randomNum(1, 13).toString();
        }

        this.answer = Math.round(eval(this.question) * 10) / 10
        this.createAnswers()
      },

      //Creates alternative answer options
      createAnswers: function(){
        //Gets number of digits in the answer
        let digitSize = this.answer.toString().length
        let defaultVariant = 5
        let symbol = this.randomSym(0, 2)
        //If the answer > 1 digits
        if (digitSize > 1){
          //mulitply by the mulitplier and then by the digitSize
          //Creates a balanced answer option
          let defaultVariant = 5 * this.mulitplier * digitSize
        }

        //For the three alternative answer options
        for (let i=0; i < 3; i++){
          //Create and variant and new answer option
          let variant = this.randomNum(1,defaultVariant+1);
          let newAnswer = Math.round(eval(this.answer + symbol + this.randomNum(1,defaultVariant+1)) * 10) / 10
          //Validates - Creates new, newAnswer untill it is > 0
          while (symbol === this.symbolArray[1] || symbol === this.symbolArray[2] || newAnswer === this.answer || this.answerArray.includes(newAnswer)){
            symbol = this.randomSym(0, 2)
            newAnswer = Math.round(eval(this.answer + symbol + this.randomNum(1,defaultVariant+1)) * 10) / 10
          }


          //Push the randomly generated answer to answerArray
          this.answerArray[i] = newAnswer
        }
        //Insert actual answer into random point in answerArray
        this.answerArray.splice(this.randomNum(0, this.answerArray.length+1), 0, this.answer)
      },

      checkAnswer: function(ans){
        //Checks if answer is correct
        if (ans === this.answer){
          console.log("Correct!\nAnswer: ", this.answer)
          this.msg = "Correct! Answer is " + this.answer
          this.$emit("scoreEmitP3", this.score + 1)
        }else{
          console.log("Incorrect!\nAnswer: ", this.answer)
          this.msg = "Incorrect! Answer is " + this.answer
        }

        //Increase the question number prop counter
        this.$emit("questEmitP3", this.questionNum + 1)
      },

      checkGameFinshed: function(){
        if (this.questionNum >=11){
          this.refresh()
          this.$emit("openPage", 4)
        }else {
          this.refresh()
          this.genQuestion();
        }
      }

    }
  }

//Timer
//random using number 1-12
//no negative numbers
//No decimals

</script>

<style lang="stylus" scoped>
  body
    margin: 0px

  div
    button
      color: black
      font-family: 'Oxygen', sans-serif

    .backBtn
      outline: none
      background: white
      font-size: 22px
      border: none
      padding-left: 10px
      padding-top: 5px
      text-shadow: 1px 2px 5px grey

    img
      position: absolute
      margin-top: 0
      top: 0
      z-index: -1

    .questionContainer
      margin-top: 17vh

      .questionText
        text-align: center
        font-family:'IBM Plex Mono', monospace;
        font-size: 40px

    .optionBtnContainer
      display: flex
      align-items: center
      flex-direction: column

      ol, li
        padding: 0
        list-style-type:none

        .optionBtn
          margin-top: 3.5vh
          font-size: 22px
          outline: none
          background-color: white
          width: 200px
          height: 45px
          border-color: orange
          border-radius: 6px
          box-shadow: 1px 6px 8px 1px grey

    .messageContainer
      margin-top: 3.5vh
      display: flex
      justify-content: center

      .messageText
        text-align: center
        font-family: 'Oxygen', sans-serif
        font-size: 22px

    .nextContainer
      display: flex
      justify-content: center

      .nextBtn
        margin-top: 3vh
        font-size: 20px
        outline: none
        background-color: white
        margin-bottom: 25px
        width: 150px
        height: 40px
        border-color: orange
        border-radius: 6px
        box-shadow: 1px 6px 8px 1px grey
        text-align: center

</style>
