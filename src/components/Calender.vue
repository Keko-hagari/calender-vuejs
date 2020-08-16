<template>
  <div class="">
    <span class="btn-model-close" @click="close($event)"></span>
    <div class="calendar-body">
      <div class="calendar-body item">
        <div v-for="day in weeks" class="day" :key="day">{{day}}</div>
      </div>
      <div class="calendar-body item">
        <div :class="checkSelectedDay(day)" v-for="day in calendarMake" :value="day" :key="day" @click="selectCell">
          <span :class="checkToday(day)">{{day}}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    data(){
      return{
        today:"",
        // selectedDay:"",
        // currentYear:0,
        // currentMonth:0,
        // currentDay:0,
        weeks:["月","火","水","木","金","土","日"],
        calendar:[],
        holidays:[]
      }
    },
    props:{
      currentYear:{
        require: true
      },
      currentMonth:{
        require: true
      },
      currentDate:{
        require: true
      },
      selectedDay:{
        require: true
      },
      commitDateList:{
        require: true
      }
    },
    created(){
      // const date = new Date();
      // [this.currentYear,  this.currentMonth, this.currentDate] = [date.getFullYear(), date.getMonth() + 1, date.getDate()];
      this.today = this.selectedDay = `${this.currentYear}-${('0' + this.currentMonth).slice(-2)}-${this.currentDate}`;
    },
    methods:{
      checkSelectedDay(day){
        return{
          'selectedDay':`${this.currentYear}-${('0' + this.currentMonth).slice(-2)}-${('0' + day).slice(-2)}` == this.selectedDay
        }
      },
      checkToday(day){
        // this.commitDateListをループさせてその要素と`${this.currentYear}-${('0' + this.currentMonth).slice(-2)}-${('0' + day).slice(-2)}`と全て比べる
        // くらべた中のtrueのものは文字列でcommitDateを返す
        // ループを抜けた後に
        // if (`${this.currentYear}-${('0' + this.currentMonth).slice(-2)}-${('0' + day).slice(-2)}` == this.today){
        //   return "today"
        // } を実行する
        // cssにcommitDateのスタイルをかく
        for(let i = 0; i < this.commitDateList.length; i++) {
          if ((`${this.currentYear}-${('0' + this.currentMonth).slice(-2)}-${('0' + day).slice(-2)}` === this.commitDateList[i])
              &&
              (`${this.currentYear}-${('0' + this.currentMonth).slice(-2)}-${('0' + day).slice(-2)}` === this.today)){
            return "commitDate today"
          } else if (`${this.currentYear}-${('0' + this.currentMonth).slice(-2)}-${('0' + day).slice(-2)}` === this.commitDateList[i]){
            return "commitDate"
          }
        }
        if (`${this.currentYear}-${('0' + this.currentMonth).slice(-2)}-${('0' + day).slice(-2)}` === this.today){
          return "today"
        }
      },
      selectCell(cellData){
        // this.selectedDay = `${this.currentYear}-${this.transformNumber(this.currentMonth)}-${this.transformNumber(cellData.target.innerText)}`
        // console.log(this.selectedDay)
        this.$emit('selectCell',{clickEvent: cellData})
      }
    },
    computed:{
      calendarMake(){
        const firstday = new Date(this.currentYear, this.currentMonth - 1, 1).getDay();
        const lastdate = new Date(this.currentYear, this.currentMonth, 0).getDate();
        const necessarySpace = firstday == 0 ? 6 : firstday - 1;
        const list = [[...Array(necessarySpace)].map(()=>" "), [...Array(lastdate)].map((_, i) => i+1)];
        const week = list.reduce((pre,current) => {pre.push(...current);return pre},[]);
        return week;
      },
    }
  }
</script>

<style lang="css">

  .calendar-body{
    width:100%;
    table-layout:fixed;
    margin-bottom: 20px;
    margin-top: 20px;
    color: #565656;
    font-size: 1.1rem;
    text-align: center;
  }
  .item{
    display: flex;
    flex-wrap: wrap;
  }
  @keyframes scaleup{
  0% {
    transform: scale(1.02);
  }
  100% {
    transform: scale(1);
  }
  }
    .item div:hover{
    box-shadow: 0 0 5px grey;
    transform: scale(1);
    animation-name: scaleup;
    animation-duration: .7s;
    animation-iteration-count: infinite;
  }
 .item div{
    box-sizing: border-box;
    width: 14.28%;
    min-height: 90px;
    padding: 12px 0;
    text-align: center;
    cursor:pointer;
    font-size: 18px;
    border: solid 1px white;
    border-radius: 10px;
  }
  .item div:nth-of-type(7n-1){
    color: #758efd;
  }
  .item div:nth-of-type(7n){
    color:#e03616;
  }
  .item div span{
    vertical-align: -25px;
  }
  .today{
    display:inline-block;
    width: 24px;
    height: 24px;
    border-radius: 50%;
    background-color:#e03616;
    color: #fff !important; 
    line-height: 24px;     
  }
  .commitDate{
    display:inline-block;
    width: 24px;
    height: 24px;
    border-radius: 50%;
    border: solid 2px green;
    line-height: 24px;     
  }
  .selectedDay{
    display:inline-block;
    background-color: rgba(0, 42, 255, 0.7);
    border-radius: 10px;
    color: #fff !important; 
  }
  .holidays{
      color: #e03616;
  }
  @media screen and (max-width: 767px){
    .item div span{
      vertical-align: 0;
    }
    .item div {
      min-height: 0;
    }
  }
</style>