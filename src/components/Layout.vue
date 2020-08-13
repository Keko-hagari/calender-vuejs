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
    <div class="side">
      <Memo
        :currentDate="currentDate"
        :currentMonth="currentMonth"
        :selectedDay="selectedDay"
        :commitDateList="commitDateList"
        @submit="submit($event)"
        :allDataList="allDataList"
      />
    </div>
  </div>
</template>
 
<script>
  import MoveBtn from './MoveBtn'
  import Calender from './Calender'
  import Memo from './Memo'
  export default {
    components:{
      MoveBtn,
      Calender,
      Memo
    },
    data (){
      return {
        selectedDay:"",
        currentYear:0,
        currentMonth:0,
        currentDate:0,
        commitDateList:[],
        allDataList:[]
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
        this.selectedDay = `${this.currentYear}-${this.transformNumber(this.currentMonth)}-${this.transformNumber(cellData.clickEvent.target.innerText)}`
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
      }
    }
  }
</script>

<style scoped>
.wrapper {
  display: flex;
  justify-content: space-around;
}
.main{
  width: 70%;
  height: 100%;
}
.side {
  width: 30%;
  height: 100%;
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
  