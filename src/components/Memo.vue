<template>
  <div class="back">
    <h2> {{ currentMonth }}月{{ renderDate }}日 </h2>
    <p v-for="(item,index) in commitDateList" :key="index"> {{ item }} </p>
    <div>
      <form @submit.prevent="submit">
        <p>アカウント名</p>
        <input type="text" v-model="accountName"/>
        <p>リポジトリ名</p>
        <input type="text" v-model="repoName"/>
        <p><input type="submit" value="送信" /></p>
      </form>
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
      },
      commitDateList:{
        require: true
      }
    },
    data(){
      return {
        renderDate:this.currentDate,
        accountName:"",
        repoName:""
      }
    },
    methods:{
      async submit(){
        // TODO inputタグに入れた値を引数にしてエミット
        this.$emit("submit", {account:this.accountName, repo:this.repoName})
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
  h2 {
    margin-top: 70px;
  }
</style>