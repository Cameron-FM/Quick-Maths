<template>
  <div>
    <img src="@/assets/cutOut.svg">
    <button class="backBtn" v-on:click = "back"><i class="material-icons">{{icon}}</i></button>
  </div>
</template>

<script>
  export default{
    name: 'pageThree',
    data: () => {
      return {
        msg: "This is Page Two",
        icon: "arrow_back_ios"
      }
    },
    methods: {
      back: function(){
        this.$emit("openPage", 2)
      }
    }
  }
</script>

<style lang="stylus" scoped>
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
      text-shadow: 1px 2px 5px grey;

    img
      position: absolute
      margin-top: 0
      top: 0
      z-index: -1

</style>
