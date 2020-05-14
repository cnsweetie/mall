<template>
    <div id="detail">
        <detail-nav-bar class="detailnavbar"/>
        <scroll class="content">
            <detail-swiper :top-images="topImages"></detail-swiper>
            <detail-base-info :goods="goods"></detail-base-info>
            <detail-shop-info :shop="shop"></detail-shop-info>
            <detail-goods-info :detail-info="detailInfo"></detail-goods-info>
            <detail-param-info :param-info="paramInfo"></detail-param-info>
            <detail-comment-info :comment-info="commentInfo"/>
        </scroll>

    </div>
</template>

<script>
    import Scroll from "../../components/common/scroll/Scroll";
    import DetailNavBar from "./childCpn/DetailNavBar";
    import DetailSwiper from "./childCpn/DetailSwiper";
    import DetailBaseInfo from "./childCpn/DetailBaseInfo";
    import DetailShopInfo from "./childCpn/DetailShopInfo";
    import DetailGoodsInfo from "./childCpn/DetailGoodsInfo";
    import DetailParamInfo from "./childCpn/DetailParamInfo";
    import DetailCommentInfo from "./childCpn/DetailCommentInfo";

    import {getDetail,  Goods, Shop, GoodsParam} from "network/detail";

    export default {
        name: "Detail",
        components: {
            DetailNavBar,
            DetailSwiper,
            DetailBaseInfo,
            DetailShopInfo,
            DetailGoodsInfo,
            DetailParamInfo,
            DetailCommentInfo,
            Scroll
        },
        data() {
            return {
                iid: undefined,
                topImages: [],
                goods: {},
                shop: {},
                detailInfo: {},
                paramInfo: {},
                commentInfo: {},
            }
        },
        created() {
            this.iid = this.$route.params.iid

            getDetail(this.iid).then(res => {
                console.log(res)
                this.topImages = res.result.itemInfo.topImages
                this.goods = new Goods(res.result.itemInfo, res.result.columns, res.result.shopInfo.services);
                this.shop = new Shop(res.result.shopInfo);
                this.detailInfo = res.result.detailInfo
                this.paramInfo = new GoodsParam(res.result.itemParams.info, res.result.itemParams.rule);
                if (res.result.rate.list) {
                    this.commentInfo = res.result.rate.list[0];
                }
            })
        }
    }
</script>

<style scoped>
    #detail{
        position: relative;
        height: 100vh;
        z-index: 9;
        background-color: white;
    }

    .detailnavbar{
        position: relative;
        z-index: 9;
        background: rgba(255,255,255,0.95)
    }

    .content{
        position: absolute;
        background-color: #fff;
        top: 44px;
        bottom: 49px;
        left: 0;
        right: 0;

    }
</style>