<template>
  <div class="middle_bottom_box">
    <div class="box_title" @click="jump">
      <commonTopText
        ref="commonTopText"
        :flag="true"
        :commonTopText="'人员进出统计'"
      ></commonTopText>
    </div>
    <div class="content">
      <div>
        <div
          ref="content_top"
          :style="{ height: 100 + '%', width: 100 + '%' }"
        ></div>
      </div>
      <div class="content_bottom_box">
        <div>
          <div
            ref="content_bottom_box_one"
            :style="{ height: 100 + '%', width: 100 + '%' }"
          ></div>
        </div>
        <div>
          <div
            ref="content_bottom_box_two"
            :style="{ height: 100 + '%', width: 100 + '%' }"
          ></div>
        </div>
        <div>
          <div
            ref="content_bottom_box_three"
            :style="{ height: 100 + '%', width: 100 + '%' }"
          ></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  mounted() {
    // 获取人员进出统计柱状图
    this.getPeopel();
    // 获取底部第一个的环状图
    this.getPeopleOne();
    // 获取底部第二个的环状图
    this.getPeopleTwo();
    // 获取底部第三个的环状图
    this.getPeopleThree();
  },
  methods: {
    // 跳转页面
    jump() {
      this.$refs.commonTopText.goUrl("/peopleOutDetails");
    },
    // 获取人员进出统计柱状图
    getPeopel() {
      let myChart = this.$echarts.init(this.$refs.content_top);
      window.onresize = myChart.resize;
      let option = {
        tooltip: {
          trigger: "axis",
          axisPointer: {
            type: "cross",
            crossStyle: {
              color: "#999",
            },
          },
        },
        legend: {
          left: "right",
          textStyle: {
            color: "#039dc6",
          },
          icon: "rect", // 形状
          itemWidth: 20, // 宽
          itemHeight: 5, // 高
          data: ["进港人员", "离港人员"],
        },
        grid: {
          top: "22%",
          left: "0%",
          right: "0%",
          bottom: "0%",
          containLabel: true,
        },
        xAxis: [
          {
            type: "category",
            data: ["穿山", "梅山", "北仑", "镇海"],
            axisPointer: {
              type: "shadow",
            },
            axisLine: {
              lineStyle: {
                type: "solid",
                color: "#74acef", //左边线的颜色
                width: "2", //坐标线的宽度
              },
            },
          },
        ],
        yAxis: [
          {
            type: "value",
            name: "单位: 人",
            min: 0,
            // axisLabel: {
            //   formatter: '{value} °C',
            // },
            axisLine: {
              lineStyle: {
                type: "solid",
                color: "#74acef", //左边线的颜色
                width: "2", //坐标线的宽度
              },
            },
            splitLine: {
              show: true,
            },
          },
        ],
        series: [
          {
            name: "进港人员",
            type: "bar",
            data: [100, 200, 300, 400],
            barWidth: 20,
            itemStyle: {
              normal: {
                color: new this.$echarts.graphic.LinearGradient(0, 0, 0, 1, [
                  { offset: 0, color: "#00392f" }, //柱图渐变色
                  { offset: 0.5, color: "#005d1f" }, //柱图渐变色
                  { offset: 1, color: "#006f0f" }, //柱图渐变色
                ]),
              },
            },
          },
          {
            name: "离港人员",
            type: "bar",
            data: [150, 160, 170, 180],
            barWidth: 20,
            itemStyle: {
              normal: {
                color: new this.$echarts.graphic.LinearGradient(0, 0, 0, 1, [
                  { offset: 0, color: "#474038" }, //柱图渐变色
                  { offset: 0.5, color: "#8a7236" }, //柱图渐变色
                  { offset: 1, color: "#c39c35" }, //柱图渐变色
                ]),
              },
            },
          },
        ],
      };
      myChart.setOption(option);
    },
    // 获取底部第一个的环状图
    getPeopleOne() {
      let myChart = this.$echarts.init(this.$refs.content_bottom_box_one);
      let data = [
        { value: 800, name: "国内" },
        { value: 200, name: "国外" },
      ];
      let total = 0;
      data.forEach((item) => {
        total += item.value;
      });
      data.map((item) => {
        item.num = ((item.value / total) * 100).toFixed(2);
      });
      window.onresize = myChart.resize;
      var colorList = ["#00baff", "#00d700"];
      // 绘制图表
      let option = {
        tooltip: {
          trigger: "item",
          formatter: function (params) {
            if (params.componentType == "series") {
              var str =
                params.data.name +
                `<span style="color:#ffac29">${params.data.num}%</span>` +
                params.data.value;
              return str;
            } else {
              return "";
            }
          },
        },
        legend: {
          orient: "vertical",
          top: "center",
          // left: "right",
          // type: "scroll",
          textStyle: {
            color: "#039dc6",
          },
          itemWidth: 10,
          itemHeight: 10,
          icon: "circle",
          x: "50%",
          formatter(name) {
            let num = "";
            let total = 0;
            data.forEach((item) => {
              total += item.value;
              if (item.name == name) {
                return (num = item.value);
              }
            });
            let p = (num / total) * 100;
            // return name + p.toFixed(2) + "%";
            return "{a|" + name + "}" + "{b|" + p.toFixed(2) + "%" + "}";
          },
          textStyle: {
            rich: {
              a: {
                color: "#3aa7ee",
                fontSize: 12,
              },
              b: {
                color: "#d18814",
                fontSize: 12,
                padding: 4,
              },
            },
          },
        },
        graphic: {
          elements: [
            {
              type: "text",
              style: {
                text: "国内外",
                width: 200,
                height: 200,
                fill: "#00a1ed",
                fontSize: 10,
              },
              left: "20%",
              top: "45%",
            },
            {
              type: "text",
              style: {
                text: "船只占比",
                width: 200,
                height: 200,
                fill: "#00a1ed",
                fontSize: 10,
              },
              left: "17.5%",
              top: "55%",
            },
          ],
        },
        series: [
          {
            type: "pie",
            radius: [35, 55],
            center: ["25%", "50%"],
            avoidLabelOverlap: false,
            label: {
              show: false,
              position: "center",
            },
            labelLine: {
              show: false,
            },
            data: data,
            // 设置圆环颜色
            itemStyle: {
              normal: {
                color: function (params) {
                  return colorList[params.dataIndex];
                },
              },
            },
          },
        ],
      };
      myChart.setOption(option);
    },
    // 获取底部第二个的环状图
    getPeopleTwo() {
      let myChart = this.$echarts.init(this.$refs.content_bottom_box_two);
      let data = [
        { value: 1048, name: "意大利" },
        { value: 735, name: "菲律宾" },
        { value: 580, name: "日本" },
        { value: 484, name: "俄罗斯" },
        { value: 300, name: "马来西亚" },
        { value: 200, name: "韩国" },
      ];
      let total = 0;
      data.forEach((item) => {
        total += item.value;
      });
      data.map((item) => {
        item.num = ((item.value / total) * 100).toFixed(2);
      });
      window.onresize = myChart.resize;
      var colorList = [
        "#f8b58a",
        "#f888b0",
        "#8999f8",
        "#89d1f9",
        "#f7f188",
        "#f9da88",
      ];
      // 绘制图表
      let option = {
        tooltip: {
          trigger: "item",
          formatter: function (params) {
            if (params.componentType == "series") {
              var str =
                params.data.name +
                `<span style="color:#ffac29">${params.data.num}%</span>` +
                params.data.value;
              return str;
            } else {
              return "";
            }
          },
        },
        legend: {
          orient: "vertical",
          // top: "center",
          // left: "right",
          // type: "scroll",
          textStyle: {
            color: "#039dc6",
          },
          itemWidth: 10,
          itemHeight: 10,
          icon: "circle",
          x: "50%",
          formatter(name) {
            let num = "";
            let total = 0;
            data.forEach((item) => {
              total += item.value;
              if (item.name == name) {
                return (num = item.value);
              }
            });
            let p = (num / total) * 100;
            // return name + p.toFixed(2) + "%";
            return "{a|" + name + "}" + "{b|" + p.toFixed(2) + "%" + "}";
          },
          textStyle: {
            rich: {
              a: {
                color: "#3aa7ee",
                fontSize: 12,
              },
              b: {
                color: "#d18814",
                fontSize: 12,
                padding: 4,
              },
            },
          },
        },
        graphic: {
          elements: [
            {
              type: "text",
              style: {
                text: "国外",
                width: 200,
                height: 200,
                fill: "#00a1ed",
                fontSize: 12,
              },
              left: "20%",
              top: "40%",
            },
            {
              type: "text",
              style: {
                text: "船只占比",
                width: 200,
                height: 200,
                fill: "#00a1ed",
                fontSize: 12,
              },
              left: "15%",
              top: "50%",
            },
          ],
        },
        series: [
          {
            type: "pie",
            radius: [35, 55],
            center: ["25%", "50%"],
            avoidLabelOverlap: false,
            label: {
              show: false,
              position: "center",
            },
            labelLine: {
              show: false,
            },
            data: data,
            // 设置圆环颜色
            itemStyle: {
              normal: {
                color: function (params) {
                  return colorList[params.dataIndex];
                },
              },
            },
          },
        ],
      };
      myChart.setOption(option);
    },
    // 获取底部第三个的环状图
    getPeopleThree() {
      let myChart = this.$echarts.init(this.$refs.content_bottom_box_three);
      let data = [
        { value: 1048, name: "浙江省" },
        { value: 735, name: "江苏省" },
        { value: 580, name: "福建省" },
        { value: 484, name: "山东省" },
        { value: 300, name: "上海" },
        { value: 200, name: "天津" },
      ];
      let total = 0;
      data.forEach((item) => {
        total += item.value;
      });
      data.map((item) => {
        item.num = ((item.value / total) * 100).toFixed(2);
      });
      window.onresize = myChart.resize;
      var colorList = [
        "#6b4bff",
        "#ff6045",
        "#0096ff",
        "#00e492",
        "#f8c451",
        "#ffef00",
      ];
      // 绘制图表
      let option = {
        tooltip: {
          trigger: "item",
          formatter: function (params) {
            if (params.componentType == "series") {
              var str =
                params.data.name +
                `<span style="color:#ffac29">${params.data.num}%</span>` +
                params.data.value;
              return str;
            } else {
              return "";
            }
          },
        },
        legend: {
          orient: "vertical",
          // top: "center",
          // left: "right",
          // type: "scroll",
          textStyle: {
            color: "#039dc6",
          },
          itemWidth: 10,
          itemHeight: 10,
          icon: "circle",
          x: "55%",
          formatter(name) {
            let num = "";
            let total = 0;
            data.forEach((item) => {
              total += item.value;
              if (item.name == name) {
                return (num = item.value);
              }
            });
            let p = (num / total) * 100;
            // return name + p.toFixed(2) + "%";
            return "{a|" + name + "}" + "{b|" + p.toFixed(2) + "%" + "}";
          },
          textStyle: {
            rich: {
              a: {
                color: "#3aa7ee",
                fontSize: 12,
              },
              b: {
                color: "#d18814",
                fontSize: 12,
                padding: 4,
              },
            },
          },
        },
        graphic: {
          elements: [
            {
              type: "text",
              style: {
                text: "船只比例",
                width: 200,
                height: 200,
                fill: "#00a1ed",
                fontSize: 12,
              },
              left: "20%",
              top: "40%",
            },
            {
              type: "text",
              style: {
                text: "(省份)",
                width: 200,
                height: 200,
                fill: "#00a1ed",
                fontSize: 12,
              },
              left: "22.5%",
              top: "50%",
            },
          ],
        },
        series: [
          {
            type: "pie",
            radius: [35, 55],
            center: ["30%", "50%"],
            avoidLabelOverlap: false,
            label: {
              show: false,
              position: "center",
            },
            labelLine: {
              show: false,
            },
            data: data,
            // 设置圆环颜色
            itemStyle: {
              normal: {
                color: function (params) {
                  return colorList[params.dataIndex];
                },
              },
            },
          },
        ],
      };
      myChart.setOption(option);
    },
  },
};
</script>

<style lang="less" scoped>
.middle_bottom_box {
  .box_title {
    width: 22%;
    color: #50f4c1;
  }
  .content {
    display: flex;
    flex-direction: column;
    align-items: center;
    height: 100%;
    > div {
      height: 50%;
      width: 100%;
    }
    .content_bottom_box {
      display: flex;
      align-items: center;
      justify-content: space-between;
      > div {
        height: 100%;
        width: 33.33%;
      }
    }
  }
}
</style>