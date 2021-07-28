<template>
    <div class="pagination">
        <button @click="select(-1)">上一页</button>
        <span @click="selectNum(item)" :class="['pagination_item',item==activeIndex?'active':'']" v-for="(item,index) in nums" :key="index">{{item}}</span>
        <button @click="select(1)">下一页</button>
    </div>
</template>

<script>
    export default {
        data(){
            return{
                activeIndex:0,
                pagerCount:5,
                nums:[],
                timer:null
            }
        },
        props:{
            total:{
                type:Number,
                default:0
            },
            pageSize:{
                type:Number,
                default:0
            },
            defaultIndex:{
                type:Number,
                default:1
            }
        },
        mounted(){
            this.activeIndex=this.defaultIndex
        },
        methods:{
            initNums(middle,isEmit){
                var max=Math.ceil(this.total/this.pageSize)
                var average= parseInt((this.pagerCount)/2)
                if(middle<=1)middle=1
                if(middle>=max)middle=max
                if(this.activeIndex==middle&&!isEmit)return;

                var end=middle+average
                if(end<=this.pagerCount)end=this.pagerCount
                if(end>=max)end=max
                
                var start= end-this.pagerCount+1
                if(start<=1)start=1

                this.activeIndex=middle
                this.nums=[]
                for(var i=start;i<=end;i++){
                    this.nums.push(parseInt(i))
                }
                clearTimeout(this.timer)
                this.timer=setTimeout(()=>{
                    this.$emit('change',this.activeIndex)
                },300)
            },
            select(type){
                this.initNums(this.activeIndex+type)
            },
            selectNum(num){
               this.initNums(num) 
            }
        },
        watch:{
            defaultIndex(v){
                this.initNums(v);
            },
            pageSize(){
                this.initNums(this.activeIndex,true);
            },
            total(){
                this.initNums(this.activeIndex,true);
            }
        }
    }
</script>
<style>
.pagination{
    display: flex;
}
.pagination_item{
    background: gray;
    margin: 0px 3px;
    display: flex;
    padding: 0px 10px;
    box-sizing: border-box;
    min-width: 10px;
    height: 60px;
    align-items: center;
    cursor: pointer;
}
.pagination_item.active{
    background: red;
}
</style>