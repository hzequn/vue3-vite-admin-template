<template>
  <el-breadcrumb separator="/" class="app-breadcrumb">
    <transition-group name="breadcrumb">
      <el-breadcrumb-item v-for="(item, index) in levelList" :key="item.path">
        <span v-if="item.redirect === 'noRedirect' || index == levelList.length - 1" class="no-redirect">
          {{ item.meta.title }}
        </span>
        <router-link v-else :to="item.redirect || item.path">{{ item.meta.title }}</router-link>
      </el-breadcrumb-item>
    </transition-group>
  </el-breadcrumb>
</template>

<script>
import { defineComponent } from 'vue'
export default defineComponent({
  data() {
    return {
      levelList: []
    }
  },
  watch: {
    $route() {
      this.getBreadcrumb()
    }
  },
  created() {
    this.getBreadcrumb()
  },
  methods: {
    getBreadcrumb() {
      // only show routes with meta.title
      let matched = this.$route.matched.filter((item) => item.meta && item.meta.title)
      const first = matched[0]

      if (!this.isDashboard(first)) {
        matched = [{ path: '/home', meta: { title: '首页' }}].concat(matched)
      }

      this.levelList = matched.filter((item) => item.meta && item.meta.title && item.meta.breadcrumb !== false)
    },
    isDashboard(meta) {
      const name = meta && meta.name
      if (!name) {
        return false
      }
      return name.trim().toLocaleLowerCase() === 'Home'.toLocaleLowerCase()
    }
  }
})
</script>

<style lang="scss" scoped>
.app-breadcrumb.el-breadcrumb {
  display: inline-block;
  margin-left: 8px;
  font-size: 14px;
  line-height: 50px;

  .no-redirect {
    color: #97a8be;
    cursor: text;
  }
}
</style>
