<template>
  <div class="back">
    <h2> {{ currentMonth }}月{{ renderDate }}日 </h2>
    <div>
    </div>
  </div>
</template>

<script>
  export default {
    props:{
      currentMonth:{
        require: true
      },
      currentDate:{
        require: true
      },
      selectedDay:{
        require: true
      }
    },
    data(){
      return {
        renderDate:this.currentDate
      }
    },
    watch:{
      selectedDay:function(){
        this.renderDate = this.selectedDay.slice(8)
      }
    }
  }
</script>

<style scoped>
  .back {
  }
  h2 {
    margin-top: 70px;
  }
</style>