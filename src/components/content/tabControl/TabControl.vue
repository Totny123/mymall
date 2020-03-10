<template>
  <div class="tab-control">
    <div class="tab-control-item" v-for="(item,index) in titles" @click="itemClick(index)">
      <span :class="{active:index===currentIndex}">{{item}}</span>
    </div>
  </div>
</template>

<script>
export default {
  name: "TabControl",
  data(){
    return {
      currentIndex:0
    }
  },
  methods:{
    itemClick(index){
      this.currentIndex=index
      this.$emit('tabClick',index)

    }
  },
  props:{
    titles:{
      type:Array,
      default(){
        return []
      }
    }
  }
}
</script>

<style>
  .tab-control{
    display: flex;
    text-align: center;
    font-size: 15px;
    height: 40px;
    line-height: 40px;
    background-color: white;
  }

  .tab-control-item{
    flex: 1;
  }

  .tab-control-item span{
    padding: 5px;
  }

  .active{
    color: var(--color-high-text);
    border-bottom: 3px solid var(--color-high-text);
  }
</style>