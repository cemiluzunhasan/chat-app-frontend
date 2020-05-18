<template>
  <div class="Home">
    <sider />
    <layout />
  </div>
</template>
<script>

import Sider from '@/components/Sider.vue';
import Layout from '@/components/Layout.vue';

export default {
  name: 'Home',
  components: { 
    Sider,
    Layout
  }
}
</script>
<style lang="scss">
.Home {
  display: flex;
  font-family: 'Roboto', sans-serif;
}
</style>