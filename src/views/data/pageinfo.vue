<template>
  <!-- <el-card class="box-card"> -->
  <div class="box-card">
    <el-tabs type="border-card" v-model="activeName" @tab-click="handleClick">
      <el-tab-pane label="探索交互" name="first">
        <el-timeline>
          <div v-for="(region, index) in page.regions" :key="index">
            <el-timeline-item
              placement="top"
              v-for="(item, index) in region.interactions"
              :key="index"
              :timestamp="'第' + (index + 1) + '次探索'"
            >
              <el-card>
                <div v-for="step in path[item].interSteps" :key="step.stepId">
                  <div class="wrapper" v-if="step.orgSrcfile !== -1">
                    <div class="stepimgwrapper">
                      <img
                        class="stepimg"
                        :src="getsrc(step.orgSrcfile)"
                        alt=""
                        width="200px"
                      />
                      <div
                        class="ele"
                        v-if="step.act_case.bounds"
                        :style="{
                          left: getposi(step.act_case.bounds)[0] + 'px',
                          top: getposi(step.act_case.bounds)[1] + 'px',
                          width: getposi(step.act_case.bounds)[2] + 'px',
                          height: getposi(step.act_case.bounds)[3] + 'px',
                        }"
                      ></div>
                      <div v-if="step.act_desc.Action" class="red">
                        {{ step.act_desc.Action }}
                      </div>
                    </div>
                  </div>
                </div>

                <div class="stepimgwrapper">
                  <img
                    v-if="
                      !path[item].interSteps[path[item].interSteps.length - 1]
                        .error
                    "
                    class="stepimg"
                    :src="
                      getsrc(
                        path[item].interSteps[path[item].interSteps.length - 1]
                          .targetSrcfile
                      )
                    "
                    alt=""
                    width="200px"
                  />
                  <div v-else>
                    <img
                      v-if="
                        path[item].interSteps[path[item].interSteps.length - 1]
                          .targetSrcfile.ret
                      "
                      class="stepimg"
                      :src="
                        getsrc(
                          path[item].interSteps[
                            path[item].interSteps.length - 1
                          ].targetSrcfile.ret
                        )
                      "
                      alt=""
                      width="200px"
                    />

                    <div
                      v-if="
                        path[item].interSteps[path[item].interSteps.length - 1]
                          .error
                      "
                      class="red"
                    >
                      {{
                        path[item].interSteps[path[item].interSteps.length - 1]
                          .error
                      }}
                    </div>
                  </div>
                </div>
              </el-card>
            </el-timeline-item>
          </div>
        </el-timeline>
      </el-tab-pane>
      <el-tab-pane label="导航路径" name="second">
        <el-timeline>
          <el-timeline-item
            placement="top"
            v-for="(item, index) in page.navSteps"
            :key="index"
            :timestamp="'第' + (index + 1) + '次导航'"
          >
            <el-card>
              <div v-for="step in path[item].baseSteps" :key="step.stepId">
                <div class="wrapper" v-if="step.orgSrcfile !== -1">
                  <div class="stepimgwrapper">
                    <img
                      class="stepimg"
                      :src="getsrc(step.orgSrcfile)"
                      alt=""
                      width="200px"
                    />
                    <div
                      class="ele"
                      v-if="step.act_case.bounds"
                      :style="{
                        left: getposi(step.act_case.bounds)[0] + 'px',
                        top: getposi(step.act_case.bounds)[1] + 'px',
                        width: getposi(step.act_case.bounds)[2] + 'px',
                        height: getposi(step.act_case.bounds)[3] + 'px',
                      }"
                    ></div>
                    <div v-if="step.act_desc.Action" class="red">
                      {{ step.act_desc.Action }}
                    </div>
                  </div>
                </div>
              </div>
              <div v-for="step in path[item].interSteps" :key="path[item].baseSteps.length + step.stepId">
                <div class="wrapper" v-if="step.orgSrcfile !== -1">
                  <div class="stepimgwrapper">
                    <img
                      class="stepimg"
                      :src="getsrc(step.orgSrcfile)"
                      alt=""
                      width="200px"
                    />
                    <div
                      class="ele"
                      v-if="step.act_case.bounds"
                      :style="{
                        left: getposi(step.act_case.bounds)[0] + 'px',
                        top: getposi(step.act_case.bounds)[1] + 'px',
                        width: getposi(step.act_case.bounds)[2] + 'px',
                        height: getposi(step.act_case.bounds)[3] + 'px',
                      }"
                    ></div>
                    <div v-if="step.act_desc.Action" class="red">
                      {{ step.act_desc.Action }}
                    </div>
                  </div>
                </div>
              </div>
              <div class="stepimgwrapper">
                  <img
                    v-if="
                      !path[item].interSteps[path[item].interSteps.length - 1]
                        .error
                    "
                    class="stepimg"
                    :src="
                      getsrc(
                        path[item].interSteps[path[item].interSteps.length - 1]
                          .targetSrcfile
                      )
                    "
                    alt=""
                    width="200px"
                  />
                  <div v-else>
                    <img
                      v-if="
                        path[item].interSteps[path[item].interSteps.length - 1]
                          .targetSrcfile.ret
                      "
                      class="stepimg"
                      :src="
                        getsrc(
                          path[item].interSteps[
                            path[item].interSteps.length - 1
                          ].targetSrcfile.ret
                        )
                      "
                      alt=""
                      width="200px"
                    />

                    <div
                      v-if="
                        path[item].interSteps[path[item].interSteps.length - 1]
                          .error
                      "
                      class="red"
                    >
                      {{
                        path[item].interSteps[path[item].interSteps.length - 1]
                          .error
                      }}
                    </div>
                  </div>
                </div>
            </el-card>
          </el-timeline-item>
        </el-timeline>
      </el-tab-pane>
    </el-tabs>
  </div>
  <!-- </el-card> -->
</template>

<script>
import * as cover from "../../assets/pagedoc.json";

export default {
  data() {
    return {
      activeName: "first",
      page: {},
      path: [],
    };
  },
  methods: {
    getposi(bounds) {
      if (bounds) {
        const [x1, y1] = bounds.slice(1, -1).split("][")[0].split(",");
        const [x2, y2] = bounds.slice(1, -1).split("][")[1].split(",");
        console.log([x1 / 6, y1 / 6, (x2 - x1) / 6, (y2 - y1) / 6]);

        return [x1 / 6, y1 / 6, (x2 - x1) / 6, (y2 - y1) / 6];
      }
    },
    getsrc(path) {
      console.log(path);
      const file = path.imgpath.replace("/Users/linbao/zju/uitest/proj/", "");
      return require("../../assets/" + file);
    },
    handleClick(tab, event) {
      console.log(this);
    },
    getData() {
      console.log(cover.default.Pages[this.$route.query.pageId]);
      this.$set(this, "page", cover.default.Pages[this.$route.query.pageId]);
      this.$set(this, "path", cover.default.Paths);
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
.stepimgwrapper {
  position: relative;
  /* background-color: #ccc; */
  float: left;
  margin: 10px;
  height: 360px;
}
.imgwrapper {
  padding: 10px;
}
.ele {
  position: absolute;
  border: 1px solid red;
  color: white;
}
.red {
  color: red;
}
.blue {
  color: blue;
}
.wrapper {
  overflow: hidden;
  /* width: 400px; */
  float: left;
}
.el-card.is-always-shadow,
.el-card.is-hover-shadow:focus,
.el-card.is-hover-shadow:hover {
  overflow: scroll;
}
</style>