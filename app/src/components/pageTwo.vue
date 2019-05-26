<template>
  <div>
    <h1>{{ msg }}</h1>
    <button v-on:click = "start">Not gay</button>
  </div>
</template>

<script>
  export default{
    name: 'pageTwo',
    data: () => {
      return {
        msg: "tHIS IS Page Two"
      }
    },
    methods: {
      start: function(){
        this.$emit("openPage", {
          pageNumber: 1
        })
      }
    }
  }
</script>

<style lang="stylus">
  h1
    color: red
</style>
