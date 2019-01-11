<template>
<div @wheel="wheelHandel($event)">
  <transition-group tag="ul" class="fullpage-ul"
      :name="changeName"
      @after-enter="transitionEnd"
      @after-leave="transitionEnd"
      @enter = "enterHandle"
      @leave = "leaveHandle">
    <li v-for="list in pages" 
      v-show="list === showIndex"
      :key="list" class="fullpage-item" 
      :style="{'background-color': bgColors && bgColors[list-1]?bgColors[list-1]:'#a73'}">
      <slot :name="'slot'+list"  v-if="showIndex === list" :status="status"></slot>
      <h1>page-{{list}}</h1>
    </li>
  </transition-group>
</div>

</template>

<script>
export default {
  data(){
    return {
      showIndex: 1,
      changeName: 'full-down',
      wheelCount: 0,
      canWheel: true,
      status: null
    }
  },
  props:{
    pages: {
      type: Number,
      required: true
    },
    bgColors: {
      type: Array
    }
  },
  methods:{
    wheelHandel(event){
      if(!this.canWheel){
        return ;
      }
      if(event.deltaY > 0){
        this.changeName = 'full-down';
        if(this.showIndex < this.pages){
          this.showIndex ++;
          this.canWheel = false;
        }
      } else if(event.deltaY < 0){
        this.changeName = 'full-up';
        if(this.showIndex > 1){
          this.showIndex --;
          this.canWheel = false;
        }
      }
    },
    transitionEnd(){
      this.wheelCount ++;
      if(this.wheelCount === 2){
        this.wheelCount -=2;
        this.canWheel = true;
      }
    },
    enterHandle(){
      this.status = 'enter'
    },
    leaveHandle(el){
      this.status = 'leave'
    }
  }
}
</script>

<style>
  .fullpage-ul {
    margin: 0;
    padding: 0;
  }
  .fullpage-item {
    position: absolute;
    width: 100%;
    height: 100%;
    left: 0;
    top: 0;
    padding-top: 100px;
    box-sizing: border-box;
  }
  .fullpage-item h1,.fullpage-item h2 {
    text-align: center;
    line-height: 2;
  }

  .full-down-enter {
    transform: translateY(100%);
  }
  .full-down-enter-active,.full-down-leave-active {
    transition: all .8s;
  }
  .full-down-leave-to {
    transform: translateY(-100%);
  }

  .full-up-enter {
    transform: translateY(-100%);
  }
  .full-up-enter-active,.full-up-leave-active {
    transition: all .8s;
  }
  .full-up-leave-to {
    transform: translateY(100%);
  }
</style>


