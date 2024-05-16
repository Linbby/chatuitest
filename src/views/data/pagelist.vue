<template>
  <el-card class="box-card">
    <div class="demo-image">
      <div class="block" v-for="(item, index) in pagedata" :key="index">
        <div class="demonstration">{{ item.pageId }}</div>
        <el-image
          style="width: 180px; height: 300px"
          :src="getsrc(item)"
          :fit="'fill'"
          @click="click(item)"
        ></el-image>
      </div>
    </div>
  </el-card>
</template>

<script>
import * as cover from "../../assets/pagedoc.json";
export default {
  data() {
    return {
      pagedata: [],
    };
  },
  methods: {
    click(item) {
      this.$router.push({
        path: "/data/pageinfo",
        query: {
          pageId: item.pageId,
        },
      });
    },
    getsrc(path) {
      const file = path.srcfile.imgpath.replace(
        "/Users/linbao/zju/uitest/proj/",
        ""
      );
     console.log("../../assets/" + file);

      return require("../../assets/" + file);
    },
    getData() {
      this.pagedata = [];
      cover.default.Pages.forEach((item) => {
        this.pagedata.push(item);
      });
    },
  },
  activated() {
    this.getData();
  },
};
</script>

<style scoped>
.box-card {
  margin: 20px;
}
.demonstration {
  width: 100%;
  text-align: center;
}
.block {
  float: left;
  padding: 20px;
}
.fill {
  object-fit: fill;
}
</style>