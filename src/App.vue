<script setup lang="ts">
import SurveyCreator from './components/SurveyCreator.vue';
import MySurvey from './components/MyFirstSurvey.vue';
</script>

<template>
  <div id="surveyCreator">
    <SurveyCreator />
  </div>
  <div id="surveyContainer">
    <MySurvey />
  </div>
</template>

<style scoped>
#surveyCreator {
  height: 100vh;
  width: 100vw;
}
</style>