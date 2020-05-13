<template>
    <div class="tabcontrol">
        <div class="tbitem"
             v-for="(item,index) in title" :key="index"
             :class="{active: index === currentindex}"
             @click="itemClick">
            <span>{{item}}</span>
        </div>
    </div>
</template>

<script>
    export default {
        name: "TabControl",
        props: {
            title: {
                type: Array,
                default(){
                    return []
                }
            }
        },
        data() {
            return{
                currentindex: 0
            }
        },
        methods: {
            itemClick(index){
                this.currentindex = index
            }
        }
    }
</script>

<style scoped>
    .tabcontrol{
        display: flex;
        text-align: center;
        line-height: 40px;
        background-color: #fff;
    }
    .tbitem{
        flex: 1;
    }
    .tbitem span{
        padding: 5px;
    }
    .active{
        color: #ff9aa8;
    }
    .active span{
        border-bottom: 2px solid #ff9aa8;
    }
</style>