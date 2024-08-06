
<template>
  <div id="main">
    <div class="head">
      <!-- 顶部标题 -->
      <!-- <div class="dashboard-title">C919型机体损伤可视化态势感知平台</div> -->
      <div class="dashboard-title">设备损伤可视化平台</div>
    </div>

    <div class="title2">
      <div class="centertitle"><span class="hour">3000</span>飞行小时损伤机体数字孪生</div>

    </div>

    <!-- 主体内容 -->
    <div class="mainbody">
     <div class="leftbody">

    <!-- 扇形图 -->
    <div
      id="piechart"
      ref="piechart1"
      style="width: 268px; height: 268px"
    >
    </div>

    <!-- 柱状图 -->
    <!-- <div id="barchart" ref="barchart" style="width: 168px; height: 168px"></div> -->

      <!-- 雷达图 -->
    <div ref="raderchart" id="raderchart" style="width: 468px; height: 246px"></div>
    

    <!-- 折线图 -->
    <!-- <div ref="chart1" style="width: 468px; height: 468px"></div> -->
    </div>

    <div class="centerbody">
    
    <div class="top">
    <div class="airplane">
    <canvas id="canvas"></canvas>
    </div>
    <img class="img" src="../../../public/img/001.jpg" alt="" width="10px" height="60px">
    </div>


    <div class="livechart">
      
    </div>
      
    </div>

    <div class="rightbody">
    
    <!-- 仪表盘 -->
    <div ref="ratechart" id="ratechart" style="width: 298px; height: 286px"></div>

    </div>
    </div>


  </div>
</template>

<script>
import * as THREE from 'three';
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls';
import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader';
import { EXRLoader } from 'three/examples/jsm/loaders/EXRLoader';
// import echarts from "echarts"
import * as echarts from 'echarts';

