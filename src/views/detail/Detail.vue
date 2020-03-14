<template>
  <div id="detail">
    <detail-nav-bar />
  </div>
</template>

<script>
import DetailNavBar from './childComps/DetailNavBar'
export default {
  name: "Detail",
  data(){
    return {
      id:null
    }
  },
  created(){
    this.id = this.$route.params.iid
  },
  components:{
    DetailNavBar
  }
}
</script>

<style scoped>

</style>