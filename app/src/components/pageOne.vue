<template>
  <div>
    <h1>{{ msg }}</h1>
    <button v-on:click = "start">gay</button>
  </div>
</template>

<script>
  export default{
    name: 'pageOne',
    data: () => {
      return {
        msg: "tHIS IS Page One"
      }
    },
    methods: {
      start: function(){
        this.$emit("openPage", {
          pageNumber: 2
        })
      }
    }
  }
</script>

<style lang="stylus">
  h1
    color: red
</style>
