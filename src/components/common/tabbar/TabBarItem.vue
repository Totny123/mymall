<template>
  <div class="tab-bar-item" @click="itemClick">
    <div v-if="!isActive"><slot name="item-icon"></slot></div>
    <div v-else><slot name="item-icon-active"></slot></div>
    <div :style="activeStyle"><slot name="item-text"></slot></div>
  </div>
</template>

<script>
export default {
  name:'TabBarItem',
  props:{
    path:String,
    activeColor:{
      type:String,
      default:'red'
    }
  },
  data(){
      return {
        // isActive:false
      }
  },
  methods:{
    itemClick(){
      this.$router.push(this.path);
    }
  },
  computed:{
    isActive(){
      // console.log(this.$route.path)
      return this.$route.path.indexOf(this.path)!==-1
    },
    activeStyle(){
      return this.isActive?{color:this.activeColor}:{}
    }
  }
}
</script>

<style>
.tab-bar-item{
  flex: 1;
  text-align: center;
  height:49px;
  box-shadow: 0 -1px 1px rgba(100, 100, 100, .08)
}

.tab-bar-item img{
  height:24px;
  width:24px;
  vertical-align:top;
  margin-top: 3px;
}
</style>