<template>
  <header>
    <h1 class="title">{{headerTitle}}</h1>
  </header>
</template>

<script>
export default {
  props: ["headerTitle"]
}
</script>

<style scoped>
header {
  height: 40px;
  background-color:aqua;
}

.title {
  color: white;
  font-size: 26px;
  line-height: 40px;
  margin-left: 20px;
}
</style>