<template>
    <div class="tab" :class="{mouseEnter:isMouseEnter}" :style="{backgroundImage:`url(${item.background_url})`}" >
        <div class="front-bg" 
        @mouseenter="mouseEnter"
        @mouseleave="mouseLeave"
        @touchmove='touchMove'></div>
        <div class="background" :class="{bgEnter:isMouseEnter,otherEnter:selected_item !== this.index && selected_item !== -1}" >
        <div class="tab-icon">
            <img :src="item.icon">
        </div>
        <div class="sale-icon" :style="{background:`linear-gradient(90deg, rgba(255, 209, 150, 0), ${item.color})`}">
            <span class="sale-str">前往预售 > </span>
            <img class="sale-btn" src="">
        </div>
         <transition name='sale-info'>
        <div class="sale-information">
            <sale-info :time="item.sale_info"></sale-info>
        </div>
         </transition>
    </div>
    </div>
</template>

<script>
import SaleInfo from './sale_info.vue'
import {mapGetters,mapMutations} from 'vuex'
export default {
    components:{
        SaleInfo,
    },
    props:{
        item:{
            type:Object,
            default:function(){
                return -1;
            },
        },
        index:{
            type:Number,
            default:-1,
        }
    },
    data(){
        return{
            isMouseEnter:false,
        }
    },
    methods:{
        ...mapMutations([
            'setSelectItem'
        ]),
        mouseEnter(){
            this.isMouseEnter = true;
            this.setSelectItem(this.index);
        },
        mouseLeave(){
            this.isMouseEnter = false;
            this.setSelectItem(-1);
        },
        touchMove(e){
            console.log(e.touchTargets[0])
           
        }
        
    },
    computed:{
        ...mapGetters([
            'selected_item'
        ])
    }
}
</script>

<style scoped>
.tab{
    position: relative;
    width:33.333vw;
    height: 100vh;
    background-size: cover;
    background-position: 50% 50%;
    transition: all .3s ease-in-out, -webkit-flex-basis .3s ease-in-out, -webkit-flex-basis .3s ease-in-out;
   
}

.tab:hover .sale-information{
    opacity: 0;
    transition: all .3s ease-in-out;
}

.mouseEnter{
    width:40vw; 
    transition: all .3s ease-in-out, -webkit-flex-basis .3s ease-in-out, -webkit-flex-basis .3s ease-in-out;
}

.front-bg{
    position: absolute;
    top:0;
    left:0;
    height: 100%;
    width: 100%;
    opacity: 0;
    z-index:1;
}
.background{
    height: 100%;
    width: 100%;
    background-color: rgba(0,0,0,0); 
    transition: background-color 0.3s ease-in-out;
}

.bgEnter{
    background-color: rgba(0,0,0,0.7);
    transition: background-color .3s ease-in-out
}

.otherEnter{
    background-color: rgba(0,0,0,0.4);
    transition: background-color .3s ease-in-out
}
.tab-icon{
    position: absolute;
    right: 2vh;
    top:12.8vh;
}

.tab-icon img{
    width: 12vw;
}

.sale-icon {
    position: absolute;
    right: 0px;
    top:24.5vh;
    width:83%;
    height:6vh;
    line-height: 6vh;
    font-size: 1vw;
    text-align: right;
    margin: 0;
    padding-right: 5%;
    
}

.sale-information{
    position: absolute;
    top: 53.8vh;
    right: 0;
}

 .sale-info-enter
  {
    opacity: 0;
  }

.sale-info-leave-to{
   opacity: 0;
}
  .sale-info-enter-active,
  .sale-info-leave-active {
    transition: all 0.3s ease-in-out;
  }

</style>
