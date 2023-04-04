<template>
  <div id="app">
    <AppHeader />
    
    <div class="container mt-3" style="min-height: 600px;">
      <router-view />
    </div>
  </div>
  <AppFooter />
</template>

<script>
import AppHeader from "@/components/AppHeader.vue";
import AppFooter from "@/components/AppFooter.vue";

export default {
  components: {
    AppHeader,
    AppFooter
  },
};
</script>

<style>
.page {
  max-width: 400px;
  margin: auto;
}
</style>