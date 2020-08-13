<template>
  <div class="load">
    <h2>calender</h2>
  </div>
</template>

<script>
export default {
  props: {
    
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  @keyframes identifier {
    0% {
      opacity: 0;
    }
    50% {
      opacity: 1;
    }
    100% {
      opacity: 0;
    }
  }
  .load { 
    background-color: #5998c5;
    width: 100%;
    height: 100%;
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
  }
  h2 {
    color: white;
    text-align: center;
    font-size: 60px;
    margin-top: 300px;
    animation-name: identifier;
    animation-duration: 3s;
    animation-timing-function: linear;
  }
</style>
