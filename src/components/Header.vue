<template>
  <div class="header">
      <h1>{{header_title}}</h1>
      <img src="https://cdn.iconscout.com/icon/free/png-512/bird-299-1131175.png" alt="bird" width="45" height="45">
  </div>
</template>

<script>
export default {
  name: "Header",
  props: {
    header_title: String,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .header {
      background: rgba( 255, 255, 255, 0.25 );
      box-shadow: 0 8px 32px 0 rgba(82, 82, 82, 0.37);
      backdrop-filter: blur( 4px );
      -webkit-backdrop-filter: blur( 4px );
      border: 1px solid rgba( 255, 255, 255, 0.18 );
      display: flex;
      flex-direction: row;
      height: 70px;
    }
    h1 {
      justify-content: center;
      font-size: 22px;
      font-weight: 300;
      margin-right: auto;
      margin-left: 18px;
      margin-top: 20px;
    }
    img {
      margin-left: auto;
      margin-right: 18px;
      margin-top: 10px;
    }
</style>
