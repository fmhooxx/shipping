<template>
  <div>
    <commonTitle
      :type="true"
      :commonTitleText="'数智港警 — 精密智控'"
    ></commonTitle>
    <div class="table_box">
      <iframe
        style="margin-top: 0.8rem"
        ref="iframe"
        src="http://41.204.15.209:10219"
        width="100%"
        height="100%"
        frameborder="0"
      />
      <!-- <el-table
        :data="tableData"
        style="width: 100%"
        :header-cell-style="{ background: '#00446f', color: '#fff' }"
      >
        <el-table-column align="center" label="人脸照片">
          <template slot-scope="scope">
            <img class="table_box_img" :src="scope.row.img_url" alt="" />
          </template>
        </el-table-column>
        <el-table-column align="center" prop="passtime" label="拍摄时间">
        </el-table-column>
        <el-table-column align="center" prop="address" label="拍摄地点">
        </el-table-column>
      </el-table> -->
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // tableData: [],
      // urlSrc: "http://41.204.103.50:11001/pages/rlkk/rlList.jsp",
      urlSrc: "http://41.204.15.209:10219",
    };
  },
  created() {
    // this.getLsit();
  },
  mounted() {
    // console.log("------");
    // console.log(window.location);
    // console.log("------");
    // // console.log(this.$refs.iframe);
    // // console.log(this.$refs.iframe.contentWindow);
    // window.onmessage = function (e) {
    //   console.log("从父域获取到的数据", e);
    // };
  },
  methods: {
    // getLsit() {
    //   this.$http.post("http://41.204.103.50:11001/bk/rl_list").then((res) => {
    //     console.log(res);
    //     this.tableData = res.data;
    //   });
    // },
  },
};
</script>

<style lang="less" scoped>
.table_box {
  margin-top: 0.4rem;
  height: 100%;
  /deep/.el-form-item__label {
    color: #fff;
  }
  /deep/.el-table {
    background-color: transparent;
  }
  // /deep/.el-table td {
  //   padding: 0rem 0;
  // }
  // /deep/.el-table th {
  //   padding: 0.1rem 0;
  // }
  /deep/.el-table__header {
    border: 1px solid #1c5683;
    margin-bottom: 0.1rem;
    table-layout: auto;
    background-color: #00416b;
  }
  /deep/.el-table th.is-leaf {
    border: none;
  }
  /deep/.el-table tr {
    background-color: #25456e;
    color: #fff;
  }
  /deep/.el-table tbody tr:hover > td {
    background-color: #096c7f;
  }
  /deep/.is-center {
    border: 0;
  }
  /deep/.el-table::before {
    background-color: transparent;
  }
  .table_box_img {
    width: 3rem;
    height: 2rem;
  }
}
</style>