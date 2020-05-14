<template>
        <div ref="wrapper">
            <div class="content">
                <slot></slot>
            </div>
        </div>
</template>

<script>
    import BScroll from 'better-scroll'
    export default {
        name: "Scroll",
        props: {
            probeType: {
                type: Number,
                default: 1
            },
            data: {
                type: Array,
                default: () => {
                    return []
                }
            },
            pullUpLoad: {
                type: Boolean,
                default: false
            }
        },
        data () {
            return {
                scroll: null
            }
        },
        methods: {
            finishPullUp() {
                this.scroll && this.scroll.finishPullUp()
            },
            refresh() {
                this.scroll && this.scroll.refresh()
            },
        },
        mounted() {
            this.scroll = new BScroll(this.$refs.wrapper, {
                probeType: this.probeType,
                pullUpLoad: this.pullUpLoad,
                click: true,
            })

            this.scroll.scrollTo(0,0)

            this.scroll.on('scroll', position => {
                this.$emit('scroll', position)
            })

            this.scroll.on('pullingUp',() => {
                this.$emit('pullingUp')
            })
        },
        //监听到data改变就refresh
        watch: {
            data() {
                setTimeout(this.refresh, 20)
            }
        }
    }
</script>

<style scoped>

</style>