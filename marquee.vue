<template>
    <div class="marquee">
        <div class="title">
            <div class="tit_content" ref="wrap">
                <!--<p>测试数据测试数据测试数据测试数据测</p>-->
                <p ref="titWidth" :class='animationClass' :style="contentStyle">{{content}}</p>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "marquee",
        data() {
            return {
                content: String,
                delay:'',
                speed:0.02,//为了过渡，已经运动速度相等，我们用px/s，即每px需要多少秒，暂定每px需要0.02s
                duration: 0,
                animationClass: '',
                contentStyle:{},
                first:true,

            }
        },
        created() {
            this.content = '测试数据测试数据测试数据测试数据测测试数据测测试数据测测试数据测试数据测测试数据测测试数据测';

        },
        methods: {},
        mounted() {
            this.$nextTick(() => {
                const {wrap, titWidth} = this.$refs;
                if (!wrap || !titWidth) {
                    return;
                }
                const wrapWidth = wrap.getBoundingClientRect().width;
              /*  const paddingLeft =((screen.width - wrapWidth) / 2 + wrapWidth);
                console.log(paddingLeft);*/
                const offsetWidth = titWidth.getBoundingClientRect().width;
                this.delay = offsetWidth * this.speed  *1000  ;
                if (offsetWidth>wrapWidth){
                    this.animationClass ='tit_content__play';
                    this.contentStyle = {
                        animationDelay:0 ,//延时
                        animationDuration:offsetWidth * this.speed +'s',
                    };
                }
                setTimeout(()=>{
                    this.animationClass ='tit_content--infinite ';
                    this.contentStyle = {
                        animationDelay:0 ,//延时
                        animationDuration: (wrapWidth+offsetWidth) *this.speed +'s'  ,//总时间
                        paddingLeft:wrapWidth +'px'
                    };
                }, this.delay)

            })

        }

    }
</script>

<style scoped lang="less">
    @width: 100%;
    .title {
        width: @width;
        height: 0.9rem;
        background: linear-gradient(0deg, #C2F4BE 0%, #ECFFDE 100%);
        padding: 0 .5rem;
        box-sizing: border-box;
        font-size: 0.3rem;
        .tit_content {
            overflow: hidden;
            height: 0.9rem;
            line-height: 0.9rem;
            position: relative;
            p {
                white-space: nowrap;
                word-wrap: normal;
                display: inline-block;
                position: absolute;

            }
            &__play {
                 animation-name: van-notice-bar-play ;
                animation-iteration-count:1;
                animation-fill-mode:both;
                animation-timing-function: linear;
            }
            &--infinite {
                animation-name: van-notice-bar-play-infinite ;
                animation-iteration-count:infinite;
                animation-fill-mode:both;
                animation-timing-function: linear;
            }
        }
    }
    @keyframes van-notice-bar-play {
        to { transform: translate3d(-100%, 0, 0); }
    }

    @keyframes van-notice-bar-play-infinite {
        to { transform: translate3d(-100%, 0, 0); }
    }

</style>
