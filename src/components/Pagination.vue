<template>
  <div class="pagination">
    <button @click="changeBtn">首页</button>
    <button @click="changeBtn">上一页</button>
    <button class="pageBtn" v-if="judge" @click="backwards">......</button>
    <button v-for="btn in pageBtns" @click="changeBtn(btn)" :class="[{currentPage: btn == currentPage}, 'pageBtn']">{{btn}}</button>
    <button class="pageBtn" @click="forwards">......</button>
    <button @click="changeBtn">下一页</button>
  </div>
</template>

<script>
import $ from 'jquery'
export default {
  name: 'Pagination',
  data(){
    return {
      pageBtns: [1,2,3,4,5],
      currentPage: 1,
      judge: false
    }
  },
  methods: {
    judgement(){
      if(this.pageBtns[0] > 1){
        this.judge = true
      }else{
        this.judge = false
      }
    },
    changeBtn(page){
      if(typeof page != 'number'){
        switch(page.target.innerText){
          case '上一页': 
            $('button.currentPage').prev().click();
            break;
          case '下一页':
            $('button.currentPage').next().click();
            break;
          case '首页': 
            this.pageBtns = [1,2,3,4,5];
            this.changeBtn(1);
            break;
        }
        return
      }
      this.currentPage = page
      if(page == this.pageBtns[4]){
        this.pageBtns.shift()
        this.pageBtns.splice(4,0,this.pageBtns[3]+1)
      }else if(page == this.pageBtns[0] && page > 1){
        this.pageBtns.unshift(this.pageBtns[0]-1)
        this.pageBtns.splice(5,1)
      }
      this.judgement()
      this.$emit('handleList', this.currentPage)
    },
    backwards(){
      this.pageBtns = []
      if(this.currentPage < 5){
        this.pageBtns = [1,2,3,4,5]
      }else{
        for(var i=0; i<5; i++){
          this.pageBtns.unshift(this.currentPage-i)
        }
      }
      this.judgement()
    },
    forwards(){
      this.pageBtns = []
      for(var i=0; i<5; i++){
        this.pageBtns.push(this.currentPage+i)
      }
      this.judgement()
    } 
  }
}
</script>

<style scoped>
  .pagination {
    margin-top: 5px;
    margin-bottom: 20px;
    background-color: white;
    padding: 6px 20px;
    border-radius: 5px;
    border: 1px solid #888888;
  }

  button {
    background-color: #fff;
    border: 1px solid #ddd;
    color: #778087;
    border-radius: 3px;
    outline: none;
    cursor: pointer;
    padding: 0 2px;
    width: 55px;
    height: 29px;
  }
  button:not(.currentPage):hover {
    background-color: rgba(44, 40, 40, 0.3)
  }
  .pageBtn {
    position: relative;
    bottom: 1px;
    width: 40px;
    margin: 0 4px;
  }

  .currentPage {
    color: white;
    background-color: #1f1b1b;
  }
</style>
