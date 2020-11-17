<template>
  <div>
    <NavBar />

    <SideBar />
  </div>
</template>



<script>
// Import components
import NavBar from "@/components/NavBar";
import SideBar from "@/components/SideBar";

export default {
  components: {
    SideBar,
    NavBar,
  },
};
</script>
