<template>
    <div id="detail">
        <detail-nav-bar
                @itemClick="titleClick"
                class="detailnavbar"
                ref="nav"/>
        <scroll class="content"
                ref="scroll"
                @scroll="contentScroll"
                :data="[topImages, goods, shop, detailInfo, paramInfo, commentInfo, recommendList]"
                :probe-type="3">
            <detail-swiper :top-images="topImages"></detail-swiper>
            <detail-base-info :goods="goods"></detail-base-info>
            <detail-shop-info :shop="shop"></detail-shop-info>
            <detail-goods-info  :detail-info="detailInfo"></detail-goods-info>
            <detail-param-info ref="params" :param-info="paramInfo"></detail-param-info>
            <detail-comment-info ref="comment" :comment-info="commentInfo"/>
            <detail-recommend-info ref="recommend" :recommend-list="recommendList"></detail-recommend-info>
        </scroll>
        <back-top @click.native="backTop" v-show="showBackTop"></back-top>
        <detail-bottom-bar @addToCart="addToCart"></detail-bottom-bar>
        <toast :show="showToast"></toast>
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
    import DetailRecommendInfo from "./childCpn/DetailRecommendInfo";
    import DetailBottomBar from "./childCpn/DetailBottomBar";
    import BackTop from "../../components/content/backTop/BackTop";
    import Toast from "../../components/common/toast/Toast";

    import {getDetail, Goods, Shop, GoodsParam, getRecommend} from "network/detail";
    import {debounce} from "../../common/utils";
    import {BACKTOP_DISTANCE} from "../../common/const";

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
            DetailRecommendInfo,
            DetailBottomBar,
            BackTop,
            Toast,
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
                recommendList: [],
                themeTopY: [],
                debounceThemeTop: null,
                showBackTop: false,
                showToast: false,
                currentIndex: 0
            }
        },
        updated() {
            this.loadImg()
        },
        methods: {
            titleClick(index) {
                this.$refs.scroll.scroll.scrollTo(0, -this.themeTopY[index], 500)
            },

            loadImg() {
                this.debounceThemeTop()
            },

            backTop (){
                this.$refs.scroll.scroll.scrollTo(0,0,500)
            },

            addToCart () {
                const obj = {}
                obj.iid = this.iid;
                obj.imgURL = this.topImages[0]
                obj.title = this.goods.title
                obj.desc = this.goods.desc;
                obj.newPrice = this.goods.nowPrice;
                this.$store.commit('addCart', obj)
                this.showToast = !this.showToast
                setTimeout(() =>{
                    this.showToast = !this.showToast
                },2000)
            },

            contentScroll(position) {
                this.showBackTop = position.y < -BACKTOP_DISTANCE

                this._listenScrollTheme(-position.y)
            },

            _listenScrollTheme(position) {
                let length = this.themeTopY.length;
                for (let i = 0; i < length; i++) {
                    let iPos = this.themeTopY[i];
                    if (position >= iPos && position < this.themeTopY[i + 1])
                    {
                        if (this.currentIndex !== i) {
                            this.currentIndex = i;
                        }
                        this.$refs.nav.currentIndexx = this.currentIndex
                        break;
                    }

                }
            }
        },
            created() {
                this.debounceThemeTop = debounce(() => {
                    this.themeTopY = []
                    this.themeTopY.push(0)
                    this.themeTopY.push(this.$refs.params.$el.offsetTop)
                    this.themeTopY.push(this.$refs.comment.$el.offsetTop)
                    this.themeTopY.push(this.$refs.recommend.$el.offsetTop)
                    this.themeTopY.push(Number.MAX_VALUE)
                }, 1000)

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
                getRecommend().then((res, error) => {
                    if (error) return
                    this.recommendList = res.data.list
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