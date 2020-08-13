<template>
  <div class="back">
    <h2> {{ currentMonth }}月{{ renderDate }}日 </h2>
    <h4>commit message 一覧</h4>
    <div class="box">
      <a :href="`https://github.com/${accountName}/${repoName}`" target=”_blank”>
        <p v-for="(item,index) in allDataList" :key="index"> {{ item.commit.message}} ({{ item.commit.committer.date.slice(5,10) }}) </p>
      </a>
    </div>
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
      allDataList:{
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
  .box {
    width: 300px;
    height: 250px;
    overflow: scroll;
    border: solid 1px;
    border-radius: 20px;
  }
  .box p {
    margin-left: 10px;
  }
  p {
    margin: 10px 0 10px 0;
  }
  a {
    text-decoration: none;
    color: black;
  }
</style>