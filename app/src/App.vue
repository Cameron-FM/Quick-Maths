<template>
  <div id="app">
    <template v-if="currentPage === 1">
      <pageOne @openPage = "changePage"/>

      <div class="feedbackContainer">
        <p class="feedbackText">Please Give us Your Feeback!</p>
        <a class="linkText"href="https://forms.gle/VSosFNrwhikZrcHD6" target="_blank">https://forms.gle/VSosFNrwhikZrcHD6</a>
        <p class="versionText">v 0.1.0</p>
      </div>
    </template>

    <template v-if="currentPage === 2">
      <pageTwo @openPage = "changePage"/>
    </template>

    <template v-if="currentPage === 3">
      <pageThree @openPage = "changePage" @scoreEmitP3 = "changeScoreData" @questEmitP3 = "changeQuestData" :score="score" :questionNum="questionNum"/>
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
      questionNum: 0
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
    }
  }
}

</script>

<style lang="stylus">
  div
    p
      font-family: 'Oxygen', sans-serif

    .feedbackContainer
      margin-top: 20vh
      display: flex
      align-items: center
      flex-direction: column

      .feedbackText
        font-size: 14px

      .versionText, a
        font-size: 10px

      .versionText
        color: grey
</style>
