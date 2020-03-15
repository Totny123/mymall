<template>
  <div id="home">
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>
    <tab-control :titles="['流行','新款','精选']" class="tab-control" @tabClick="tabClick" ref="tabControl1" v-show="isShow"></tab-control>
    <scroll class="content" ref="scroll" :probe-type="3" @scroll="contentScroll" @pullUpLoad="pullUpLoad" :pull-up-load="true">
      <home-swiper :banners="banners" @swiperImgLoad='swiperImgLoad'></home-swiper>
      <recommend-view :recommends="recommends"></recommend-view>
      <feature-view></feature-view>
      <tab-control :titles="['流行','新款','精选']"  @tabClick="tabClick" ref="tabControl2"></tab-control>
      <goods-list :goods="showGoods"></goods-list>
    </scroll>
    <back-top @click.native="backClick" v-show="backTopIsShow"></back-top>
  </div>
</template>

<script>
import HomeSwiper from './childComps/HomeSwiper'
import RecommendView from './childComps/RecommendView'
import FeatureView from './childComps/FeatureView'

import NavBar from 'components/common/navbar/NavBar'
import TabControl from 'components/content/tabControl/TabControl'
import GoodsList from 'components/content/goods/GoodsList'
import Scroll from 'components/common/scroll/Scroll'
import BackTop from 'components/content/backTop/BackTop'

import {getHomeMultidata,getHomeGoods} from 'network/home'
import {debounce} from 'common/utils'

export default {
  name: "Home",
  data(){
    return {
      banners:[],
      recommends:[],
      goods:{
        'pop':{page:0,list:[]},
        'new':{page:0,list:[]},
        'sell':{page:0,list:[]}
      },
      currentType:'pop',
      backTopIsShow:false,
      tabOffsetTop:0,
      isShow:false,
      saveY:0
    }
  },
  created(){
    this.getHomeMultidata(),
    this.getHomeGoods('pop'),
    this.getHomeGoods('new'),
    this.getHomeGoods('sell')
  },
  mounted(){
    const refresh=debounce(this.$refs.scroll.refresh,500)
    this.$bus.$on('itemImgLoad',()=>{
      refresh()
    })
  },
  activated(){
    this.$refs.scroll.refresh()
    this.$refs.scroll.scrollTo(0,this.saveY,0)
    
  },
  deactivated(){
    this.saveY = this.$refs.scroll.getScrollY()
  },
  methods:{
    getHomeMultidata(){
      getHomeMultidata().then(res=>{
      this.banners=res.data.banner.list
      this.recommends=res.data.recommend.list
      // console.log(res)
      })
    },
    getHomeGoods(type){
      const page = this.goods[type].page + 1
      getHomeGoods(type,page).then(res=>{
        // console.log(res)
        this.goods[type].list.push(...res.data.list)
        this.goods[type].page += 1
        this.$refs.scroll.finishPullUp()
      })
    },
    tabClick(index){
      switch(index){
        case 0:this.currentType='pop'
              break
        case 1:this.currentType='new'
              break
        case 2:this.currentType='sell'
              break
      }
      this.$refs.tabControl1.currentIndex = index
      this.$refs.tabControl2.currentIndex = index
    },
    backClick(){
      this.$refs.scroll.scrollTo(0,0,500)
    },
    contentScroll(position){
      // console.log(position)
      this.backTopIsShow = (-position.y) > 1000
      this.isShow = (-position.y) > this.tabOffsetTop
    },
    pullUpLoad(){
      this.getHomeGoods(this.currentType)
      
    },
    swiperImgLoad(){
      this.tabOffsetTop = this.$refs.tabControl2.$el.offsetTop
      // console.log(this.tabOffsetTop)
    }
  },
  computed:{
    showGoods(){
      return this.goods[this.currentType].list
    }
  },
  components:{
    HomeSwiper,
    RecommendView,
    FeatureView,
    NavBar,
    TabControl,
    GoodsList,
    Scroll,
    BackTop
  }
}
</script>

<style scoped>
  .home-nav{
    color:white;
    background-color: var(--color-tint);
    position: relative;
    /* top: 0;
    left: 0;
    right: 0; */
    z-index: 10;
  }

  #home{
    /* padding-top: 44px; */
    position: relative;
    height: 100vh;
  }

  /* .tab-control{
    position: sticky;
    top: 44px;
    z-index: 10;
  } */

  .tab-control{
    position: relative;
    z-index: 10;
  }

  .content{
    position: absolute;
    top:44px;
    bottom: 49px;
    left: 0;
    right: 0;
  }

  /* .fixed{
    position: fixed;
    left: 0;
    right: 0;
    top: 44px;
  } */
</style>