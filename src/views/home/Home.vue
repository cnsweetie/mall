

<template>
    <div id="home">
        <nav-bar class="home-nav"><div slot="center">Shopping</div></nav-bar>
        <home-swiper :banners="banners"></home-swiper>
        <home-recommend :recommends="recommends"></home-recommend>
        <home-trend></home-trend>
        <tab-control :titles="['流行','新款','精选']" @itemClick="tabClick"></tab-control>
        <goods-list :goods-list="showGoodsList"></goods-list>
    </div>
</template>

<script>
    import NavBar from "../../components/common/navbar/NavBar";
    import HomeSwiper from "./childCpn/HomeSwiper";
    import HomeRecommend from "./childCpn/HomeRecommend";
    import HomeTrend from "./childCpn/HomeTrend";
    import TabControl from "../../components/content/tabControl/TabControl";
    import GoodsList from "../../components/content/goods/GoodsList";
    import {NEW, POP, SELL } from "../../common/const";
    import {getHomeMultidata,getHomeData} from "../../network/home";


    export default {
        name: "Home",
        components: {
            NavBar,
            HomeSwiper,
            HomeRecommend,
            HomeTrend,
            TabControl,
            GoodsList
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

                    //this.$refs.scroll.finishPullUp()
                })
            }
        }
    }
</script>

<style scoped>
    #home{
        padding-top: 44px;
    }
    .home-nav {
        background-color: lightpink;
        color: white;
        position: fixed;
        left: 0;
        right: 0;
        top: 0px;
        z-index: 5;
    }
</style>