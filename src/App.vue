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
@import url(https://fonts.googleapis.com/earlyaccess/nanumgothic.css);
body {
  margin: 0 0 0 0;
}

input, button {
  font-family: 'Nanum Gothic';
  font-size: 15px;
}

#app {
  font-family: 'Nanum Gothic';
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin: 0 0 0 0;
}
</style>
