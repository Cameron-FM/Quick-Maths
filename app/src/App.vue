<template>
  <div id="app">
    <template v-if="currentPage === 1">
      <pageOne @openPage = "changePage"/>
    </template>

    <template v-if="currentPage === 2">
      <pageTwo @openPage = "changePage" @emitDifficulty = "changeDifficulty"/>
    </template>

    <template v-if="currentPage === 3">
      <pageThree @openPage = "changePage" @scoreEmitP3 = "changeScoreData" @questEmitP3 = "changeQuestData" :score="score" :questionNum="questionNum" :difficulty="difficulty"/>
    </template>

    <template v-if="currentPage === 4">
      <pageFour @openPage = "changePage" @scoreEmitP3 = "changeScoreData" @questEmitP3 = "changeQuestData" :score="score" :questionNum="questionNum"/>
    </template>

  </div>
</template>

<script>
import pageOne from './components/pageOne'
import pageTwo from './components/pageTwo'
import pageThree from './components/pageThree'
import pageFour from './components/pageFour'

export default {
  name: 'app',
  components: {
    pageOne,
    pageTwo,
    pageThree,
    pageFour
  },
  data: () => {
    return {
      currentPage: 1,
      score: 0,
      questionNum: 1,
      difficulty: 0
    }
  },

  methods: {
    changePage: function(pageNumber){
      this.currentPage = pageNumber
    },

    changeScoreData: function(scoreNum){
      this.score = scoreNum
    },

    changeQuestData: function(questNum){
      this.questionNum = questNum
    },

    changeDifficulty: function(diff){
      this.difficulty = diff
      console.log("Difficulty: ", this.difficulty);
    }
  }
}

</script>

<style lang="stylus">
  div
    p
      font-family: 'Oxygen', sans-serif

</style>
