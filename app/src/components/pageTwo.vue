<template>
  <div>
    <img src="@/assets/cutOut.svg">
    <button class="backBtn" v-on:click = "back"><i class="material-icons">{{icon}}</i></button>
    <div>
      <ol>
        <li v-for="difficulty in difficulties">
          <button class="diffBtn" v-on:click = "nextPage(difficulty.text[1])"> {{difficulty.text[0]}} </button>
        </li>
      </ol>
    </div>
  </div>
</template>

<script>
  export default{
    name: 'pageTwo',
    data: () => {
      return {
        icon: "arrow_back_ios",
        difficulties : [
          {text: ['Easy', 0]}, {text: ['Medium', 1]}, {text: ['Hard', 2]}]
      }
    },
    methods: {
      back: function(){
        this.$emit("openPage", 1)
      },
      nextPage: function(diff){
        this.$emit("openPage", 3)
        this.$emit("emitDifficulty", diff)
      }
    }
  }
</script>

<style lang="stylus" scoped>
  div
    h1, button
      color: black
      font-family: 'Oxygen', sans-serif

    .backBtn
      outline: none
      background: white
      font-size: 22px
      border: none
      padding-left: 10px
      padding-top: 5px
      text-shadow: 1px 2px 5px grey;

    ol, li
      padding: 0
      list-style-type:none

    .diffBtn
      font-size: 22px
      outline: none
      background-color: white
      margin-bottom: 30px
      width: 250px
      height: 50px
      border-color: orange
      border-radius: 6px
      box-shadow: 1px 6px 8px 1px grey

    img
      position: absolute
      margin-top: 0
      top: 0
      z-index: -1

    div:nth-child(3)
      margin-top: 30vh
      display: flex
      align-items: center
      flex-direction: column
</style>
