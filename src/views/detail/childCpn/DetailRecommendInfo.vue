<template>
  <div class="recommend-info">
      <goods-list-item v-for="(item, index) in recommendList" :key="index" :goods="item"></goods-list-item>
  </div>
</template>

<script>
  import GoodsListItem from "../../../components/content/goods/GoodsListItem";

	export default {
		name: "DetailRecommendInfo",
        components: {
            GoodsListItem
        },
        props: {
            recommendList: {
            type: Array
      }
    }
	}
</script>

<style scoped>
  .recommend-info {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;
  }


</style>
