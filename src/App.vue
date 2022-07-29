<template>
  <HeaderItem/>
  <div class="container mx-auto ">
    <router-view/>
  </div>
  
</template>

<script >
import HeaderItem from './components/HeaderItem.vue';

export default{
  components:{
    HeaderItem,
  }
}
</script>
