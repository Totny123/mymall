<template>
  <div id="detail">
    <detail-nav-bar class="detail-nav-bar" />
    <scroll class="scroll" ref="scroll">
      <detail-swiper :topImgs="topImgs" />
      <detail-base-info :goods="goods" />
      <detail-shop-info :shop="shop" />
      <detail-goods-info :detailInfo="detailInfo" @imgLoad="imgLoad" />
      <detail-param-info :paramInfo="paramInfo" />
    </scroll>
  </div>
</template>

<script>
import DetailNavBar from './childComps/DetailNavBar'
import DetailSwiper from './childComps/DetailSwiper'
import DetailBaseInfo from './childComps/DetailBaseInfo'
import DetailShopInfo from './childComps/DetailShopInfo'
import DetailGoodsInfo from './childComps/DetailGoodsInfo'
import DetailParamInfo from './childComps/DetailParamInfo'

import Scroll from 'components/common/scroll/Scroll'

import {getDetail,Goods,Shop,GoodsParam} from 'network/detail'
import {debounce} from 'common/utils'

export default {
  name: "Detail",
  data(){
    return {
      id:null,
      topImgs:null,
      goods:{},
      shop:{},
      detailInfo:{},
      paramInfo:{},
      refresh:null
    }
  },
  created(){
    this.id = this.$route.params.iid
    getDetail(this.id).then(res=>{
      const data=res.result
      console.log(data)
      this.topImgs = data.itemInfo.topImages
      this.goods = new Goods(data.itemInfo,data.columns,data.shopInfo.services)
      this.shop = new Shop(data.shopInfo)
      this.detailInfo = data.detailInfo
      this.paramInfo = new GoodsParam(data.itemParams.info,data.itemParams.rule)
      // console.log(Object.keys(this.goods))
      // console.log(typeof(this.goods))
    })
  },
  mounted(){
    const r=debounce(this.$refs.scroll.refresh,500)
    this.refresh = r
  },
  components:{
    DetailNavBar,
    DetailSwiper,
    DetailBaseInfo,
    DetailShopInfo,
    Scroll,
    DetailGoodsInfo,
    DetailParamInfo
  },
  methods:{
    imgLoad(){
      this.refresh()
    }
  }
}
</script>

<style scoped>
  #detail{
    position: relative;
    height: 100vh;
    background-color: #fff;
    z-index: 10;
  }

  .detail-nav-bar{
    position: relative;
    z-index: 10;
    background-color: #fff;
  }

  .scroll{
    /* position:absolute;
    left: 0;
    right: 0;
    top: 44px;
    bottom: 0; */
    height: calc(100% - 44px);
  }
</style>