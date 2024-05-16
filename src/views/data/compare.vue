<template>
  <div>
    <el-card>
    <el-timeline>
        <el-timeline v-for="(item,index) in pagedata" :key="index" :timestamp="(index + 1) " placement="top">
            <el-card>
                <div class="stepimgwrapper">
                      <img
                        class="stepimg"
                        :src="getsrc(item.pageA)"
                        alt=""
                        width="200px"
                      />
                </div> 
                <div class="stepimgwrapper">
                      <img
                        class="stepimg"
                        :src="getsrc(item.pageB)"
                        alt=""
                        width="200px"
                      />
                </div> 
                <div class="stepimgwrapper">
                    <div><span class="blue">Judgment</span>{{ item.res.Judgment}}</div>
                    <div><span class="blue">Reason</span>{{ item.res.Reason}}</div>
                    <div><span class="blue">Logic</span>{{ item.res.Logic}}</div>
                </div> 
            </el-card>    
        </el-timeline>
    </el-timeline>
    </el-card>
    
  </div>
</template>

<script>
import * as cover from "../../assets/compare.json";
export default {
  data() {
    return {
      pagedata: [],
    };
  },
  methods: {
    
    getsrc(path) {
      const file = path.imgpath.replace(
        "/Users/linbao/zju/uitest/proj/",
        ""
      );
     console.log("../../assets/" + file);

      return require("../../assets/" + file);
    },
    getData() {
      this.pagedata = [];
      cover.default.forEach((item) => {
        const str = item.response.choices[0].message.content
        const start = str.indexOf('{')
        const end = str.lastIndexOf('}')
        console.log(str.slice(start,end+1));
        
        item.res =JSON.parse(str.slice(start,end+1)) 
        this.pagedata.push(item);
      });
    },
  },
  activated() {
    this.getData();
  },
}
</script>

<style scoped>
.stepimgwrapper {
  position: relative;
  /* background-color: #ccc; */
  float: left;
  margin: 10px;
  height: 360px;
  width: 200px;
  font-size: 12px;
}
.blue {
  color: blue;
  width: 80px;
  display: inline-block
}
</style>