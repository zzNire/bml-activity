<template>
  <div id="app" @mousemove="mouseMove">
   <div class="menu">
       <my-tab :item="item" :index="index" v-for="(item,index) in menu" :key="item.str" ></my-tab>
   </div>
  <div class="other"></div>
  <div class="mouse-follow">
    <sale-info   :time="this.menu[1].sale_info" :follow_type="1"></sale-info>
  </div>
  </div>

  
 
</template>

<script>
import MyTab from './components/tab.vue'
import SaleInfo from './components/sale_info.vue'
import {prefixStyle} from './common/dom.js'
import {mapGetters} from 'vuex'

const tranform = prefixStyle('transform')
/* eslint-disable */
export default {
  name: 'app',
  components: {
    MyTab,
    SaleInfo,
  },
  data(){
    return {
      menu:[{
        sale_info:{
          time:[7,19],
          sale_time:[5,20],
          str:'全息演唱会专场',
          str_en:'BILIBILI MACRO LINK VISUAL RELEASE'
        },
        background_url: require('./assets/pictures/bili_vr_bg.jpg'),
        icon: require('./assets/pictures/bili_vr.png'),
        color:"#58b8ff",
      },{
       sale_info:{
          time:[7,20],
          sale_time:[5,21],
          str:'主题线下聚会',
          str_en:'BILIBILI MACRO LINK'
        },
        background_url:require('./assets/pictures/bili_link_bg.jpg'),
        icon:require('./assets/pictures/bili_link.png'),
        color:"#ffd196"
      },{
        sale_info:{
          time:[7,21],
          sale_time:[5,22],
          str:'海外嘉宾专场',
          str_en:'BILIBILI MACRO LINK STAR PHASE'
        },
        background_url:require('./assets/pictures/bili_sp_bg.jpg'),
        icon:require('./assets/pictures/bili_sp.png'),
        color:"#ffdd00"
      }],
      left:10,
      timer:null,
      pageOldX:0,
      pageOldY:0,
    }
  },
  computed:{
    ...mapGetters([
      'selected_item'
    ])
  },
  methods:{
    //generator 异步任务 自动机
    * delyFollow(doms,e) {
      var time = 100;
      var speed =3;
      var count =0;
      for(var dom of doms){
          if(count>4){
            time = 50;
          }
          count++;
          var x = yield  new Promise((resolve,reject)=>{setTimeout(()=>{
            dom.style[tranform] = `translate3D(${e.pageX+this.left}px,${e.pageY}px,0)`;
           // if(Math.abs(e.pageX - this.pageOldX) >200 ||Math.abs(this.pageOldY- e.pageY) >200){
              dom.style.transition = `transform .1s ease-in-out`;
            //}else{
           //    dom.style.transition = '';
          //  }
            resolve(1);
            },time);
          })
          this.pageOldX = e.pageX;
          this.pageOldY = e.pageY;
      }
    },
    run(fun,...args){
      var gen = fun(...args);
      function next(data){
        var result = gen.next();
        if(result.done) return 
        result.value.then((data)=>{
          next(data);
        })
      }
      next(gen);
    },
    mouseMove(e){
      // eslint-disable-next-line
     
      if(!this.timer){
         this.timer = setTimeout(()=>{
         console.log(e.pageX,e.pageY) ;
      var follower = [...document.getElementsByClassName('mouse-follow')];
      var follow = follower[0].getElementsByClassName('follow');
      //run(delyFollow,follow,e);
      this.run(this.delyFollow,follow,e);
      this.timer = null;
      },16)
      }
      
     
      // eslint-disable-next-line
      
    }
  }

}
</script>

<style>
#app {
  position: relative;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  overflow: hidden;

}

.menu{
  display: flex;
  height: 100vh;
  width: 90vw;
  
}

.other{
  position: absolute;
  top:0;
  right:0;
  width: 10vw;
  height: 100vh;

}

.mouse-follow{
  position: absolute;
  left: 0;
  top:0;
}
</style>
