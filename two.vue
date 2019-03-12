<template>
    <div class="two">
        <scroll></scroll>
        <div class="activity">
            <div class="activity_content" ref="lists" >
                <div class="box" v-for="(n,i) in lists "> {{n.title}}</div>
                <div style="text-align: center; width: 100%;height: 50px;margin: auto;background:#fff;line-height: 50px;" v-if="isShow">
                    <img src="../assets/image/loading.gif" alt="" width="100"></div>
                <div  v-else style="text-align: center; width: 100%;height: 50px;">-- 到底了哦 --</div>
            </div>

        </div>
    </div>
</template>

<script>
    import Scroll from "../components/scroll";

    export default {
        name: "two",
        components: {Scroll},
        data() {
            return {
                lists: [],
                page:1,
                isTrue:false,
                isShow:false,

            }
        },
        created() {
            this.$store.commit('set_current', 0);
            this.getInfo();
        },
        methods: {
            getInfo() {
                this.$http({
                    method: "post",
                    url: "http://www.chuanloo.com/home/information/informationList?size=4",
                    dataType: "json",
                    data: {
                        cid: 1,
                        p:this.page
                    },
                })
                    .then(res => {
                        if (res.data.status == 1) {
                            /*if(res.data.result===undefined ||res.data.result ==null||res.data.result==''){
                                this.isTrue=false;
                                this.isShow = false;
                            }else {*/
                                this.lists=this.lists.concat(res.data.result.param);
                                if(res.data.result.param.length <4){
                                    this.isTrue =false;
                                    this.isShow = false;
                                }else {
                                    this.isTrue =true;
                                    this.isShow = true;
                                }

                            // }
                        }
                    })
                    .catch(err => {
                        console.log(err);
                    })
            },
          /*  getHeight() {
                this.$http({
                    method: "post",
                    url: "http://192.168.0.98/changlu/home/information/informationList?size=4",
                    dataType: "json",
                    data: {
                        cid: 1,
                        p:this.page
                    },
                })
                    .then(res => {
                        if (res.data.status == 1) {
                           this.lists=this.lists.concat(res.data.result.param);
                        }
                    })
                    .catch(err => {
                        console.log(err);
                    })

            },*/
            handleScroll(){
                    var that = this;
                    let s_top = document.documentElement.scrollTop;
                    let d_height = document.body.clientHeight-document.documentElement.clientHeight;
                    if(that.isTrue){
                        if(s_top >= d_height){
                            this.isShow = true;
                            setTimeout(()=>{
                                that.page++;
                                that.getInfo();
                            },1000);
                            that.isTrue=false;
                        }
                    }


                //判断滚动到底部
                /*if ($(window).scrollTop() >= $(document).height() - $(window).height()) {
                    if (that.isTrue) {
                        setTimeout(function() {
                        }, 1000);
                        that.isTrue = false;
                    }
                }*/

            }


        },
        mounted() {
              window.addEventListener('scroll',this.handleScroll)


        },
        computed: {
            current() {
                return this.$store.state.current;
            }
        }
    }
</script>

<style scoped>
    .two {
        background: #3ac8ff;
        /*height: 700px;*/
        padding: 0 0 1px;
    }

    .paging_numbers ul {
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .paging_numbers ul li {
        margin: 0 10px 10px 0;
        padding: 4px 10px;
        border-radius: 5px;
        border: 1px solid #ccc;
        background: #fff;
    }

    .paging_numbers ul li a {
        display: block;
        width: 100%;
    }

    .paging_numbers ul li.active {
        background: #7e272f;

    }

    .paging_numbers ul li.active a {
        color: #fff;
    }

    @media (min-width: 768px) and (orientation: portrait) {
        .paging_numbers ul li {
            margin: 0 10px 10px 0;
            padding: 6px 12px;
            border-radius: 5px;
            border: 1px solid #ccc;
            background: #fff;
        }
    }

    .box {
        width: 80%;
        margin: 0 auto 20px;
        border-radius: 10px;
        background: #f0f0f0;
        height: 200px;
        text-align: center;

    }
</style>
