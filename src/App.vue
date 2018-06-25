<template>
  <div id="app">
    <!-- <router-view/> -->
    <m-header></m-header>
    <m-nav></m-nav>
    <keep-alive>
      <router-view></router-view>
    </keep-alive>
  </div>
</template>

<script>
import MHeader from "@/components/header";
import MNav from "@/components/nav";

export default {
  name: "App",
  components: { MHeader, MNav }
};
</script>

<style>
#app {
  width: 100%;
  height: 100%;
  margin: 0;
  background-color: rgba(36, 17, 17, 0.897);
}
</style>
