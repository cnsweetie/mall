

<template>
    <div id="home">
        <nav-bar class="home-nav"><div slot="center">Shopping</div></nav-bar>
        <scroll class="content"
                ref="scroll"
                @scroll="contentScroll"
                @pullingUp="lodeMore"
                :data="showGoodsList"
                :pull-up-load="true"
                :probe-type="3"
                >
            <home-swiper :banners="banners"></home-swiper>
            <home-recommend :recommends="recommends"></home-recommend>
            <home-trend></home-trend>
            <tab-control :titles="['流行','新款','精选']" @itemClick="tabClick"></tab-control>
            <goods-list :goods-list="showGoodsList"></goods-list>
        </scroll>
        <back-top @click.native="backTop" v-show="showBackTop"></back-top>
    </div>
</template>

<script>
    import NavBar from "../../components/common/navbar/NavBar";
    import HomeSwiper from "./childCpn/HomeSwiper";
    import HomeRecommend from "./childCpn/HomeRecommend";
    import HomeTrend from "./childCpn/HomeTrend";
    import TabControl from "../../components/content/tabControl/TabControl";
    import GoodsList from "../../components/content/goods/GoodsList";
    import Scroll from "../../components/common/scroll/Scroll";
    import BackTop from "../../components/content/backTop/BackTop";
    import {NEW, POP, SELL,BACKTOP_DISTANCE } from "../../common/const";
    import {getHomeMultidata,getHomeData,} from "../../network/home";


    export default {
        name: "Home",
        components: {
            NavBar,
            HomeSwiper,
            HomeRecommend,
            HomeTrend,
            TabControl,
            GoodsList,
            Scroll,
            BackTop
        },

        data() {
            return {
                banners: [],
                recommends: [],
                goods: {
                    'pop': {page: 1, list: []},
                    'new': {page: 1, list: []},
                    'sell': {page: 1, list: []}
                },
                currentType: POP,
                tabOffsetTop: 0,
                showBackTop: false
            }
        },

        created() {
            this.getHomeMultidata()
            this.getHomeProducts(POP)
            this.getHomeProducts(NEW)
            this.getHomeProducts(SELL)
        },
        computed: {
            showGoodsList() {
                return this.goods[this.currentType].list
            }
        },
        methods :{
            tabClick(index) {
                switch (index) {
                    case 0:
                        this.currentType = POP
                        break
                    case 1:
                        this.currentType = NEW
                        break
                    case 2:
                        this.currentType = SELL
                        break
                }
            },
            contentScroll(position) {
                // 1.决定tabFixed是否显示
                this.isTabFixed = position.y < -this.tabOffsetTop

                // 2.决定backTop是否显示
                this.showBackTop = position.y < -BACKTOP_DISTANCE
            },
            backTop (){
                this.$refs.scroll.scroll.scrollTo(0,0,500)
            },
            lodeMore() {
                this.getHomeProducts(this.currentType)
            },

            getHomeMultidata() {
                getHomeMultidata().then(res => {
                    this.banners = res.data.banner.list
                    this.recommends = res.data.recommend.list
                })
            },
            getHomeProducts(type) {
                getHomeData(type, this.goods[type].page).then(res => {
                    const goodsList = res.data.list;
                    this.goods[type].list.push(...goodsList)
                    this.goods[type].page += 1
                    this.$refs.scroll.finishPullUp()
                })
            }
        }
    }
</script>

<style scoped>
    #home{
        height: 100vh;
    }
    .home-nav {
        background-color: lightpink;
        color: white;
        position: fixed;
        left: 0;
        right: 0;
        top: 0px;
        z-index: 1;
    }
    .content{
        position: absolute;
        top: 44px;
        bottom: 49px;
        left: 0;
        right: 0;

    }

</style>