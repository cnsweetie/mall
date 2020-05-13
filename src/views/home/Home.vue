<template>
    <div id="home">
        <nav-bar class="home-nav"><div slot="center">Shopping</div></nav-bar>
        <home-swiper :banners="banners"></home-swiper>
        <home-recommend :recommends="recommends"></home-recommend>
        <home-trend></home-trend>
        <tab-control :title="title"></tab-control>
        <li>1</li>
        <li>1</li>
        <li>1</li>
        <li>1</li>
        <li>1</li>
        <li>1</li>
    </div>
</template>

<script>
    import NavBar from "../../components/common/navbar/NavBar";
    import HomeSwiper from "./childCpn/HomeSwiper";
    import HomeRecommend from "./childCpn/HomeRecommend";
    import HomeTrend from "./childCpn/HomeTrend";
    import TabControl from "../../components/content/tabControl/TabControl";
    import {getHomeMultidata} from "../../network/home";


    export default {
        name: "Home",
        components: {
            NavBar,
            HomeSwiper,
            HomeRecommend,
            HomeTrend,
            TabControl
        },
        data (){
            return{
                banners: [],
                recommends: [],
                title: ['精选','热门','新款']
            }
        },
        created() {
            // 请求数据
            getHomeMultidata().then(res => {
                console.log(res)
                this.banners = res.data.banner.list
                this.recommends = res.data.recommend.list
            })
        },
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
        top: 0;
        z-index: 1;
    }
</style>