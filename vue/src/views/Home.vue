<template>
  <div style="padding: 10px">
    <el-row :gutter="10">
      <el-col :span="12">
        <el-card>
          <div id="myChart" :style="{width: '600px', height: '500px'}"></div>
        </el-card>
      </el-col>

      <el-col :span="12">
        <el-card>
          <template #header>
            <div class="card-header">
              <span>VIP服务</span>
            </div>
          </template>
          <div style="text-align: left">
            <div style="padding: 10px 0">B站：程序员青戈。 代码和资料全部在VIP群里开放</div>

            <div style="padding: 10px 0">永久VIP 99元，扫下方二维码然后加我的企业微信，就可以进企业微信VIP群了。</div>
            <div>
              <img src="https://img-blog.csdnimg.cn/297ce70543654adfbcd34b2826e5801b.png" alt="">
            </div>
            <div style="line-height: 25px">
              <h3>服务内容：</h3><br>

              1. Q&A服务（问答服务）<br>
              2. VIP代码生成器源码和使用教程<br>
              3. B站所有的成品项目源码（持续更新）<br>
              4. 所有笔记文档<br>
              5. 模板之家静态模板下载服务<br>
              6. 后续所有VIP服务内容都在（包括小程序）<br>
              7. 必备软件下载和破解<br>
            </div>
          </div>
        </el-card>
      </el-col>
    </el-row>

  </div>
</template>

<script>
import request from "@/utils/request";

export default {
  name: "Home",
  data() {
    return {}
  },
  mounted() {
    this.drawLine();
  },
  methods: {
    drawLine() {
      // 基于准备好的dom，初始化echarts实例
      let myChart = this.$root.echarts.init(document.getElementById('myChart'))
      let option = {
        title: {
          text: '各地区用户比例统计图',
          subtext: '虚拟数据',
          left: 'left'
        },
        tooltip: {
          trigger: 'item'
        },
        legend: {
          orient: 'vertical',
          trigger: 'item',
          left: 'center'
        },
        toolbox: {
          show: true,
          feature: {
            mark: {show: true},
            dataView: {show: true, readOnly: false},
            restore: {show: true},
            saveAsImage: {show: true}
          }
        },
        series: [
          {
            name: '用户比例',
            type: 'pie',
            radius: [50, 150],
            center: ['50%', '60%'],
            roseType: 'area',
            itemStyle: {
              borderRadius: 8
            },
            data: []
          }
        ]
      }
      request.get("/user/count").then(res => {
        if (res.code === '0') {
          res.data.forEach(item => {
            option.series[0].data.push({name: item.address, value: item.count})
          })
          // 绘制图表
          myChart.setOption(option);
        }
      })

    }
  }
}
</script>

<style scoped>

</style>
