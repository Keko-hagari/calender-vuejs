<template>
  <div class="back">
    <h2> {{ currentMonth }}月{{ renderDate }}日 </h2>
    <h4>commit message 一覧</h4>
    <div class="box">
      <a :href="`https://github.com/${accountName}/${repoName}`" target=”_blank”>
        <p v-for="(item,index) in allDataList" :key="index"> {{ item.commit.message}} ({{ item.commit.committer.date.slice(5,10) }}) </p>
      </a>
    </div>
    <div class="conteiner">
      <form @submit.prevent="submit">
        <p>アカウント名</p>
        <input type="text" v-model="accountName"/>
        <p>リポジトリ名</p>
        <input type="text" v-model="repoName"/>
        <br>
        <input type="submit" value="送信" />
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
    text-align: center;
    color: rgb(240, 155, 139);
  }
  h4 {
    text-align: center;
    font-size: 18px;
    color: rgb(240, 155, 139);
    margin: 10px;
  }
  .box {
    width: 300px;
    height: 250px;
    overflow: scroll;
    border: solid 1px;
    border-radius: 20px;
    margin: auto;
  }
  .box p {
    margin-left: 10px;
  }
  
  p {
    font-size: 18px;
    color: rgb(240, 155, 139);
    margin: 5px 0;
  }
  a {
    text-decoration: none;
    color: rgb(240, 155, 139);
  }
  input[type=submit] {
    padding: 1% 3%;
    border-radius: 5px;
    border: none;
    display: inline-block;
    margin: 20px auto;
    width: 100%;
    background-color: white;
    color: rgb(88, 53, 94);
    font-weight: bold;
  }
  form {
    padding: 2%;
  }
  input[type=text] {
    width: 100%;
    height: 30px;
    border-radius: 5px;
    border: solid 1px gray;
    padding: 1% 3%;
    color: gray;
    font-weight: bold;
  }
  @media screen and (max-width: 767px){
    h2 {
      margin-top: 30px;
      margin-left: 10px;
    }

    .conteiner {
      margin-left: 10px;
    }
  }
</style>