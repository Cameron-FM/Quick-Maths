<template>
  <div id="app">
    <template v-if="currentPage === 1">
      <pageOne @openPage = "changePage"/>
    </template>

    <template v-if="currentPage === 2">
      <pageTwo @openPage = "changePage"/>
    </template>
  </div>
</template>

<script>
import pageOne from './components/pageOne'
import pageTwo from './components/pageTwo'

export default {
  name: 'app',
  components: {
    pageOne,
    pageTwo
  },
  data: () => {
    return {
      currentPage: 1
    }
  },
  methods: {
    changePage: function(pageData){
      this.currentPage = pageData.pageNumber
    }
  }
}

</script>

<style lang="stylus">

</style>
