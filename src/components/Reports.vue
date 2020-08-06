<!--  -->
<template>
<div class='home_report'>
  <el-breadcrumb separator-class="el-icon-arrow-right">
    <el-breadcrumb-item :to="{ path: '/home' }">首页</el-breadcrumb-item>
    <el-breadcrumb-item>数据统计</el-breadcrumb-item>
    <el-breadcrumb-item>数据报表</el-breadcrumb-item>
  </el-breadcrumb>
  <el-card>
    <div class="report_box" ref="echartRef"></div>
  </el-card>
</div>
</template>

<script>
//这里可以导入其他文件（比如：组件，工具js，第三方插件js，json文件，图片文件等等）
//例如：import 《组件名称》 from '《组件路径》';
import echarts from 'echarts'
import merge from 'lodash/merge'
export default {
//import引入的组件需要注入到对象中才能使用
components: {
  echarts,
  merge
},
data() {
//这里存放数据
return {
  myOption: {
    title: {
      text: '用户来源'
    },
    tooltip: {
      trigger: 'axis',
      axisPointer: {
        type: 'cross',
        label: {
          backgroundColor: '#E9EEF3'
        }
      }
    },
    grid: {
      left: '3%',
      right: '4%',
      bottom: '3%',
      containLabel: true
    },
    xAxis: [
      {
        boundaryGap: false
      }
    ],
    yAxis: [
      {
        type: 'value'
      }
    ]
  }
};
},
//监听属性 类似于data概念
computed: {},
//监控data中的数据变化
watch: {},
//方法集合
methods: {
  showEcharts:async function(){
    let res = await this.$axios.get("/reports/type/1")
    if(res.meta.status != 200){
      this.$message.error("获取数据报表失败！")
    }else if(res.meta.status == 200){
      let result = merge(this.myOption,res.data)
      let myChart = echarts.init(this.$refs.echartRef);
      myChart.setOption(result);
      myChart.resize()
    }
  }
  
},
//生命周期 - 创建完成（可以访问当前this实例）
created() {

},
//生命周期 - 挂载完成（可以访问DOM元素）
mounted() {
  var that = this;
  this.showEcharts();
  window.onresize = function(){
    setTimeout(()=>{
      that.showEcharts()
    },100)
  }
},
beforeCreate() {}, //生命周期 - 创建之前
beforeMount() {}, //生命周期 - 挂载之前
beforeUpdate() {}, //生命周期 - 更新之前
updated() {}, //生命周期 - 更新之后
beforeDestroy() {}, //生命周期 - 销毁之前
destroyed() {}, //生命周期 - 销毁完成
activated() {}, //如果页面有keep-alive缓存功能，这个函数会触发
}
</script>
<style  scoped>
  .report_box{
    width: 100%;
    height: 400px;
    margin: 0 auto;
  }
</style>