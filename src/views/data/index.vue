<template>
  <el-container style="height: 700px; border: 1px solid #eee">
    <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
      <h1>已存页面</h1>
      <div
        v-for="(item, index) in result"
        :key="item.pageId"
        class="imgwrapper"
        @click="click(index)"
      >
        <img :src="getsrc(item)" alt="" width="180px" />
      </div>
    </el-aside>
    <el-container>
      <el-main>
        <el-timeline>
          <el-timeline-item
            placement="top"
            v-for="item in result[acitve].interactions"
            :key="item.episode_id"
            :timestamp="'第' + (item.episode_id + 1) + '次探索'"
          >
            <el-card>
              <div v-for="step in item.steps" :key="step.stepid">
                <div class="wrapper">
                  <div class="stepimgwrapper">
                    <img
                      class="stepimg"
                      :src="getsrc(result[acitve])"
                      alt=""
                      width="200px"
                    />
                    <div
                      class="ele"
                      :style="{
                        left: getposi(step.bounds)[0] + 'px',
                        top: getposi(step.bounds)[1] + 'px',
                        width: getposi(step.bounds)[2] + 'px',
                        height: getposi(step.bounds)[3] + 'px',
                      }"
                    ></div>
                  </div>
                  <div
                    :style="{ width: '200px', float: 'left' }"
                    class="stepimgwrapper"
                  >
                    <div class="blue">Thought</div>
                    <div :style="{ marginBottom: '20px' }">
                      {{ step.act_desc.Thought }}
                    </div>

                    <div class="blue">Summary</div>
                    <div>{{ step.act_desc.Summary }}</div>
                  </div>
                </div>
              </div>

              <div class="stepimgwrapper">
                <img
                  v-if="item.steps[item.steps.length - 1].targetPageId"
                  class="stepimg"
                  :src="
                    getsrc(
                      result[item.steps[item.steps.length - 1].targetPageId]
                    )
                  "
                  alt=""
                  width="200px"
                />
                <div v-else>error</div>
              </div>
            </el-card>
          </el-timeline-item>
        </el-timeline>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
import * as cover from "../../assets/pagedoc.json";
export default {
  data() {
    return {
      result: [],
      acitve: 0,
      activeNames: [0],
    };
  },
  methods: {
    getposi(bounds) {
      if (bounds) {
        const [x1, y1] = bounds.slice(1, -1).split("][")[0].split(",");
        const [x2, y2] = bounds.slice(1, -1).split("][")[1].split(",");
        return [x1 / 6, y1 / 6, (x2 - x1) / 6, (y2 - y1) / 6];
      }
    },
    handleChange(val) {
      console.log(val);
    },
    click(index) {
      this.acitve = index;
    },
    getsrc(path) {
      const file = path.srcfile.imgpath.replace(
        "/Users/linbao/zju/uitest/proj/",
        ""
      );
      return require("../../assets/" + file);
    },
    getData() {
      this.result = [];
      cover.default.Pages.forEach((item) => {
        this.result.push(item);
      });
    },
  },
  activated() {
    this.getData();
  },
};
</script>

<style>
.stepimgwrapper {
  position: relative;
  /* background-color: #ccc; */
  float: left;
  margin: 10px;
}
.imgwrapper {
  padding: 10px;
}
.ele {
  position: absolute;
  border: 1px solid red;
}
.blue {
  color: blue;
}
.wrapper{
  overflow: hidden;
  /* width: 400px; */
  float: left;
}
.el-card.is-always-shadow, .el-card.is-hover-shadow:focus, .el-card.is-hover-shadow:hover{
  overflow: scroll;
}
</style>