// import { OrbitControls } from 'three/addons/controls/OrbitControls.js';
// import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js';
// import { EXRLoader } from 'three/addons/loaders/EXRLoader.js';
export default {
  data() {},
  mounted() {
    this.initThreeJS();
    //柱状图
    this.getBarEchartData();
    //折线图
    this.getEchartData1();
    //扇形图
    this.getEchartData2();
    //雷达图
    this.getRaderChart();

    //仪表盘
    this.getRateEchartData();
  },
  methods: {

    initThreeJS() {
      const canvas = document.querySelector('#canvas');
      const renderer = new THREE.WebGLRenderer({ canvas, antialias: true });
      //调节飞机的背景颜色
      renderer.setClearColor(0x000000,0);


      const camera = new THREE.PerspectiveCamera(75, 1, 0.1, 100);
      // camera.position.set(6, 6, 6);
      camera.position.set(4.6, 3.0,4.7);
      camera.lookAt(0, 0, 0);


      const controls = new OrbitControls(camera, renderer.domElement);
      controls.enableDamping = true;


      /**
       * Loaders
       */
      const gltfLoader = new GLTFLoader();
      const exrLoader = new EXRLoader();


      const scene = new THREE.Scene();


      /**
       * Models
       */
      gltfLoader.load('/models/aircraft/scene.glb', (gltf) => {
        console.log(gltf.scene);
        scene.add(gltf.scene);
      });


      /**
       * Environment Map
       */
      exrLoader.load(
        '/textures/environmentMaps/nvidiaCanvas-4k.exr',
        (environmentMap) => {
          environmentMap.mapping = THREE.EquirectangularReflectionMapping;
          scene.environment = environmentMap;
        }
      );


      function resizeRendererToDisplaySize() {
        const canvas = renderer.domElement;
        const width = (canvas.clientWidth * window.devicePixelRatio) | 0;
        const height = (canvas.clientHeight * window.devicePixelRatio) | 0;
        const needResize = canvas.width!== width || canvas.height!== height;
        if (needResize) {
          renderer.setSize(width, height, false);
        }
        return needResize;
      }


      function render() {
        if (resizeRendererToDisplaySize()) {
          const canvas = renderer.domElement;
          camera.aspect = canvas.clientWidth / canvas.clientHeight;
          camera.updateProjectionMatrix();
        }


        renderer.render(scene, camera);
        controls.update();
        // console.log(123,camera.position)


        requestAnimationFrame(render);
      }


      render();
    }
  ,


    //饼图2
    getEchartData2() {
      const piechart1 = this.$refs.piechart1;
      if (piechart1) {
        const myChart = this.$echarts.init(piechart1);
        const option = {
          // backgroundColor: '#013954',
          title: {
            text: "系统损伤程度占比",
            left: "center",
            top: 5,
            textStyle: {
              color: "#85cfe3",
            },
          },
          tooltip: {
            trigger: "item",
            formatter: "系统损伤程度<br/>{b} : {c} ({d}%)",
          },
          //侧边的矩形提示框
          legend: {
            orient: "vertical",
            left: 10,
            top: 20,
            data: [
              "发动机系统",
              "APU",
              "燃油系统",
              "液压系统",
              "电源系统",
              "导航系统",
              "起落架",
              "气源系统",
            ],
            textStyle: {
              color: "#fff",
            },
          },
          series: [
            {
              name: "系统损伤程度",
              type: "pie",
              radius: "85%",
              center: ["50%", "50%"],
              data: [
                {
                  value: 300,
                  name: "发动机系统",
                },
                {
                  value: 335,
                  name: "APU",
                },
                {
                  value: 234,
                  name: "燃油系统",
                },
                {
                  value: 200,
                  name: "液压系统",
                },
                {
                  value: 180,
                  name: "电源系统",
                },
                {
                  value: 154,
                  name: "起落架",
                },
                {
                  value: 153,
                  name: "导航系统",
                },
                {
                  value: 130,
                  name: "气源系统",
                },
              ],
              roseType: "radius",
              label: {
                color: "#fff",
                show: false,
              },
              labelLine: {
                lineStyle: {
                  color: "#888",
                  normal: {
                    show: false,
                  },
                },
              },
              itemStyle: {
                normal: {
                  color: function (params) {
                    var colorList = [
                      "#f845f1",
                      "#ad46f3",
                      "#5045f6",
                      "#4777f5",
                      "#44aff0",
                      "#45dbf7",
                      "#f6d54a",
                      "#f69846",
                      "#ff4343",
                    ];
                    return colorList[params.dataIndex % colorList.length];
                  },
                },
              },
              animationType: "scale",
              animationEasing: "elasticOut",
              animationDelay: function (idx) {
                return Math.random() * 200;
              },
            },
          ],
        };
        myChart.setOption(option);
        window.addEventListener("resize", function () {
          myChart.resize();
        });
      }
      this.$on("hook:destroyed", () => {
        window.removeEventListener("resize", function () {
          myChart.resize();
        });
      });
    },




    //柱状图
    getBarEchartData() {
      const barchart = this.$refs.barchart;
      if (barchart) {
        const myChart = this.$echarts.init(barchart);

var data = [{
    "name": "一月",
    "value": 80
}, {
    "name": "二月",
    "value": 87.8
}, {
    "name": "三月",
    "value": 71
}, {
    "name": "四月",
    "value": 80
}, {
    "name": "五月",
    "value": 66
}, {
    "name": "六月",
    "value": 80
}, {
    "name": "七月",
    "value": 80
}];
var xData = [],
    yData = [];
var min = 50; 
data.map(function(a, b) {
    xData.push(a.name);
    if (a.value === 0) {
        yData.push(a.value + min);
    } else {
        yData.push(a.value);
    }
});
 const option = {
    backgroundColor:"#111c4e",
    color: ['#3398DB'],
    tooltip: {
        trigger: 'axis',
        axisPointer: {
            type: 'line',
            lineStyle: {
                opacity: 0
            }
        },
        formatter: function(prams) {
            if (prams[0].data === min) {
                return "合格率：0%"
            } else {
                return "合格率：" + prams[0].data + "%"
            }
        }
    },
    legend: {
        data: ['直接访问', '背景'],
        show: false
    },
    grid: {
        left: '0%',
        right: '0%',
        bottom: '5%',
        top: '7%',
        height: '85%',
        containLabel: true,
        z: 22
    },
    xAxis: [{
        type: 'category',
        gridIndex: 0,
        data: xData,
        axisTick: {
            alignWithLabel: true
        },
        axisLine: {
            lineStyle: {
                color: '#0c3b71'
            }
        },
        axisLabel: {
            show: true,
             color: 'rgb(170,170,170)',
             fontSize:16
        }
    }],
    yAxis: [{
            type: 'value',
            gridIndex: 0,
            splitLine: {
                show: false
            },
            axisTick: {
                show: false
            },
            min: min,
            max: 100,
            axisLine: {
                lineStyle: {
                    color: '#0c3b71'
                }
            },
            axisLabel: {
                color: 'rgb(170,170,170)',
                formatter: '{value} %'
            }
        },
        {
            type: 'value',
            gridIndex: 0,
            min: min,
            max: 100,
            splitNumber: 12,
            splitLine: {
                show: false
            },
            axisLine: {
                show: false
            },
            axisTick: {
                show: false
            },
            axisLabel: {
                show: false
            },
            splitArea: {
                show: true,
                areaStyle: {
                    color: ['rgba(250,250,250,0.0)', 'rgba(250,250,250,0.05)']
                }
            }
        }
    ],
    series: [{
            name: '合格率',
            type: 'bar',
            barWidth: '30%',
            xAxisIndex: 0,
            yAxisIndex: 0,
            itemStyle: {
                normal: {
                    barBorderRadius: 30,
                    color: new this.echarts.graphic.LinearGradient(
                        0, 0, 0, 1, [{
                                offset: 0,
                                color: '#00feff'
                            },
                            {
                                offset: 0.5,
                                color: '#027eff'
                            },
                            {
                                offset: 1,
                                color: '#0286ff'
                            }
                        ]
                    )
                }
            },
            data: yData,
            zlevel: 11

        },
        {
            name: '背景',
            type: 'bar',
            barWidth: '50%',
            xAxisIndex: 0,
            yAxisIndex: 1,
            barGap: '-135%',
            data: [100, 100, 100, 100, 100, 100, 100],
            itemStyle: {
                normal: {
                    color: 'rgba(255,255,255,0.1)'
                }
            },
            zlevel: 9
        },
      
    ]
};



        myChart.setOption(option);
        window.addEventListener("resize", function () {
          myChart.resize();
        });
      }
      this.$on("hook:destroyed", () => {
        window.removeEventListener("resize", function () {
          myChart.resize();
        });
      });
    },

    //折线图
    getEchartData1() {
      const chart1 = this.$refs.chart1;
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
            trigger: "axis",
          },
          legend: {
            data: [
              "发动机系统",
              "APU",
              "燃油系统",
              "液压系统",
              "空调系统",
              "电源系统",
              "防火系统",
              "氧气系统",
              "起落架",
              "导航系统",
              "气源系统",
            ],
            textStyle: {
              color: "#fff",
            },
          },
          grid: {
            left: "3%",
            right: "4%",
            bottom: "3%",
            containLabel: true,
          },
          toolbox: {
            feature: {
              // saveAsImage: {}
            },
          },
          xAxis: {
            type: "category",
            boundaryGap: false,
            data: [
              "一月",
              "二月",
              "三月",
              "四月",
              "五月",
              "六月",
              "七月",
              "八月",
              "九月",
              "十月",
              "十一月",
              "十二月",
            ],
          },
          yAxis: {
            type: "value",
          },
          series: [
            {
              name: "发动机系统",
              type: "line",
              stack: "Total",
              data: [
                120, 132, 101, 134, 90, 230, 210, 120, 132, 101, 134, 90, 230,
              ],
            },
            {
              name: "APU",
              type: "line",
              stack: "Total",
              data: [
                220, 182, 191, 234, 290, 330, 310, 220, 182, 191, 234, 290, 330,
              ],
            },
            {
              name: "燃油系统",
              type: "line",
              stack: "Total",
              data: [
                150, 232, 201, 154, 190, 330, 410, 150, 232, 201, 154, 190, 330,
              ],
            },
            {
              name: "液压系统",
              type: "line",
              stack: "Total",
              data: [
                320, 332, 301, 334, 390, 330, 320, 320, 332, 301, 334, 390, 330,
              ],
            },
            {
              name: "空调系统",
              type: "line",
              stack: "Total",
              data: [
                820, 932, 901, 934, 1290, 1330, 1320, 820, 932, 901, 934, 1290,
                1330,
              ],
            },
            {
              name: "电源系统",
              type: "line",
              stack: "Total",
              data: [
                820, 932, 901, 934, 1290, 1330, 1320, 820, 932, 901, 934, 1290,
                1330,
              ],
            },
            {
              name: "防火系统",
              type: "line",
              stack: "Total",
              data: [
                820, 932, 901, 934, 1290, 1330, 1320, 820, 932, 901, 934, 1290,
                1330,
              ],
            },
            {
              name: "起落架",
              type: "line",
              stack: "Total",
              data: [
                820, 932, 901, 934, 1290, 1330, 1320, 820, 932, 901, 934, 1290,
                1330,
              ],
            },
            {
              name: "导航系统",
              type: "line",
              stack: "Total",
              data: [
                820, 932, 901, 934, 1290, 1330, 1320, 820, 932, 901, 934, 1290,
                1330,
              ],
            },
            {
              name: "气源系统",
              type: "line",
              stack: "Total",
              data: [
                820, 932, 901, 934, 1290, 1330, 1320, 820, 932, 901, 934, 1290,
                1330,
              ],
            },
          ],
        };
        myChart.setOption(option);
        window.addEventListener("resize", function () {
          myChart.resize();
        });
      }
      this.$on("hook:destroyed", () => {
        window.removeEventListener("resize", function () {
          myChart.resize();
        });
      });
    },

    //饼图
    getEchartData3() {
      const piechart = this.$refs.piechart;
      if (piechart) {
        const myChart = this.$echarts.init(piechart);
        const option = {
          title: {
            text: "各系统损伤程度占比",
          },
          tooltip: {
            trigger: "axis",
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
              type: "pie",

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
                  name: "发动机系统",
                },
                {
                  value: 335,
                  name: "APU",
                },
                {
                  value: 234,
                  name: "燃油系统",
                },
                {
                  value: 200,
                  name: "液压系统",
                },
                {
                  value: 180,
                  name: "电源系统",
                },
                {
                  value: 153,
                  name: "导航系统",
                },
              ],
              // radius: '50%',
              radius: ["10%", "80%"],
              roseType: "area",
            },
          ],
        };
        myChart.setOption(option);
        window.addEventListener("resize", function () {
          myChart.resize();
        });
      }
      this.$on("hook:destroyed", () => {
        window.removeEventListener("resize", function () {
          myChart.resize();
        });
      });
    },



    //雷达图
    getRaderChart(){
        const raderchart = this.$refs.raderchart;
      if (raderchart) {
        const myChart = this.$echarts.init(raderchart);

var legendData = ['机体损伤原因分析', '']; //图例

var indicator = [{
        text: '疲劳损伤',
        max: 30,
    },
    {
        text: '环境因素',
        max: 30
    },
    {
        text: '机械磨损',
        max: 30
    },
    {
        text: '过载与冲击',
        max: 30,
        //  axisLabel: {show: true, textStyle: {fontSize: 18, color: '#333'}}
    },
    {
        text: '制造缺陷',
        max: 30
    },
    {
        text: '外来物损伤',
        max: 30
    }
    
];
var dataArr = [{
        value: [28, 26, 21 ,19, 10, 17],
        name: legendData[0],
        itemStyle: {
            normal: {
                lineStyle: {
                    color: '#4A99FF',
                    // shadowColor: '#4A99FF',
                    // shadowBlur: 10,
                },
                shadowColor: '#4A99FF',
                shadowBlur: 10,
            },
        },
        areaStyle: {
                normal: { // 单项区域填充样式
                    color: {
                        type: 'linear',
                        x: 0, //右
                        y: 0, //下
                        x2: 1, //左
                        y2: 1, //上
                        colorStops: [{
                            offset: 0,
                            color: '#4A99FF'
                        }, {
                            offset: 0.5,
                            color: 'rgba(0,0,0,0)'
                        }, {
                            offset: 1,
                            color: '#4A99FF'
                        }],
                        globalCoord: false
                    },
                    opacity: 1 // 区域透明度
                }
            }
    },
    {
        value: [0, 0, 0, 0, 0, 0],
        name: legendData[1],
        itemStyle: {
            normal: {
                lineStyle: {
                    color: '#4BFFFC',
                    // shadowColor: '#4BFFFC',
                    // shadowBlur: 10,
                },
                shadowColor: '#4BFFFC',
                shadowBlur: 10,
            },
        },
         areaStyle: {
                normal: { // 单项区域填充样式
                    color: {
                        type: 'linear',
                        x: 0, //右
                        y: 0, //下
                        x2: 1, //左
                        y2: 1, //上
                        colorStops: [{
                            offset: 0,
                            color: '#4BFFFC'
                        }, {
                            offset: 0.5,
                            color: 'rgba(0,0,0,0)'
                        }, {
                            offset: 1,
                            color: '#4BFFFC'
                        }],
                        globalCoord: false
                    },
                    opacity: 1 // 区域透明度
                }
            }
    }
];
var colorArr = ['#4A99FF', '#4BFFFC']; //颜色
 const option = {
    // backgroundColor: ,
    title: {
        show: false,
        text: "机体损伤原因分析",
        x: "4%",

        textStyle: {
            color: '#4A99FF',
            fontSize: '22'
        },
        subtextStyle: {
            color: '#90979c',
            fontSize: '16',

        },
    },
    color: colorArr,
    legend: {
        orient:'vertical',
        icon: 'circle', //图例形状
        data: legendData,
        bottom:35,
        right:40,
        itemWidth: 10, // 图例标记的图形宽度。[ default: 25 ]
        itemHeight: 10, // 图例标记的图形高度。[ default: 14 ]
        itemGap: 21, // 图例每项之间的间隔。[ default: 10 ]横向布局时为水平间隔，纵向布局时为纵向间隔。
        textStyle: {
            fontSize: 14,
            color: '#00E4FF',
        },
    },
    radar: {
        // shape: 'circle',
        name: {
            textStyle: {
                color: '#fff',
                fontSize: 16
            },
        },
        indicator: indicator,
        splitArea: { // 坐标轴在 grid 区域中的分隔区域，默认不显示。
            show: true,
            areaStyle: { // 分隔区域的样式设置。
                color: ['rgba(255,255,255,0)', 'rgba(255,255,255,0)'], // 分隔区域颜色。分隔区域会按数组中颜色的顺序依次循环设置颜色。默认是一个深浅的间隔色。
            }
        },
        axisLine: { //指向外圈文本的分隔线样式
            lineStyle: {
                color: '#153269'
            }
        },
        splitLine: {
            lineStyle: {
                color: '#113865', // 分隔线颜色
                width: 1, // 分隔线线宽
            }
        },
    },
    series: [{
        type: 'radar',
        symbolSize: 6,
        // symbol: 'angle',
        data: dataArr
    }]
};

// var list = [
//     [1, 2, 3, 1, 2, 3, 3], 3, [{
//         "name": "工程质量",
//         "max": 3,
//         color: "#F84C0B"
//     }, {
//         "name": "运行管理",
//         "max": 3,
//         color: "#00CF75"
//     }, {
//         "name": "防洪能力复核",
//         "max": 3,
//         color: "#0099FF"
//     }, {
//         "name": "结构安全",
//         "max": 3,
//         color: "#0099FF"
//     }, {
//         "name": "渗流安全",
//         "max": 3,
//         color: "#0099FF"
//     }, {
//         "name": "抗震安全",
//         "max": 3,
//         color: "#0099FF"
//     }, {
//         "name": "金属结构安全",
//         "max": 3,
//         color: "#0099FF"
//     }]
// ];
// var seriesData = list[0];
// var maxValue = list[1];
// var radarData = list[2];
// var background = "#0e2147"; //背景 

// var option = {
//     title: {
//         text: '基础雷达图'
//     },

//     radar: {
//         indicator: radarData,
//         center: ['50%', '50%'],
//         shape: 'polygon',
//         radius: '60%',
//         nameGap: 20,
//         splitNumber: 3,
//         axisLine: {
//             lineStyle: {
//                 color: '#447AB5'
//             }
//         },
//         splitLine: {
//             show: true,
//             lineStyle: {
//                 color: '#447AB5'
//             }
//         },
//         splitArea: {
//             show:true,
//             areaStyle: {
//                 color: ['rgba(68,122,181,0.7)', 'rgba(68,122,181,0.7)']
//             }
//         },
//         axisLabel: {
//             fontSize: 0
//         }
//     },
//     series: [{
//         type: 'radar',
//         symbolSize: 1,

//         lineStyle: {
//             normal: {
//                 color: '#0018ff',
//                 type: 'solid',
//                 width: 0
//             }
//         },
//         data: [{
//             value: seriesData,
//             label: {
//                 show: false
//             },
//             areaStyle: {
//                 color: {
//                     type: 'radial',
//                     x: 0.5,
//                     y: 0.5,
//                     r: 0.66,
//                     colorStops: [{
//                         offset: 0,
//                         color: '#F84C0B' // 0% 处的颜色
//                     }, {
//                         offset: 0.66,
//                         color: '#00CF75' // 100% 处的颜色
//                     }, {
//                         offset: 1,
//                         color: '#0099FF' // 100% 处的颜色
//                     }],
//                     global: false // 缺省为 false
//                 }
//             }
//         }]
//     }]
// };
      // let dataMax = 60;
      // const source = {
      //   data: [43, 10, 28, 35, 50, 19, 13],
      //   indicator: [
      //     { name: '家政服务', max: dataMax},
      //     { name: '助餐服务', max: dataMax},
      //     { name: '助医服务', max: dataMax},
      //     { name: '待办服务', max: dataMax},
      //     { name: '交谈服务', max: dataMax},
      //     { name: '康复服务', max: dataMax},
      //     { name: '助行服务', max: dataMax},
      //   ]
      // }
      // const buildSeries = function(data){
      //   const helper = data.map((item, index) => {
      //     const arr = new Array(data.length);
      //     arr.splice(index, 1, item);
      //     return arr;
      //   })

      //   return [data, ...helper].map((item, index) => {
      //     return {
      //       type: 'radar',
      //       itemStyle: {
      //         color: '#31e586'
      //       },
      //       lineStyle: {
      //         color: index === 0 ? '#31e586' : 'transparent'
      //       },
      //       areaStyle: {
      //         color: index === 0 ? '#31e586' : 'transparent',
      //         opacity: 0.3
      //       },
      //       tooltip: {
      //         show: index === 0 ? false : true,
      //         formatter: function() {
      //           return source.indicator[index - 1].name + '不满意度：' + source.data[index - 1]+'%';
      //         }
      //       },
      //       z: index === 0 ? 1 : 2,
      //       data: [item]
      //     }
      //   })
      // }


      // const option = {
      //   backgroundColor: '#080b30',
      //   tooltip: {},
      //   radar: {
      //     // shape: 'circle',
      //     name: {
      //       textStyle: {
      //         fontSize: 22,
      //         color: ['#d1dbf2'],
      //         padding: [3, 5]
      //       }
      //     },
      //     splitNumber: 4,
      //     splitArea: {
      //       show: true,
      //       areaStyle: {
      //         color: ['rgba(12,62,129,0)','rgba(12,62,129,0.3)','rgba(12,62,129,0)','rgba(12,62,129,0)']
      //       }
      //     },
      //     splitLine: {
      //       lineStyle: {
      //         color: '#0c3e81'
      //       }
      //     },
      //     axisLine: {
      //       lineStyle: {
      //         color: '#0c3e81'
      //       }
      //     },
      //     indicator: source.indicator
      //   },
      //   series: buildSeries(source.data)
      // };
  
  
  

       myChart.setOption(option);
        window.addEventListener("resize", function () {
          myChart.resize();
        });



     }
    },

    //仪表盘
    getRateEchartData() {
      const ratechart = this.$refs.ratechart;
      if (ratechart) {
        const myChart = this.$echarts.init(ratechart);
    var highlight = '#03b7c9';

    var demoData = [
        { name: '发动机', value: 90, unit: '%', pos: ['16.6%', '25%'], range: [0, 100] },
        { name: '电流', value: 32, unit: 'A', pos: ['49.8%', '25%'], range: [0, 60] },
        { name: '功率因数', value: 0.9, pos: ['83%', '25%'], range: [0.1, 1.0], splitNum: 9 },
        { name: '有功功率', value: 6.34, unit: 'kW', pos: ['16.6%', '75%'], range: [0, 50] },
        { name: '有功电能', value: 6.28, unit: 'kWh', pos: ['49.8%', '75%'], range: [0, 50] },
        { name: '电网频率', value: 50, unit: 'Hz', pos: ['83%', '75%'], range: [0, 100] }
    ];

 const option = {
    // backgroundColor: '#222939',
    
    series: (function() {
        var result = [];
        
        demoData.forEach(function(item) {
            result.push(
                // 外围刻度
                {
                    type: 'gauge',
                    center: item.pos,
                    radius: '33.33%',  // 1行3个
                    splitNumber: item.splitNum || 10,
                    min: item.range[0],
                    max: item.range[1],
                    startAngle: 225,
                    endAngle: -45,
                    axisLine: {
                        show: true,
                        lineStyle: {
                            width: 2,
                            shadowBlur: 0,
                            color: [
                                [1, highlight]
                            ]
                        }
                    },
                    axisTick: {
                        show: true,
                        lineStyle: {
                            color: highlight,
                            width: 1
                        },
                        length: -5,
                        splitNumber: 10
                    },
                    splitLine: {
                        show: true,
                        length: -14,
                        lineStyle: {
                            color: highlight,
                        }
                    },
                    axisLabel: {
                        distance: -20,
                        textStyle: {
                            color: highlight,
                            fontSize: '14',
                            fontWeight: 'bold'
                        }
                    },
                    pointer: {
                        show: 0
                    },
                    detail: {
                        show: 0
                    }
                },
                
                // 内侧指针、数值显示
                {
                    name: item.name,
                    type: 'gauge',
                    center: item.pos,
                    radius: '30.33%',
                    startAngle: 225,
                    endAngle: -45,
                    min: item.range[0],
                    max: item.range[1],
                    axisLine: {
                        show: true,
                        lineStyle: {
                            width: 16,
                            color: [
                                [1, 'rgba(255,255,255,.1)']
                            ]
                        }
                    },
                    axisTick: {
                        show: 0,
                    },
                    splitLine: {
                        show: 0,
                    },
                    axisLabel: {
                        show: 0
                    },
                    pointer: {
                        show: true,
                        length: '105%'
                    },
                    detail: {
                        show: true,
                        offsetCenter: [0, '100%'],
                        textStyle: {
                            fontSize: 20,
                            color: '#fff'
                        },
                        formatter: [
                            '{value} ' + (item.unit || ''),
                            '{name|' + item.name + '}'
                        ].join('\n'),
                        rich: {
                            name: {
                                fontSize: 14,
                                lineHeight: 30,
                                color: '#ddd'
                            }
                        }
                    },
                    itemStyle: {
                        normal: {
                            color: highlight,
                        }
                    },
                    data: [{
                        value: item.value
                    }]
                }
            );
        });
        
        return result;
    })()
};
        myChart.setOption(option);
        window.addEventListener("resize", function () {
          myChart.resize();
        });
      }
      this.$on("hook:destroyed", () => {
        window.removeEventListener("resize", function () {
          myChart.resize();
        });
      });
    },
  },
  watch: {},
  created() {},
};
</script>
<style lang="scss" scoped>
// 整体
#main {
  background-image: url("./assest/bg002.png");
  background-size: 1465px 716px; /* 或者可以使用具体的宽高值，例如100px 150px */
  background-repeat: no-repeat;
  background-position: top;
  height: 710px;

  //标题
  .head {
    width: 100%;
    // background-color: #012da6;
    // background-image: url("./header1.png");
    background-size: 100%;
    display: flex;
    justify-content: center;
    .dashboard-title {
      font-size: 20px;
      color: #8ed1fd;
      font-weight: bold;
      height: 30px;
      transform: translateY(6px);
      // padding-top: 5px;

      // color:blue;
    }
  }

  // 小标题
  .title2{
    display: flex;
    justify-content: center;
    .centertitle{
      color: #8ed1fd;
      font-size: 14px;
      transform: translateY(25px);
      .hour{
        font-weight: bold;
        color: red;
        
      }
    }
  }

  .mainbody{
    display: flex;
    justify-content: space-between;

  //左边部分
  .leftbody{
  width: 400px;
    // background-color: paleturquoise;
  //       #canvas {
  // width: 400px;
  // height: 400px;
  //       }
  // 扇形图
  #piechart {
    // background-color: blue;
    transform: translate(50px, -10px);

    // background-color: red;
  }

  // 柱状图
  #barchart{
    background-color: plum;

  }

  //雷达图
  #raderchart{
    // background-color: #fff;
    transform: translate(-40px, 140px);
    padding-left: 30px;
    // margin-top: 5px;
    
  }

  }
    
  //中心部分
  .centerbody{
    flex:1;
    .top{
      display: flex;
      justify-content: center;
    }
    #canvas {
  width: 760px;
  height: 560px;
  transform: translate(10px, -70px);
  }

  .img{
  transform: translate(-140px, 350px);
  }
  }

  .rightbody{
    width: 400px;
    // background-color: red;

  #ratechart{
  transform: translate(-110px, 130px);
    
  }
    
  }


  }




}
</style>



