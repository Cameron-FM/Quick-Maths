<template>
  <div>
    <img src="@/assets/cutOut.svg">
    <button class="backBtn" v-on:click = "back"><i class="material-icons">{{icon}}</i></button>

    <div class="questionContainer">
      <div class="questionText">{{question}}</div>
    </div>

    <div class="optionBtnContainer">
      <ol>
        <li v-for="option in options">
          <button class="optionBtn"> {{option.text}}</button>
        </li>
      </ol>
    </div>

  </div>

  </div>
</template>

<script>
  export default{
    name: 'pageThree',
    mounted(){
      this.genQuestion()
    },

    data: () => {
      return {
        icon: "arrow_back_ios",
        symbolArray: ["+","-","/","*"],
        question: "x",
        answer: "",
        options : [
          {text: '20'},
          {text: '47'},
          {text: '100'},
          {text: '22'}]
      }
    },

    methods: {
      back: function(){
        this.$emit("openPage", 2);
      },

      randomNum: function(min,max){
        min = Math.ceil(min);
        max = Math.floor(max);
        return Math.floor(Math.random() * (max - min)) + min;
      },

      randomSym: function(minIndex, maxIndex){
        return this.symbolArray[this.randomNum(minIndex, maxIndex)];
      },

      genQuestion: function(){
        let symbol = this.randomSym(0,3)
        let firstNum = this.randomNum(2,13).toString()
        if (symbol === this.symbolArray[1]){
          this.question = firstNum + symbol + this.randomNum(0, firstNum).toString();
        }else{
          this.question = firstNum + " " + symbol + " " + this.randomNum(0, 13).toString();
        }

      },

      //genNumber: function

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
      margin-top: 22vh

      .questionText
        text-align: center
        font-family:'IBM Plex Mono', monospace;
        font-size: 40px

    .optionBtnContainer
      margin-top: 4vh
      display: flex
      align-items: center
      flex-direction: column

      ol, li
        padding: 0
        list-style-type:none

        .optionBtn
          margin-top: 0.5vh
          font-size: 22px
          outline: none
          background-color: white
          margin-bottom: 25px
          width: 250px
          height: 50px
          border-color: orange
          border-radius: 6px
          box-shadow: 1px 6px 8px 1px grey

</style>
