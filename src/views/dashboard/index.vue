
<template>
  <div id="main">
    <div class="head">
    <!-- <div class="dashboard-title">C919型机体损伤可视化平台</div> -->
    <div class="dashboard-title">设备损伤可视化平台</div>

    </div>

    <!-- 扇形图 -->
    <!-- <div ref="piechart" style="width:368px;height:368px"></div> -->
        <!-- 扇形图 -->
    <div id="piechart" ref="piechart1" style="width:308px;height:308px"></div>
    <!-- 柱状图 -->
    <div ref="chart" style="width:368px;height:368px"></div>
    <!-- 折线图 -->
    <div ref="chart1" style="width:468px;height:468px"></div>

  </div>

</template>

<script>

  export default {
    data() {

    },
    mounted() {
       //柱状图
      this.getEchartData()
      //折线图
      this.getEchartData1()
      //扇形图
      this.getEchartData2()

    },
    methods: {
      //柱状图
      getEchartData() {
        const chart = this.$refs.chart
        if (chart) {
          const myChart = this.$echarts.init(chart)

          const option = { 
            legend: {},
            tooltip: {},
            dataset: {
              source: [
                ['订单', 43.3, 85.8],
                ['订单1', 83.1, 73.4],
                ['订单2', 86.4, 65.2],
                ['订单3', 72.4, 53.9],
                ['订单4', 82.4, 53.9],
                ['订单5', 42.4, 53.9],
                ['订单6', 72.4, 53.9],
                ['订单7', 72.4, 53.9]
              ]
            },
            xAxis: { type: 'category' },
            yAxis: {},
            series: [ { type: 'bar' }, { type: 'bar' }]
            }

          myChart.setOption(option)
          window.addEventListener("resize", function() {
            myChart.resize()
          })
        }
        this.$on('hook:destroyed',()=>{
          window.removeEventListener("resize", function() {
            myChart.resize();
          });
        })
      },

      //折线图
      getEchartData1() {
        const chart1 = this.$refs.chart1
        if (chart1) {
          const myChart = this.$echarts.init(chart1);
          const option = {
            // title: {
            //   text: '各系统月度损伤报比',
            //   left: 'center',
            //   top: 0,
            //   textStyle: {
            // color: '#fff'
            // }
            // },
            tooltip: {
              trigger: 'axis'
            },
            legend: {
                  data: ['发动机系统', 'APU', '燃油系统', '液压系统', '空调系统','电源系统','防火系统','氧气系统','起落架','导航系统','气源系统'],
        textStyle: {
            color: '#fff'
        }
            },
            grid: {
              left: '3%',
              right: '4%',
              bottom: '3%',
              containLabel: true
            },
            toolbox: {
              feature: {
                // saveAsImage: {}
              }
            },
            xAxis: {
              type: 'category',
              boundaryGap: false,
              data: ['一月', '二月', '三月', '四月', '五月', '六月', '七月','八月','九月','十月','十一月','十二月']
            },
            yAxis: {
              type: 'value'
            },
            series: [
              {
                name: '发动机系统',
                type: 'line',
                stack: 'Total',
                data: [120, 132, 101, 134, 90, 230, 210,120, 132, 101, 134, 90, 230]
              },
              {
                name: 'APU',
                type: 'line',
                stack: 'Total',
                data: [220, 182, 191, 234, 290, 330, 310,220, 182, 191, 234, 290, 330]
              },
              {
                name: '燃油系统',
                type: 'line',
                stack: 'Total',
                data: [150, 232, 201, 154, 190, 330, 410,150, 232, 201, 154, 190, 330]
              },
              {
                name: '液压系统',
                type: 'line',
                stack: 'Total',
                data: [320, 332, 301, 334, 390, 330, 320,320, 332, 301, 334, 390, 330]
              },
              {
                name: '空调系统',
                type: 'line',
                stack: 'Total',
                data: [820, 932, 901, 934, 1290, 1330, 1320,820, 932, 901, 934, 1290, 1330]
              },
              {
                name: '电源系统',
                type: 'line',
                stack: 'Total',
                data: [820, 932, 901, 934, 1290, 1330, 1320,820, 932, 901, 934, 1290, 1330]
              },
              {
                name: '防火系统',
                type: 'line',
                stack: 'Total',
                data: [820, 932, 901, 934, 1290, 1330, 1320,820, 932, 901, 934, 1290, 1330]
              },
              {
                name: '起落架',
                type: 'line',
                stack: 'Total',
                data: [820, 932, 901, 934, 1290, 1330, 1320,820, 932, 901, 934, 1290, 1330]
              },
              {
                name: '导航系统',
                type: 'line',
                stack: 'Total',
                data: [820, 932, 901, 934, 1290, 1330, 1320,820, 932, 901, 934, 1290, 1330]
              },
              {
                name: '气源系统',
                type: 'line',
                stack: 'Total',
                data: [820, 932, 901, 934, 1290, 1330, 1320,820, 932, 901, 934, 1290, 1330]
              }   
            ]

          }
          myChart.setOption(option)
          window.addEventListener("resize", function() {
            myChart.resize()
          })
        }
        this.$on('hook:destroyed',()=>{
          window.removeEventListener("resize", function() {
            myChart.resize();
          });
        })
      },
     
     //饼图
      getEchartData3() {
        const piechart = this.$refs.piechart
        if (piechart) {
          const myChart = this.$echarts.init(piechart)
          const option = {
            title: {
              text: '各系统损伤程度占比'
            },
            tooltip: {
              trigger: 'axis'
            },
            // legend: {
            //   data: ['Email', 'Union Ads', 'Video Ads', 'Direct', 'Search Engine']
            // },
            // grid: {
            //   left: '3%',
            //   right: '4%',
            //   bottom: '3%',
            //   containLabel: true
            // },
            // toolbox: {
            //   feature: {
            //     saveAsImage: {}
            //   }
            // },
            // xAxis: {
            //   type: 'category',
            //   boundaryGap: false,
            //   data: ['一月', '二月', '三月', '四月', '五月', '六月', '七月','八月','九月','十月','十一月','十二月']
            // },
            // yAxis: {
            //   type: 'value'
            // },
           series: [
    {
      type: 'pie',
      


    //标签
            // itemStyle : {
            //     normal : {
            //         label:{
            //             show : true,
            //             position : 'inner',
            //             formatter: '{d}%',
            //             distance : 0.7, //这项是标识距离中心点的距离
            //             textStyle : {                   
            //                 align : 'center',
            //                 baseline : 'middle',
            //                 fontFamily : '微软雅黑',
            //                 fontSize : 10,
            //                 fontWeight : 'bolder'
            //              }
            //         },
            //         labelLine:{
            //             show : true
            //         }
            //     }
            // },






      data: [
        {
          value: 400,
          name: '发动机系统'
        },
        {
          value: 335,
          name: 'APU'
        },
        {
          value: 234,
          name: '燃油系统'
        },
        {
          value: 200,
          name: '液压系统'
        },
        {
          value: 190,
          name: '空调系统'
        },
        {
          value: 180,
          name: '电源系统'
        },
        {
          value: 170,
          name: '防火系统'
        },                
        {
          value: 160,
          name: '氧气系统'
        },  
        {
          value: 154,
          name: '起落架'
        },  
        {
          value: 153,
          name: '导航系统'
        },  
        {
          value: 130,
          name: '气源系统'
        },     
      ],
      // radius: '50%',
      radius: ['10%', '80%'],
      roseType: 'area'
    }
  ]

          }
          myChart.setOption(option)
          window.addEventListener("resize", function() {
            myChart.resize()
          })
        }
        this.$on('hook:destroyed',()=>{
          window.removeEventListener("resize", function() {
            myChart.resize();
          });
        })
      },
     

     //饼图2
      getEchartData2() {
        const piechart1 = this.$refs.piechart1
        if (piechart1) {
          const myChart = this.$echarts.init(piechart1)
          const option = {
    // backgroundColor: '#013954',
    title: {
        text: '系统损伤程度占比',
        left: 'center',
        top: 5,
        textStyle: {
            color: '#fff'
        }
    },
    tooltip: {
        trigger: 'item',
        formatter: '系统损伤程度<br/>{b} : {c} ({d}%)'
    },
    //侧边的矩形提示框
    legend: {
        orient: 'vertical',
        left: 10,
        top: 20,
        data: ['发动机系统', 'APU', '燃油系统', '液压系统', '空调系统','电源系统','防火系统','氧气系统','起落架','导航系统','气源系统'],
        textStyle: {
            color: '#fff'
        }
    },
    series: [{
        name: '系统损伤程度',
        type: 'pie',
        radius: '85%',
        center: ['50%', '50%'],
    data: [
        {
          value: 300,
          name: '发动机系统'
        },
        {
          value: 335,
          name: 'APU'
        },
        {
          value: 234,
          name: '燃油系统'
        },
        {
          value: 200,
          name: '液压系统'
        },
        {
          value: 190,
          name: '空调系统'
        },
        {
          value: 180,
          name: '电源系统'
        },
        {
          value: 170,
          name: '防火系统'
        },                
        {
          value: 160,
          name: '氧气系统'
        },  
        {
          value: 154,
          name: '起落架'
        },  
        {
          value: 153,
          name: '导航系统'
        },  
        {
          value: 130,
          name: '气源系统'
        },     
      ],
        roseType: 'radius',
        label: {
            color: '#fff',
            show:false
        },
        labelLine: {
            lineStyle: {
                color: '#888',
                normal:{
                show:false
              }
            }
        },
        itemStyle: {
            normal: {
                color: function(params) {
                    var colorList = ['#f845f1', '#ad46f3', '#5045f6', '#4777f5', '#44aff0', '#45dbf7', '#f6d54a', '#f69846', '#ff4343'];
                    return colorList[params.dataIndex % colorList.length];
                }
            }
        },
        animationType: 'scale',
        animationEasing: 'elasticOut',
        animationDelay: function(idx) {
            return Math.random() * 200;
        }
    }]
};
          myChart.setOption(option)
          window.addEventListener("resize", function() {
            myChart.resize()
          })
        }
        this.$on('hook:destroyed',()=>{
          window.removeEventListener("resize", function() {
            myChart.resize();
          });
        })
      },
  
    },
    watch: {},
    created() {
    }
  }
</script>
<style lang="scss" scoped>
#main{
  // background-color: #e0e9f0;

  // background-color: #0d3688;

  
  // background-image: url('./assest/bg1.jpg');
  background-image: url('./assest/705.jpg');
  background-repeat: no-repeat;
  .head{
    width: 100%;
    // background-color: #012da6;
    background-image: url('./header1.png');
    background-size: 100%;
    display: flex;
    justify-content: center;
    .dashboard-title{
    font-size: 20px;
    color: #fff;
    font-weight: bold;
    height: 30px;
    
    // color:blue;
  }
  }

  // 扇形图
  #piechart{
    transform: translate(100px,70px);
  // background-image: url('./assest/703.jpg');
  // background-color: red;
    
  }

}
</style>



