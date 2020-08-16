<template>
  <div class="back">
    <div id="nav-drawer">
      <input id="nav-input" type="checkbox" class="nav-unshown">
      <label id="nav-open" for="nav-input"><span></span></label>
      <label class="nav-unshown" id="nav-close" for="nav-input"></label>
      <div id="nav-content">
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
            <p><input type="submit" value="送信" /></p>
          </form>
        </div>
      </div>
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
  }
  h4 {
    text-align: center;
    font-size: 18px;
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
  .conteiner {
    margin: 40px 0 0 50px;
  }
  p {
    margin: 10px 0 10px 0;
    font-size: 18px;
  }
  a {
    text-decoration: none;
    color: white;
  }
  #nav-drawer {
    position: relative;
    padding-left: 10px;
    padding-top: 27px;
  }

/*チェックボックス等は非表示に*/
  .nav-unshown {
    display:none;
    margin-left: auto;/*右寄せ */
  }

/*アイコンのスペース*/
  #nav-open {
    display: inline-block;
    width: 30px;
    height: 22px;
    vertical-align: middle;
  }

  /*ハンバーガーアイコンをCSSだけで表現*/
  #nav-open span, #nav-open span:before, #nav-open span:after {
    position: absolute;
    height: 3px;/*線の太さ*/
    width: 25px;/*長さ*/
    border-radius: 3px;
    background: #555;
    display: block;
    content: '';
    cursor: pointer;
  }
  #nav-open span:before {
    bottom: -8px;
  }
  #nav-open span:after {
    bottom: -16px;
  }

  /*閉じる用の薄黒カバー*/
  #nav-close {
    display: none;/*はじめは隠しておく*/
    position: fixed;
    z-index: 99;
    top: 0;/*全体に広がるように*/
    left: 0;
    width: 100%;
    height: 100%;
    background: black;
    opacity: 0;
    transition: .3s ease-in-out;
  }

  /*中身*/
  #nav-content {
    overflow: auto;
    position: fixed;
    top: 0;
    right: 0;/*右端に固定*/ 
    z-index: 9999;/*最前面に*/
    width: 90%;/*右側に隙間を作る（閉じるカバーを表示）*/
    max-width: 390px;/*最大幅（調整してください）*/
    height: 100%;
    background: #fff;/*背景色*/
    transition: .3s ease-in-out;/*滑らかに表示*/
    -webkit-transform: translateX(-105%);
    transform: translateX(105%);/*右の外に隠しておく*/
    background-color: rgb(88, 53, 94);
  }

  /*チェックが入ったらもろもろ表示*/
  #nav-input:checked ~ #nav-close {
    display: block;/*カバーを表示*/
    opacity: .5;
  }

  #nav-input:checked ~ #nav-content {
    -webkit-transform: translateX(0%);
    transform: translateX(0%);/*中身を表示（右へスライド）*/
    box-shadow: 6px 0 25px rgba(0,0,0,.15);
  }
  @media screen and (max-width: 767px){
    h2 {
      margin-top: 30px;
      margin-left: 10px;
    }

    .conteiner {
      margin-left: 10px;
    }

      #nav-drawer {
      position: relative;
      padding: 0;
    }

  /*チェックボックス等は非表示に*/
    .nav-unshown {
      display:none;
    }

  /*アイコンのスペース*/
    #nav-open {
      display: inline-block;
      width: 30px;
      height: 22px;
      vertical-align: middle;
    }

    /*ハンバーガーアイコンをCSSだけで表現*/
    #nav-open span, #nav-open span:before, #nav-open span:after {
      position: absolute;
      height: 3px;/*線の太さ*/
      width: 25px;/*長さ*/
      border-radius: 3px;
      background: #555;
      display: block;
      content: '';
      cursor: pointer;
    }
    #nav-open span:before {
      bottom: -8px;
    }
    #nav-open span:after {
      bottom: -16px;
    }

    /*閉じる用の薄黒カバー*/
    #nav-close {
      display: none;/*はじめは隠しておく*/
      position: fixed;
      z-index: 99;
      top: 0;/*全体に広がるように*/
      left: 0;
      width: 100%;
      height: 100%;
      background: black;
      opacity: 0;
      transition: .3s ease-in-out;
    }

    /*中身*/
    #nav-content {
      overflow: auto;
      position: fixed;
      margin-top: 405px;
      left: 0;/*左端に固定*/ 
      z-index: 9999;/*最前面に*/
      width: 90%;/*右側に隙間を作る（閉じるカバーを表示）*/
      max-width: 320px;/*最大幅（調整してください）*/
      height: 50%;
      background: #fff;/*背景色*/
      transition: .3s ease-in-out;/*滑らかに表示*/
      -webkit-transform: translateX(-105%);
      transform: translateX(-105%);/*左の外に隠しておく*/
      background-color: rgb(88, 53, 94);
    }

    /*チェックが入ったらもろもろ表示*/
    #nav-input:checked ~ #nav-close {
      display: block;/*カバーを表示*/
      opacity: .5;
    }

    #nav-input:checked ~ #nav-content {
      -webkit-transform: translateX(0%);
      transform: translateX(0%);/*中身を表示（右へスライド）*/
      box-shadow: 6px 0 25px rgba(0,0,0,.15);
    }
  }
</style>