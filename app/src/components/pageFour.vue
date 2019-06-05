<template>
  <div>
    <img src="@/assets/cutOut.svg">
    <div class="answerTextContainer">
      <p class="answerText">You Answered</p>
      <p class="scoreText">{{score}}/10 <br>Correctly</p>
    </div>

    <div class="playTextContainer">
      <p class="playText">Click 'Exit' to Play Again</p>
      <button v-on:click = "exit">Exit</button>
    </div>
  </div>
</template>

<script>
  export default{
    name: 'pageFour',
    props: ['score', 'questionNum'],

    methods: {
      exit: function(){
        this.$emit("scoreEmitP3", 0)
        this.$emit("questEmitP3", 0)
        this.$emit("openPage", 2)
      }
    }
  }
</script>

<style lang="stylus" scoped>
  div
    button, p
      color: black
      font-family: 'Oxygen', sans-serif

    button
      font-size: 22px
      padding-bottom: 4px
      outline: none
      width: 130px
      height: 55px
      background: white
      border-color: orange
      border-radius: 6px
      box-shadow: 1px 6px 8px 1px grey

    img
      position: absolute
      margin-top: 0
      top: 0
      z-index: -1


    .answerTextContainer
      margin-top: 30vh

      .answerText
        font-size: 40px
        text-align: center
        margin-bottom: 1.5vh

      .scoreText
        margin: 0
        font-size: 30px
        text-align: center
        line-height: 40px

    .playTextContainer
      margin-top: 7vh
      display: flex
      align-items: center
      flex-direction: column

      .playText
        font-size: 16px
        padding-bottom: 5px


</style>
