<template>
  <el-breadcrumb class="app-breadcrumb" separator="/">
    <transition-group name="breadcrumb">
      <el-breadcrumb-item v-for="(item, index) in levelList" :key="item.path">
        <span
          v-if="item.redirect === 'noRedirect' || index == levelList.length - 1"
          class="no-redirect"
          >{{ item.meta.title }}</span
        >
        <a v-else @click.prevent="handleLink(item)">{{ item.meta.title }}</a>
      </el-breadcrumb-item>
    </transition-group>
  </el-breadcrumb>
</template>

<script>
import pathToRegexp from "path-to-regexp";

export default {
  data() {
    return {
      levelList: null,
    };
  },
  watch: {
    $route() {
      this.getBreadcrumb();
    },
  },
  created() {
    this.getBreadcrumb();
  },
  methods: {
    getBreadcrumb() {
      // only show routes with meta.title
      let matched = this.$route.matched.filter(
        (item) => item.meta && item.meta.title
      );
      const first = matched[0];

      // if (!this.isDashboard(first)) {
      //   matched = [{ path: '/dashboard', meta: { title: 'Dashboard' }}].concat(matched)
      // }

      this.levelList = matched.filter(
        (item) => item.meta && item.meta.title && item.meta.breadcrumb !== false
      );
      console.log(this.$route.query);
      if (this.$route.path == "/data/pageinfo") {
        this.levelList = [
          {
            path: "/data/list",
            meta: { title: "列表", icon: "el-icon-data-analysis" },
          },
          {
            path: "/data/pageinfo",
            meta: {
              title: "详情 - " + this.$route.query.pageId,
              icon: "el-icon-data-analysis",
            },
            query: this.$route.query,
          },
        ];
      }
      if (this.$route.path == "/analysis/file") {
        this.levelList = [
          {
            path: "/analysis/index",
            meta: { title: "代码检测", icon: "el-icon-data-analysis" },
          },
          {
            path: "/analysis/hub",
            meta: {
              title: this.$route.query.uuid,
              icon: "el-icon-data-analysis",
            },
            query: this.$route.query,
          },
          {
            path: "/analysis/file",
            meta: {
              title: this.$route.query.sameArtifactID,
              icon: "el-icon-data-analysis",
            },
            query: this.$route.query,
          },
        ];
      }
      if (this.$route.path == "/analysis/hub") {
        this.levelList = [
          {
            path: "/analysis/index",
            meta: { title: "代码检测", icon: "el-icon-data-analysis" },
          },
          {
            path: "/analysis/hub",
            meta: {
              title: this.$route.query.uuid,
              icon: "el-icon-data-analysis",
            },
            query: this.$route.query,
          },
        ];
      }
      if (this.$route.path == "/analysis/cvefn") {
        this.levelList = [
          {
            path: "/analysis/index",
            meta: { title: "代码检测", icon: "el-icon-data-analysis" },
          },
          {
            path: "/analysis/cvefile",
            meta: {
              title: this.$route.query.uuid,
              icon: "el-icon-data-analysis",
            },
            query: this.$route.query,
          },
          {
            path: "/analysis/cvefn",
            meta: {
              title: this.$route.query.location,
              icon: "el-icon-data-analysis",
            },
            query: this.$route.query,
          },
        ];
      }
      if (this.$route.path == "/analysis/cvefile") {
        this.levelList = [
          {
            path: "/analysis/index",
            meta: { title: "代码检测", icon: "el-icon-data-analysis" },
          },
          {
            path: "/analysis/cvefile",
            meta: {
              title: this.$route.query.uuid,
              icon: "el-icon-data-analysis",
            },
          },
        ];
      }
      if (this.$route.path == "/data/cveinfo") {
        this.levelList = [
          {
            path: "/data/index",
            meta: { title: "数据管理" },
          },
          {
            path: "/data/cve",
            meta: { title: "漏洞管理" },
          },
          {
            path: "/data/cveinfo",
            meta: { title: "漏洞详情" },
            query: this.$route.query,
          },
        ];
      }
    },
    isDashboard(route) {
      const name = route && route.name;
      if (!name) {
        return false;
      }
      return (
        name.trim().toLocaleLowerCase() === "Dashboard".toLocaleLowerCase()
      );
    },
    pathCompile(path) {
      // To solve this problem https://github.com/PanJiaChen/vue-element-admin/issues/561
      const { params } = this.$route;
      var toPath = pathToRegexp.compile(path);
      return toPath(params);
    },
    handleLink(item) {
      const { redirect, path } = item;
      if (redirect) {
        this.$router.push(redirect);
        return;
      }
      this.$router.push(item);
    },
  },
};
</script>

<style lang="scss" scoped>
.app-breadcrumb.el-breadcrumb {
  display: inline-block;
  font-size: 14px;
  line-height: 50px;
  margin-left: 8px;

  .no-redirect {
    color: #97a8be;
    cursor: text;
  }
}
</style>
