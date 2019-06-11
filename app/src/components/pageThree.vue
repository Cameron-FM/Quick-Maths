<template>
  <div>
    <img src="@/assets/cutOut.svg">
    <button class="backBtn" v-on:click = "back"><i class="material-icons">{{icons[0]}}</i></button>

    <div class="infoContainer">
      <i class="material-icons">{{icons[2]}}</i>
      <div class="questionInfo">{{questionNum}}</div>
    </div>

    <div class="timerContainer">
      <i class="material-icons">{{icons[1]}}</i>

      <div class="timerLineContainer">
        <div class="timerLine" v-bind:style="{height: timerHeight}"></div>
      </div>
    </div>

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
    props: ['score', 'questionNum', 'difficulty'],

    mounted(){
      this.genQuestion();
    },

    data: () => {
      return {
        icons: ["arrow_back_ios", "alarm", "question_answer"],
        symbolArray: ["+","-","/","*"],
        question: "",
        answer: "",
        answerArray: Array(3).fill(""),
        mulitplier: 4,
        msg: "",
        startTime: 0,
        timerCount: 0,
        timerHeight: "0%",
        timerPercent: 0
      }
    },

    methods: {
      //Emits to app.vue to change page
      back: function(){
        clearTimeout(this.timeout)
        this.refresh()
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
        this.timerCount = 0
        this.timerPercent =  0
        this.timerHeight = this.timerPercent + "%"
      },

      randomNum: (min,max) => {
        min = Math.ceil(min);
        max = Math.floor(max);
        return Math.floor(Math.random() * (max - min)) + min;
      },

      randomSym: function(minIndex, maxIndex){
        return this.symbolArray[this.randomNum(minIndex, maxIndex)];
      },

      updateTimer: function(){
        console.log(this.timerCount)
        this.timerCount-=1
        this.timerPercent +=  100/this.startTime
        this.timerHeight = this.timerPercent + "%"

        if (this.timerCount <= 0){
          clearTimeout(this.timeout)
          this.timerCount = this.startTime
          this.$emit("questEmitP3", this.questionNum + 1)
          this.msg = "You Ran Out of Time!"
        }
      },

      startCountdown: function(){
        if (this.difficulty === 1){
          this.startTime = 20
          this.timerCount = this.startTime
          this.timeout = setInterval(()=>this.updateTimer(), 1000)
          //this.timeout = setInterval(()=>this.updateTimer(), 1000)
        }else if (this.difficulty === 2){
          this.startTime = 10
          this.timerCount = this.startTime
          this.timeout = setInterval(()=>this.updateTimer(), 1000)
        }
      },

      //Generates random question
      genQuestion: function(){
        //Get a random symbol and number between 1 and 12
        let symbol = this.randomSym(0,3)
        let firstNum = this.randomNum(2,13).toString()
        //Disable division for if playing on easy
        if (this.difficulty === 0){
          this.symbolArray.splice(2,1)
          symbol = this.randomSym(0,2)
        }
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
        this.startCountdown()
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
          //Play 'Success' Sound
          new Audio(
            "https://suraj.codes/dist/assets/cdn/audio/success.wav").play();
        }else{
          console.log("Incorrect!\nAnswer: ", this.answer)
          this.msg = "Incorrect! Answer is " + this.answer
          //Play 'Fail' Sound
          new Audio(
            "https://suraj.codes/dist/assets/cdn/audio/fail.wav").play();
        }
      },

      checkGameFinshed: function(){
        //Increase the question number prop counter
        this.$emit("questEmitP3", this.questionNum + 1)
        //Stops Timer
        clearTimeout(this.timeout)
        if (this.questionNum === 10){
          this.refresh()
          this.$emit("openPage", 4)
        }else {
          this.refresh()
          this.genQuestion()
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

    .infoContainer
      position: absolute
      display: flex
      margin-top: 13vh
      margin-left: 3vh
      z-index: 2
      align-items: center

      .questionInfo
        margin-left: 10px
        font-size: 24
        text-align: center
        font-family: 'Oxygen', sans-serif
        color: purple
        font-weight: bold;


    .timerContainer
      position: absolute
      display: flex
      align-items: center
      flex-direction: column
      margin-top: 19vh
      margin-left: 3vh
      z-index: 2

      .timerLineContainer
        margin-top: 0.5vh
        background-color: rgba(211,211,211, 0.5)
        width: 5px
        height: 45vh
        border-bottom: 1px solid rgba(211,211,211, 0.5)
        border-radius: 5px

        .timerLine
          transition: ease 0.5s
          background-color: purple
          width: 5px
          height: 20%
          border-bottom: 1px solid purple
          border-radius: 5px

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
