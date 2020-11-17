<template>
  <div id="app">
    <!-- render layout component  -->
    <component :is="layout">
    <!-- render content by router -->
      <router-view />
    </component>
  </div>
</template>

<script>
import MainLayout from "@/layouts/MainLayout";
import EmptyLayout from "@/layouts/EmptyLayout";
export default {
  computed: {
    // get layout value (by "meta" in router from "@router/index.js")
    layout() {
      return (this.$route.meta.layout ) +'-layout';
    },
  },
  components: {
    MainLayout,
    EmptyLayout,
  },
};
</script>
