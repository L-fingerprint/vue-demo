<template>
     <div class="scroll">
          <ul class="tabs" ref="tabs" id="tabs">
              <li v-for="(n,i) in dataLists" class="list" :class="{isActive: currentIndex==i}" @click="Change(i)">
                  <router-link :to="n.to" tag="div">
                        <span> {{n.title}} </span>
                  </router-link>
              </li>
          </ul>
     </div>
</template>

<script>
    export default {
        name: "scroll",
        data (){
            return {
                  dataLists : [
                      {
                          to:'/',
                          title:'首页'
                      },
                      {
                          to:'/three',
                          title:'第二页面'
                      },
                      {
                          to:'/four',
                          title:'第三页面'
                      },
                      {
                          to:'/five',
                          title:'第四页面'
                      },
                      {
                          to:'/six',
                          title:'第五页面'
                      }


                  ],
                  currentIndex:0
            }
        },
        created (){
            this.currentIndex = this.$store.state.current;

        },
        methods:{
            Change (i){
                this.currentIndex = i;
                this.$store.commit('set_current',i);
            },
        //    记录滚动条的位置
            recordScroll(){
                   let dom = this.$refs.tabs;
                   // console.log(dom.scrollLeft);
                   dom.scrollLeft += this.current * 80
            }
        },
        mounted(){
            this.$nextTick(()=>{
                this.recordScroll();
            })
        },
        computed: {
            current() {
                return this.$store.state.current;
            }
        }

    }
</script>

<style scoped>
    .list {
        padding:0 20px  ;
        font-size: 16px;
        color: #282828;
        line-height: 50px;
    }
    .tabs {
        height: 50px;
        background: #fff;

    }
   /* .scroll {
        overflow-x: scroll;
        display: -webkit-box;

    }*/
    .scroll::-webkit-scrollbar{
        display: none;
    }
    .isActive {
        color:coral;
        font-size: 20px;

    }
    .tabs{
        display: -webkit-box;
        overflow-x: scroll;
        overflow-y:hidden ;
        -webkit-overflow-scrolling: touch;
    }


    .tabs::-webkit-scrollbar {
        display: none;
    }

</style>
