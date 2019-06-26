<template>
    <div class="sale-info">
        <p v-if="follow_type" class="alert follow">查看详情</p>
        <p v-if="follow_type" class="title_en">
            <span  class="follow" >{{flatWords[0]}} </span>
            <span  class="follow" >{{flatWords[1]}} </span>
            <span  class="follow" >{{flatWords[2]}} </span>
            <br v-if="flatWords[3]!==''">
            <span  class="follow" >{{flatWords[3]}} </span>
            <span  class="follow" >{{flatWords[4]}} </span>
           
        </p>
      
        <p class="info-title follow">{{time.str}}</p>
        <p class='time follow'>
            <span class="time-num">{{time.time[0]|fillNum}}</span>
            <span class="time-dscrp">月</span>
            <span class="time-num">{{time.time[1]|fillNum}}</span>
            <span class="time-dscrp">日</span>
        </p>
        <p class='sale-title follow'>参考预售时间 </p>
        <p class='sale-time follow'>
            <span class="sale-time-num">{{time.sale_time[0]|fillNum}}</span>
            <span class="time-dscrp">月</span>
            <span class="sale-time-num">{{time.sale_time[1]|fillNum}}</span>
            <span class="time-dscrp">日</span>
        </p>
    </div>
</template>

<script>
export default {
    props:{
        time:{
            type:Object,
            default:function(){
                return {
                    time:['',''],
                    sale_time:['',''],
                    str:'',
                    str_en:''
                };
            }
        },
        follow_type:{
            type:Number,
            default:0,
        }
    },
    computed:{
        flatWords(){
            if(!this.time.str) return ['','','','',''];
            var word = this.time.str_en.split(' ');
            return word.length === 3 ? word.concat(['','']): word;
        }
    },
    filters:{
        fillNum(num){
            return Math.floor(num/10) === 0 ? '0'+num : num;
        },
        
    },
    
}
</script>

<style scoped>
.sale-info{
    text-align: left;
    position: relative;
    color:white;
}

.alert{
    font-size: 1vw;
    color: gray;
}

.title_en{
    font-size:5.5vh;
    font-weight: bolder;
}

.title_en span {
    display: inline-block;
    margin-right: 2vh;
}

.info-title{
    font-size: 1vw;
}

.time-num{
    display: inline-block;
    font-size:3vw;
    font-weight: bolder;
}

.sale-title{
    font-size: 1vw;
    font-weight: bolder;
}
.sale-time{
 
}
.sale-time-num{
    display: inline-block;
    font-weight: bolder;
    font-size:3vw;
    color: rgba(0,0,0,0);
    -webkit-text-stroke:1px white;
   
}
.time-dscrp{
    display: inline-block;
    font-size: 1vw;
    margin: 0 1vw;
}

span.follow{
    display: inline-block;
}
</style>
