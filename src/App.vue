<template lang= "pug">
  #app
    router-view
</template>

<script>
export default {
  name: 'app',
};
</script>

<style lang="less">
body {
  margin: 0 0 0 0;
}
#app {
  @import url('https://fonts.googleapis.com/css?family=Acme');

  font-family: 'Acme';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin: 0 0 0 0;
}
</style>
