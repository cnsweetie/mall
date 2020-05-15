<template>
    <div class="toast" v-show="show">
        <span>继续买吧</span>
    </div>
</template>

<script>
    export default {
        name: "Toast",
        props: {
            show: {
                type: Boolean,
                default: false
            }
        }
    }
</script>

<style scoped>
    .toast{
        position: fixed;
        top: 50%;
        left: 50%;
        transform: translate(-50%,-50%);
        padding: 8px 10px;
        color: white;
        background-color: rgba(0,0,0,0.66);
        border-radius: 10px;
    }
</style>