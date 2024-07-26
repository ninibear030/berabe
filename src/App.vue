<template>
  <div id="app">
    <div class="content">
      <router-view />
    </div>
  </div>
</template>
<script setup lang="ts"></script>
<style scoped>
#app {
  padding: 16px 16px 50px;
  min-height: 100vh;
  background-size: 100% 100%;
  background: url("bg.png");
}

.content {
  width: 480px;
  height: 700px;
  margin: 0 auto;
  padding: 10px;
  background-color: black;
  position: relative;
}
</style>
