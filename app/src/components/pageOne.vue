<template>
  <div>
    <img src="@/assets/cutOut.svg">
    <div>
      <h1>{{ msg }}</h1>
      <button v-on:click = "start">Start</button>
    </div>
  </div>
</template>

<script>
  export default{
    name: 'pageOne',
    data: () => {
      return {
        msg: "This is Page One"
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
    h1
      color: black

    img
      position: absolute
      margin-top: 0
      top: 0
      z-index: -1

    div:nth-child(2)
      margin-top: 20vh
      display: flex
      align-items: center
      flex-direction: column
</style>
