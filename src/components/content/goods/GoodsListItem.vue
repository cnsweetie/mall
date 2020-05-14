<template>
    <div class="goods" @click="itemClick">
        <img :src="goods.show.img"  alt="" >
        <div class="goods-info">
            <p>{{goods.title}}</p>
            <span class="price">¥{{goods.price}}</span>
            <span class="collect">{{goods.cfav}}</span>
        </div>
    </div>
</template>

<script>
    export default {
        name: "GoodsListItem",
        props: {
            goods: {
                type: Object,
                default() {
                    return {}
                }
            }
        },
        methods: {
            itemClick () {
                let iid = this.goods.iid
                this.$router.push('/detail/' + iid)
            }
        },


    }
</script>

<style scoped>
    .goods {
        padding-bottom: 20px;
    }
    .goods img {
        width: 100%;
        height: 92%;
        border-radius: 5px;
    }

    .goods-info {
        font-size: 12px;
        position: relative;
        top: 10px;
        left: 0;
        right: 0;
        bottom: 5px;
        overflow: hidden;
        text-align: center;
    }

    .goods-info p {
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
        margin-bottom: 3px;
    }

    .goods-info .price {
        color: var(--color-high-text);
        margin-right: 20px;
    }

    .goods-info .collect {
        position: relative;
    }

    .goods-info .collect::before {
        content: '';
        position: absolute;
        left: -15px;
        top: 0;
        width: 14px;
        height: 14px;
        background: url("~assets/img/common/collect.svg") 0 0/13px 13px;
    }
</style>