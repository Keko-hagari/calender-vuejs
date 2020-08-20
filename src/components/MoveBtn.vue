<template>
  <div class="calendar-title">
     <span class="btn-monthMove prev fa fa-angle-left" @click="movePrevMonth"><img :src="left"></span>
     {{currentYear+"/"+currentMonth}}
     <span class="btn-monthMove next fa fa-angle-right" @click="moveNextMonth"><img :src="right"></span>
   </div>
</template>
 
<script>
  import Right from "@/assets/to-right.svg"
  import Left from "@/assets/to-left.svg"
  export default {
    data(){
      return {
        right: Right,
        left: Left
      }
    },
    props:{
      currentYear:{
        require: true
      },
      currentMonth:{
        require: true
      },
    },
    methods:{
      movePrevMonth(){
        // this.currentMonth = this.currentMonth != 1 ? this.currentMonth - 1 : 12;
        // this.currentYear = this.currentMonth != 12 ? this.currentYear : this.currentYear - 1;
        this.$emit('movePrevMonth')
      },
      moveNextMonth(){
        // this.currentMonth = this.currentMonth != 12 ? this.currentMonth + 1 : 1;
        // this.currentYear = this.currentMonth != 1 ? this.currentYear : this.currentYear + 1;
        this.$emit('moveNextMonth')
      },
    }
  }
</script>

<style scoped>
.calendar-title{
  text-align: center;
  margin-top: 20px;
  font-size: 23px;
}
img {
  width: 16px;
}
img:hover {
  width: 16px;
  transform: scale(1.2,1.2);
}

</style>