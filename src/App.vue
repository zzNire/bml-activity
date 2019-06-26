<template>
  <div id="app">
   <div class="menu">
       <my-tab :item="item" :index="index" v-for="(item,index) in menu" :key="item.str" ></my-tab>
   </div>
  <div class="other"></div>
  <div class="mouse-follow" :class="{hideFollow:!(selected_item+1)}">
    <sale-info   :time="followInfo" :follow_type="1" ></sale-info>
  </div>
  </div>

  
 
</template>

<script>
import MyTab from './components/tab.vue'
import SaleInfo from './components/sale_info.vue'
import {
  mapGetters
} from 'vuex'

/* eslint-disable */
export default {
  name: 'app',
  components: {
    MyTab,
    SaleInfo,
  },
  data() {
    return {
      menu: [{
        sale_info: {
          time: [7, 19],
          sale_time: [5, 20],
          str: '全息演唱会专场',
          str_en: 'BILIBILI MACRO LINK VISUAL RELEASE'
        },
        background_url: require('./assets/pictures/bili_vr_bg.jpg'),
        icon: require('./assets/pictures/bili_vr.png'),
        color: "#58b8ff",
      }, {
        sale_info: {
          time: [7, 20],
          sale_time: [5, 21],
          str: '主题线下聚会',
          str_en: 'BILIBILI MACRO LINK'
        },
        background_url: require('./assets/pictures/bili_link_bg.jpg'),
        icon: require('./assets/pictures/bili_link.png'),
        color: "#ffd196"
      }, {
        sale_info: {
          time: [7, 21],
          sale_time: [5, 22],
          str: '海外嘉宾专场',
          str_en: 'BILIBILI MACRO LINK STAR PHASE'
        },
        background_url: require('./assets/pictures/bili_sp_bg.jpg'),
        icon: require('./assets/pictures/bili_sp.png'),
        color: "#ffdd00"
      }],
      left: 10,

    }
  },
  computed: {
    ...mapGetters([
      'selected_item'
    ]),
    followInfo() {
      if (this.menu[this.selected_item]) {
        return this.menu[this.selected_item].sale_info;
      } else {
        return undefined;
      }
    }
  },
  mounted() {
    var Follow = function () {
      let OBJ = [],
        sp, rs, N = 0,
        m = {};

      function init(id, config) {
        this.config = config;
        this.obj = document.querySelector(`.${id}`);
        sp = this.config.speed || 4;
        rs = this.config.animR || 1;
        m = {
          x: 0,
          y: 0
        };

        this.setXY();
        this.start();
      }

      init.prototype.setXY = function () {
        document.getElementById('app').addEventListener('mousemove', function (e) {
          m.x = e.clientX;
          m.y = e.clientY;
        })
      }
      init.prototype.start = function () {
        function initOBJ(parent,cx,cy,dom){
          OBJ[N] = new CObj(parent, 0, 0, dom);
          N++;
          return OBJ[N-1];
        }

        var OO = null;
  debugger;
    let follow_dom = document.getElementsByClassName('mouse-follow')[0];
        let alert_dom_OBJ = OO = initOBJ(null,0,0,follow_dom.getElementsByClassName('alert')[0]);
        let activity_name_dom = follow_dom.getElementsByClassName('title_en')[0].children;
        for(let child of activity_name_dom){
          OO = initOBJ(OO,0,0,child);
        }
        let info_title_dom = OO = initOBJ(OBJ[4],0,0,follow_dom.getElementsByClassName('info-title')[0]);
        let time_info_dom = OO = initOBJ(OO,0,0,follow_dom.getElementsByClassName('time')[0]);
        /*for(let child of time_info_dom){
          OO = initOBJ(OO,0,0,child);
        }*/
        let sale_title_dom = OO = initOBJ(OO,0,0,follow_dom.getElementsByClassName('sale-title')[0])
        let sale_time_dom = OO = initOBJ(OO,0,0,follow_dom.getElementsByClassName('sale-time')[0]);
        /*for(let child of sale_time_dom){
          OO = initOBJ(OO,0,0,child);
        }
        */


        setInterval(function() {
				for (var i = 0; i < N; i++) OBJ[i].run();
        }, 16);
      }

      var CObj = function (p, cx, cy, dom) {
        this.css = dom.style;
        this.ddx = 0;
        this.ddy = 0;
        this.PX = 0;
        this.PY = 0;
        this.x = 0;
        this.y = 0;
        this.x0 = 0;
        this.y0 = 0;
        this.cx = cx;
        this.cy = cy;
        this.parent = p;
      }

      CObj.prototype.run = function () {
        if (!this.parent) {
          this.x0 = m.x;
          this.y0 = m.y;
        } else {
          this.x0 = this.parent.x;
          this.y0 = this.parent.y;

        }
        this.x = this.PX += (this.ddx += ((this.x0 - this.PX - this.ddx) + this.cx) / rs) / sp;
        this.y = this.PY += (this.ddy += ((this.y0 - this.PY - this.ddy) + this.cy) / rs) / sp;
        this.css.transform = `translate3D(${Math.round(this.x)}px,${ Math.round(this.y)}px,0)`
      }

      return init
    }();

    new Follow('mouse-follow', {
      speed: 6,
      animR: 2
    });
  },
  methods: {

  },

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

.hideFollow{
  opacity: 0;
}
</style>
