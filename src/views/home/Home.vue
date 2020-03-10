<template>
  <div id="home">
    <nav-bar class="home-nav"><div slot="center">购物街</div></nav-bar>
    <home-swiper :banners="banners"></home-swiper>
    <recommend-view :recommends="recommends"></recommend-view>
    <feature-view></feature-view>
    <tab-control :titles="['流行','新款','精选']" class="tab-control" @tabClick="tabClick"></tab-control>
    <goods-list :goods="showGoods"></goods-list>
    <ul>
      <li>列表：1</li>
      <li>列表：2</li>
      <li>列表：3</li>
      <li>列表：4</li>
      <li>列表：5</li>
      <li>列表：6</li>
      <li>列表：7</li>
      <li>列表：8</li>
      <li>列表：9</li>
      <li>列表：10</li>
      <li>列表：11</li>
      <li>列表：12</li>
      <li>列表：13</li>
      <li>列表：14</li>
      <li>列表：15</li>
      <li>列表：16</li>
      <li>列表：17</li>
      <li>列表：18</li>
      <li>列表：19</li>
      <li>列表：20</li>
      <li>列表：21</li>
      <li>列表：22</li>
      <li>列表：23</li>
      <li>列表：24</li>
      <li>列表：25</li>
      <li>列表：26</li>
      <li>列表：27</li>
      <li>列表：28</li>
      <li>列表：29</li>
      <li>列表：30</li>
      <li>列表：31</li>
      <li>列表：32</li>
      <li>列表：33</li>
      <li>列表：34</li>
      <li>列表：35</li>
      <li>列表：36</li>
      <li>列表：37</li>
      <li>列表：38</li>
      <li>列表：39</li>
      <li>列表：40</li>
      <li>列表：41</li>
      <li>列表：42</li>
      <li>列表：43</li>
      <li>列表：44</li>
      <li>列表：45</li>
      <li>列表：46</li>
      <li>列表：47</li>
      <li>列表：48</li>
      <li>列表：49</li>
      <li>列表：50</li>
      <li>列表：51</li>
      <li>列表：52</li>
      <li>列表：53</li>
      <li>列表：54</li>
      <li>列表：55</li>
      <li>列表：56</li>
      <li>列表：57</li>
      <li>列表：58</li>
      <li>列表：59</li>
      <li>列表：60</li>
      <li>列表：61</li>
      <li>列表：62</li>
      <li>列表：63</li>
      <li>列表：64</li>
      <li>列表：65</li>
      <li>列表：66</li>
      <li>列表：67</li>
      <li>列表：68</li>
      <li>列表：69</li>
      <li>列表：70</li>
      <li>列表：71</li>
      <li>列表：72</li>
      <li>列表：73</li>
      <li>列表：74</li>
      <li>列表：75</li>
      <li>列表：76</li>
      <li>列表：77</li>
      <li>列表：78</li>
      <li>列表：79</li>
      <li>列表：80</li>
      <li>列表：81</li>
      <li>列表：82</li>
      <li>列表：83</li>
      <li>列表：84</li>
      <li>列表：85</li>
      <li>列表：86</li>
      <li>列表：87</li>
      <li>列表：88</li>
      <li>列表：89</li>
      <li>列表：90</li>
      <li>列表：91</li>
      <li>列表：92</li>
      <li>列表：93</li>
      <li>列表：94</li>
      <li>列表：95</li>
      <li>列表：96</li>
      <li>列表：97</li>
      <li>列表：98</li>
      <li>列表：99</li>
      <li>列表：100</li>
    </ul>
  </div>
</template>

<script>
import HomeSwiper from './childComps/HomeSwiper'
import RecommendView from './childComps/RecommendView'
import FeatureView from './childComps/FeatureView'

import NavBar from 'components/common/navbar/NavBar'
import TabControl from 'components/content/tabControl/TabControl'
import GoodsList from 'components/content/goods/GoodsList'

import {getHomeMultidata,getHomeGoods} from 'network/home'
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
      currentType:'pop'
    }
  },
  created(){
    this.getHomeMultidata(),
    this.getHomeGoods('pop'),
    this.getHomeGoods('new'),
    this.getHomeGoods('sell')
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
    GoodsList
  }
}
</script>

<style>
  .home-nav{
    color:white;
    background-color: var(--color-tint);
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 10;
  }

  #home{
    padding-top: 44px;
  }

  .tab-control{
    position: sticky;
    top: 44px;
    z-index: 10;
  }
</style>