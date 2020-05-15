<template>
    <div class="profile">
        <a class="a" href="https://github.com/cnsweetie/mall">Click to Star My Github</a>
    </div>
</template>

<script>
    export default {
        name: "Profile"
    }
</script>

<style scoped>
    .profile{
        position: fixed;
        top: 50%;
        left: 50%;
        transform: translate(-50%,-50%);
    }
    .a{
        text-align: center;
        font-size: 15px;
    }
</style>