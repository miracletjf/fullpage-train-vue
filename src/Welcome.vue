<template>
  <div  class="wel-text" >
    <span v-html="showTexts.join('')"></span>
    <span v-show="cursorShow">|</span>
  </div>
</template>

<script>
export default {
  data(){
    return {
      textArray: ('Hello').split('').concat(['&nbsp;','&nbsp;','{','<br>','&nbsp;','&nbsp;','&nbsp;','&nbsp;'],('MiracleTang').split(''),['<br>','}','<br>']),
      showTexts: [],
      pushIndex: 0,
      appendTimer: 0,
      cursorTimer: 0,
      cursorShow: false
    }
  },
  props: ['status'],
  created(){
    this.start(); 
  } ,
  methods:{
    start(){
      this.appendTimer = setInterval(this.appendText,100);
      this.cursorTimer = setInterval(this.cursorJump,80);
    },
    appendText(){
      this.showTexts.push(this.textArray[this.pushIndex]);
      if(this.pushIndex < this.textArray.length-1){
        this.pushIndex++;
      }else{
        this.clear();
      }
    },
    cursorJump(){
      this.cursorShow = !this.cursorShow;
    },
    clear(){
      clearInterval(this.appendTimer);
      clearInterval(this.cursorTimer);
    }
  },
  watch: {
    status(){
      if(this.status === 'enter'){
        this.start()
      }else if(this.status === 'leave'){
        this.clear();
        this.showTexts = [];
        this.pushIndex = 0;
      }
    }
  }
}
</script>

<style>
.wel-text {
  position: absolute;
  left: 100px;
  top: 100%;
  margin-top: -200px;
  z-index: 10;
  font-size: 30px;
  line-height: 1.8;
  color: #333;
  font-weight: bold;
}
</style>
