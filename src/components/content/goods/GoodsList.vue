<template>
    <div class="good">
        <goods-list-item v-for="(item, index) in goodsList" :key="index" :goods="item"></goods-list-item>
    </div>

</template>

<script>
    import GoodsListItem from './GoodsListItem'

    export default {
        name: "GoodsList",
        components: {
            GoodsListItem
        },
        props: {
            goodsList: {
                type: Array,
                default(){
                    return []
                }
            }
        },
    }
</script>

<style scoped>
    .good{
        display: flex;
        flex-wrap: wrap;
        justify-content: space-around;
    }
</style>