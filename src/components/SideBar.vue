<template>
  <el-row>
    <el-col :span="4" :xs="24">
      <el-menu
        default-active="1"
        class="el-menu-vertical"
        background-color="#d0cfff"
        text-color="#000000"
        active-text-color="#ffd04b">
        <router-link
          v-for="link in links"
          :key="link.url"
          tag="el-menu-item"
          active-class="active"
          :to="link.url">
          {{ link.title }}
          <i class="el-icon-right icon-lg"></i>
          <i class="el-icon-bottom icon-xs"></i>
        </router-link>
      </el-menu>
   </el-col>
    <el-col :span="20" :xs="24">
      <router-view />
    </el-col>
  </el-row>
</template>

<script>
export default {
  data: () => ({
    links: [
      { title: "My contacts", url: "/" },
      { title: "Add new", url: "/add" },
    ],
    input: "",
  }),
};
</script>
