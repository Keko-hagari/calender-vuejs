<template>
  <div class="wrapper">
    <div class="main">
      <MoveBtn
        :currentYear="currentYear"
        :currentMonth="currentMonth"
        @movePrevMonth="movePrevMonth"
        @moveNextMonth="moveNextMonth"
      />
      <Calender
        :currentYear="currentYear"
        :currentMonth="currentMonth"
        :currentDate="currentDate"
        :selectedDay="selectedDay"
        @selectCell="selectCell($event)"
        :commitDateList="commitDateList"
      />
    </div>
    <div :class="checkSideFlag">
      <Memo
        :currentDate="currentDate"
        :currentMonth="currentMonth"
        :selectedDay="selectedDay"
        :commitDateList="commitDateList"
        @submit="submit($event)"
        :allDataList="allDataList"
      />
    </div>
    <div @click="transformMenu" class="drawer"><img :class="transformBtn" :src="plus"></div>
  </div>
</template>
 
<script>
  import MoveBtn from './MoveBtn'
  import Calender from './Calender'
  import Memo from './Memo'
  import Plus from '@/assets/plus.svg' 
  export default {
    components:{
      MoveBtn,
      Calender,
      Memo,
    },
    data (){
      return {
        selectedDay:"",
        currentYear:0,
        currentMonth:0,
        currentDate:0,
        commitDateList:[],
        allDataList:[],
        openFlag: false,
        plus: Plus
      }
    },
    computed:{
      checkSideFlag(){
        return this.openFlag ? "side open" : "side close"
      },
      transformBtn(){
        return this.openFlag ? "draw-img-close" : "draw-img"
      }
    },
    created(){
      const date = new Date();
      [this.currentYear,  this.currentMonth, this.currentDate] = [date.getFullYear(), date.getMonth() + 1, date.getDate()];
    },
    methods:{
      movePrevMonth(){
        this.currentMonth = this.currentMonth != 1 ? this.currentMonth - 1 : 12;
        this.currentYear = this.currentMonth != 12 ? this.currentYear : this.currentYear - 1;
      },
      moveNextMonth(){
        this.currentMonth = this.currentMonth != 12 ? this.currentMonth + 1 : 1;
        this.currentYear = this.currentMonth != 1 ? this.currentYear : this.currentYear + 1;
      },
      selectCell(cellData){
        if((cellData.clickEvent.target.innerText) != 0){
          this.selectedDay = `${this.currentYear}-${this.transformNumber(this.currentMonth)}-${this.transformNumber(cellData.clickEvent.target.innerText)}`
          console.log(cellData.clickEvent.target.innerText)
        }
      },
      transformNumber(number){
        return number < 10 ? `0${number}` : number
      },
      submit(e){
        this.axios.get(`https://api.github.com/repos/${e.account}/${e.repo}/commits`)
          .then((response) => {
            this.allDataList = this.commitDateList = response.data
            const tmp = []
            // tmpに日付を文字列で追加
            this.commitDateList.map((obj) => {
              tmp.push(obj.commit.committer.date.slice(0,10))
            })
            this.commitDateList = tmp
          })
          .catch((e) => {
            alert(e);
          });
      },
      transformMenu(){
        this.openFlag = !this.openFlag
      }
    }
  }
</script>

<style scoped>
.draw-img {
  height: 30px;
  transition: all .5s;
}
.draw-img-close{
  height: 30px;
  transform: rotate(405deg);
  transition: all .5s;
}
.wrapper {
  width: 100%;
  height: 100vh;
  overflow: hidden;
  position: relative;
}
.main{
  width: 100%;
  height: 100%;
}
.side {
  height: 100%;
  position:absolute;
  top: 0;
  bottom: 0;
  padding: 0 20px;
  background-color: rgb(88, 53, 94);
  transition: all .5s;
}
.close {
  right: -500px;
}
.open {
  right: 0;
}
.drawer {
  position:absolute;
  right: 20px;
  top: 20px;
  font-size: 20px;
}
@media screen and (max-width: 767px){
  .wrapper {
    flex-direction: column;
  }
  .main {
    width: 100%;
  }
  .side {
    width: 100%;
  }
}
</style>
  