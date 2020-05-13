<template>
    <div class="recommend">
        <div class="reitem" v-for="(item, index) in recommends" :key="index">
            <a :href="item.link">
                <img :src="item.image" alt="">
                <div>{{item.title}}</div>
            </a>
        </div>
    </div>
</template>

<script>
    export default {
        name: "HomeRecommend",
        props: {
            recommends: {
                type: Array,
                default(){
                    return []
                }
            }
        },
    }
</script>

<style scoped>
    .recommend{
        display: flex;
        width: 100%;
        text-align: center;
        padding: 15px 0 15px;
        border-bottom: 10px solid #dbdbdb;
    }
    .reitem{
        flex: 1;
    }
    .reitem img{
        width: 70px;
        height: 70px;
        margin-bottom: 3px;
    }
</style>