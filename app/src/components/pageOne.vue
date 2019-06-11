<template>
  <div>
    <img src="@/assets/cutOut.svg">
    <div>
      <h1>{{ msg }}</h1>
      <button v-on:click = "start">Start</button>
      <img src="https://suraj.codes/dist/images/success.png">
    </div>
  </div>
</template>

<script>
  export default{
    name: 'pageOne',
    data: () => {
      return {
        msg: "Quick Maths"
      }
    },
    methods: {
      start: function(){
        this.$emit("openPage", 2)
      }
    }
  }
</script>

<style lang="stylus" scoped>
  div
    h1, button
      color: black
      font-family: 'Oxygen', sans-serif
      font-size: 42px

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

    div:nth-child(2)
      margin-top: 35vh
      display: flex
      align-items: center
      flex-direction: column

      img
        width: 300px
        margin-top: 62vh

</style>
