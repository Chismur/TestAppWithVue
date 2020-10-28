<template>
<div id="header-quize">
  <b-nav>
    <b-nav-item>Home</b-nav-item>
    <b-nav-item>Achievements</b-nav-item>
    <b-nav-item disabled>Counter {{correct}}/{{total}}</b-nav-item>
    <b-nav-item hidden>Admin page</b-nav-item>
  </b-nav>
</div>
</template>
<script>
export default {
  name: "HeaderQuize",
  props: {
    correct: null,
    total: null
  },
  data() {
    return {
        someData:Number
    }
  }
}
</script>
<style scoped>
</style>
