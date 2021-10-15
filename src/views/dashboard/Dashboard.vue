<template>
  <div class="bg">
    <div class="conic"></div>
    <div class="top">
      <div class="version">
        <div class="buttom">用户数 3,510,254</div>
        <div class="buttom">版本号: v1.0</div>
      </div>
      <div class="title">伊克塞兽药可视化大数据</div>
      <dv-decoration-5 :color="['#61d6f7']" />
      <div class="date">
        <div class="nowtime">{{ nowDate }}</div>
        <div class="buttom">透视</div>
        <div class="buttom">对比</div>
        <div class="buttom">设置</div>
      </div>
    </div>
    <div class="middle">
      <div class="left">
        <dv-border-box-9 :color="['#61d6f7', '#2798dc']"></dv-border-box-9>
      </div>
      <div id="main" class="center"></div>
      <div class="right">
        <dv-border-box-9 :color="['#61d6f7', '#2798dc']">
          <dv-active-ring-chart :config="config" style="width: 200px; height: 200px" />
        </dv-border-box-9>
      </div>
    </div>
    <div class="bottom">
      <div class="bottom_left">
        <dv-border-box-9 :color="['#61d6f7', '#2798dc']"></dv-border-box-9>
      </div>
      <div class="bottom_center">
        <dv-border-box-9 :color="['#61d6f7', '#2798dc']"></dv-border-box-9>
      </div>
      <div class="bottom_right">
        <dv-border-box-9 :color="['#61d6f7', '#2798dc']"></dv-border-box-9>
      </div>
    </div>
  </div>
</template>

<script>
import * as echarts from 'echarts'
import map from '@/assets/map/innermongol.json'
export default {
  name: 'Echarts',
  data () {
    return {
      clickAreaName: '',
      dongbu: ['呼伦贝尔市', '兴安盟', '通辽市', '赤峰市'],
      zhongbu: ['锡林郭勒盟', '乌兰察布市', '包头市', '呼和浩特市'],
      xibu: ['巴彦淖尔市', '鄂尔多斯市', '乌海市', '阿拉善盟'],
      san: {
            type: 'scatter',
            coordinateSystem: 'geo',
            data: [[117.59434743563688, 48.8656393394309], [118.20594811991872, 48.51486835873985], [119.78891459688349, 49.1624455538618], [120.54442132452576, 49.83700513211383], [120.54442132452576, 50.565529476626025], [121.51578711720869, 46.680066305894314], [121.94750524729, 47.00385490345529], [122.4511763990515, 46.787995838414645], [122.55910593157184, 46.35627770833334], [117.01872326219514, 45.223017616869924], [115.93942793699189, 44.737334720528466], [114.50036750338755, 44.08975752540651], [112.48568289634149, 44.22466944105692], [114.6802500575881, 42.76762075203253], [112.84544800474256, 42.68667360264229], [110.47099828929541, 42.01211402439025], [111.37041106029812, 40.71695963414635], [113.09728358062333, 40.689977251016266], [108.20447810636858, 41.823237342479686], [105.07452166327916, 40.55506533536586], [101.44089406842821, 41.39151921239838], [105.07452166327916, 38.612333750000005], [108.24045461720868, 38.66629851626017], [121.91152873644988, 43.55010986280489], [119.14133740176153, 42.983479817073174]],
            z: 2,
            symbolSize: [11, 14],
            itemStyle: {
              normal: {
                  color: 'white',
                  shadowBlur: 2,
                  shadowColor: 'rgba(0, 0, 0, 0.62)',
                  shadowOffsetX: 2,
                  shadowOffsetY: 2
              },
              emphasis: {
                  shadowColor: 'rgba(255, 255, 255, 10)',
                  shadowBlur: 5,
                  shadowOffsetX: 0,
                  shadowOffsetY: 0
              }
            },
            symbol: 'path://M513.024 1024h-1.024c-17.92 0-34.816-7.168-47.104-20.48-9.728-10.24-97.28-102.912-184.832-219.648C162.304 625.664 102.4 499.2 102.4 409.088 102.4 183.296 286.208 0 512 0s409.6 183.296 409.6 409.088c0 54.784-20.992 121.856-62.976 199.68-39.936 74.752-100.352 161.792-179.712 258.048l-0.512 0.512-117.76 134.144c-11.776 14.336-29.184 22.528-47.616 22.528z m-1.024-423.936c105.984 0 191.488-86.016 191.488-191.488S617.984 217.6 512 217.6 320 303.104 320 409.088s86.016 190.976 192 190.976z'
          },
      tooltipData: [],
      Initoption: {
        tooltip: {
          show: false
        },
        geo: {
          map: 'map',
          roam: false, // 一定要关闭拖拽
          zoom: 1.23,
          center: [112, 45.25], // 调整地图位置
          label: {
            normal: {
              show: false, // 关闭省份名展示
              fontSize: '10',
              color: 'rgba(0,0,0,0.7)'
            },
            emphasis: {
              show: false
            }
          },
          itemStyle: {
            normal: {
              areaColor: '#2897dc',
              borderColor: '#111f46',
              borderWidth: 1, // 设置外层边框
              shadowBlur: 5,
              shadowOffsetY: 8,
              shadowOffsetX: 0,
              shadowColor: '#01012a'
            },
            emphasis: {
              areaColor: '#09355a',
              shadowOffsetX: 0,
              shadowOffsetY: 0,
              shadowBlur: 5,
              borderWidth: 0,
              shadowColor: 'rgba(0, 0, 0, 0.5)'
            }
          }
        },
        series: [
          {
            type: 'map',
            map: 'map',
            roam: false,
            zoom: 1.23,
            center: [112, 45.25],
            // geoIndex: 1,
            // aspectScale: 0.75, //长宽比
            showLegendSymbol: false, // 存在legend时显示
            selectedMode: false, // 关闭点击事件
            label: {
              normal: {
                show: false
              },
              emphasis: {
                show: false,
                textStyle: {
                  color: '#fff'
                }
              }
            },
            itemStyle: {
              normal: {
                areaColor: '#2897dc',
                borderColor: '#111f46',
                borderWidth: 0.5,
                label: {
                    show: true,
                    formatter: '{b} : {c} ({d}%)'
                  },
                  labelLine: { show: true }
              },
              emphasis: {
                areaColor: '#a9ddf2',
                shadowOffsetX: 0,
                shadowOffsetY: 0,
                shadowBlur: 5,
                borderWidth: 0,
                shadowColor: 'rgba(0, 0, 0, 0.5)'
              }
            }
          }
        ] },
      option: {
        tooltip: {
          show: false
        },
        geo: {
          map: 'map',
          roam: false, // 一定要关闭拖拽
          zoom: 1.23,
          center: [112, 45.25], // 调整地图位置
          label: {
            normal: {
              show: false, // 关闭省份名展示
              fontSize: '10',
              color: 'rgba(0,0,0,0.7)'
            },
            emphasis: {
              show: false
            }
          },
          itemStyle: {
            normal: {
              areaColor: '#2897dc',
              borderColor: '#111f46',
              borderWidth: 1, // 设置外层边框
              shadowBlur: 5,
              shadowOffsetY: 8,
              shadowOffsetX: 0,
              shadowColor: '#01012a'
            },
            emphasis: {
              areaColor: '#09355a',
              shadowOffsetX: 0,
              shadowOffsetY: 0,
              shadowBlur: 5,
              borderWidth: 0,
              shadowColor: 'rgba(0, 0, 0, 0.5)'
            }
          }
        },
        series: [
          {
            type: 'map',
            map: 'map',
            roam: false,
            zoom: 1.23,
            center: [112, 45.25],
            // geoIndex: 1,
            // aspectScale: 0.75, //长宽比
            showLegendSymbol: false, // 存在legend时显示
            selectedMode: false, // 关闭点击事件
            label: {
              normal: {
                show: false
              },
              emphasis: {
                show: false,
                textStyle: {
                  color: '#fff'
                }
              }
            },
            itemStyle: {
              normal: {
                areaColor: '#2897dc',
                borderColor: '#111f46',
                borderWidth: 0.5,
                label: {
                    show: true,
                    formatter: '{b} : {c} ({d}%)'
                  },
                  labelLine: { show: true }
              },
              emphasis: {
                areaColor: '#a9ddf2',
                shadowOffsetX: 0,
                shadowOffsetY: 0,
                shadowBlur: 5,
                borderWidth: 0,
                shadowColor: 'rgba(0, 0, 0, 0.5)'
              }
            }
          }
        ] },
      config: {
        data: [
          {
            name: '周口',
            value: 55
          },
          {
            name: '南阳',
            value: 120
          },
          {
            name: '西峡',
            value: 78
          },
          {
            name: '驻马店',
            value: 66
          },
          {
            name: '新乡',
            value: 80
          }
        ]
      },
      nowDate: '' // 当前日期
    }
  },
  mounted () {
    this.currentTime()
    echarts.registerMap('map', map)
    this.echartsInit()
  },
  methods: {
    currentTime () {
      setInterval(this.formatDate, 500)
    },
    formatDate () {
      const date = new Date()
      const year = date.getFullYear() // 年
      const month = date.getMonth() + 1 // 月
      const day = date.getDate() // 日
      // let week = date.getDay(); // 星期
      // let weekArr = [ "星期日", "星期一", "星期二", "星期三", "星期四", "星期五", "星期六" ];
      let hour = date.getHours() // 时
      hour = hour < 10 ? '0' + hour : hour // 如果只有一位，则前面补零
      let minute = date.getMinutes() // 分
      minute = minute < 10 ? '0' + minute : minute // 如果只有一位，则前面补零
      let second = date.getSeconds() // 秒
      second = second < 10 ? '0' + second : second // 如果只有一位，则前面补零
      this.nowDate = `${year}年${month}月${day}日 ${hour}:${minute}:${second}`
    },
    echartsInit () {
      const myecharts = echarts.init(document.getElementById('main'))
      this.option.series.push(this.san)
      this.Initoption.series.push(this.san)
      const option = this.option
      // 区域弹出数据
      myecharts.on('click', (data) => {
        // 点击获取经度纬度
        // console.log(
        //   myecharts.convertFromPixel('geo', [
        //     data.event.offsetX,
        //     data.event.offsetY
        //   ])
        // )
        if (data.name) {
        const op = this.Initoption
        myecharts.setOption(op, true)
        if (this.dongbu.includes(data.name)) {
          this.clickAreaName = data.name
          this.tooltipData = [
              { coords: [[120.383983, 48.644405], [105.670801, 48.318311]] }
            ]
          } else if (this.zhongbu.includes(data.name)) {
            this.clickAreaName = data.name
            this.tooltipData = [
              { coords: [[116.090996, 43.944018], [105.670801, 48.318311]] }
            ]
          } else if (this.xibu.includes(data.name)) {
            this.clickAreaName = data.name
            this.tooltipData = [
              { coords: [[103.314352, 40.550928], [105.670801, 46.318311]] }
            ]
          }
        console.log('option', data)
        option.series.splice(2, 1)
        option.series.push({
            type: 'lines',
            z: 3,
            coordinateSystem: 'geo',
            // symbolSize: [10, 0], // 只保留地图端标记
            opacity: 1,
            // polyline: true, // 多点连线
            data: this.tooltipData,
            label: {
              show: true,
              // position: 'end',
              formatter: function (params) {
                // 文本提示框
                return (
                  '{value| 锡林浩特店}\n{value| 200/万}'
                )
              },
              backgroundColor: '#eee',
              borderColor: '#FF0033',
              borderWidth: 1,
              align: 'center',
              width: 250,
              rich: {
                // 标题样式
                title: {
                  align: 'center',
                  lineHeight: 17,
                  fontSize: 12,
                  color: '#fff',
                  backgroundColor: '#FF0033',
                  width: 250,
                  height: 125
                },
                value: {
                  // 内容样式
                  height: 75,
                  width: 250,
                  color: '#FF0033',
                  backgroundColor: '#fff'
                }
              }
            },
            lineStyle: {
              // 视觉引导线属性
              type: 'solid',
              opacity: 1,
              color: '#fff' // 引导线颜色
            }
          })
        myecharts.setOption(option)
        if (this.dongbu.includes(data.name)) {
          myecharts.dispatchAction({
            seriesIndex: 0,
            type: 'highlight',
            dataIndex: [3, 4, 6, 9]
          })
        } else if (this.zhongbu.includes(data.name)) {
            myecharts.dispatchAction({
            seriesIndex: 0,
            type: 'highlight',
            dataIndex: [0, 1, 8, 10]
          })
        } else if (this.xibu.includes(data.name)) {
            myecharts.dispatchAction({
            seriesIndex: 0,
            type: 'highlight',
            dataIndex: [2, 5, 7, 11]
          })
        }
        }
      })
      // 区域高亮
      myecharts.on('mouseover', (data) => {
        // 东部区
        if (this.dongbu.includes(data.name)) {
          myecharts.dispatchAction({
            seriesIndex: 0,
            type: 'highlight',
            dataIndex: [3, 4, 6, 9]
          })
        // 中部区
        } else if (this.zhongbu.includes(data.name)) {
            myecharts.dispatchAction({
            seriesIndex: 0,
            type: 'highlight',
            dataIndex: [0, 1, 8, 10] })
          // 西部区
          } else if (this.xibu.includes(data.name)) {
          myecharts.dispatchAction({
            seriesIndex: 0,
            type: 'highlight',
            dataIndex: [2, 5, 7, 11]
          })
          }
      })
      myecharts.on('mouseout', (data) => {
        if (myecharts.getOption().series.length > 2) {
          if (this.dongbu.includes(this.clickAreaName)) {
            myecharts.dispatchAction({
            type: 'downplay',
            dataIndex: [0, 1, 8, 10, 2, 5, 7, 11]
             })
          } else if (this.zhongbu.includes(this.clickAreaName)) {
            myecharts.dispatchAction({
            type: 'downplay',
            dataIndex: [3, 4, 6, 9, 2, 5, 7, 11]
             })
            } else if (this.xibu.includes(this.clickAreaName)) {
            myecharts.dispatchAction({
            type: 'downplay',
            dataIndex: [3, 4, 6, 9, 0, 1, 8, 10]
             })
            }
        } else {
        myecharts.dispatchAction({
          type: 'downplay',
          seriesIndex: 0
        })
}
      })
      myecharts.setOption(this.option)
    }
  }
}
</script>
<style scoped>
.bg {
  background: #09355a;
}
.title {
  width: 60%;
  font-size: xx-large;
  color: #58aadc;
  letter-spacing: 7px;
  text-align: center;
  padding-top: 10px;
  position: relative;
}
.dv-decoration-5 {
  position: absolute;
  width: 1000px;
  height: 60px;
  left: 32.5em;
  top: 25px;
}
.version,
.date {
  display: flex;
  width: 20%;
  font-size: xx-large;
  color: white;
}
.top {
  /* background:blue; */
  width: 100%;
  height: 80px;
  display: flex;
}
.middle {
  display: flex;
  width: 100%;
  height: 600px;
}
.left,
.right {
  /* background:green; */
  width: 20%;
  height: 600px;
}
.center {
  /* background:red; */
  width: 60%;
  height: 600px;
}
.bottom {
  /* background:gray; */
  display: flex;
  width: 100%;
  height: 400px;
  margin-top: 20px;
}
.bottom_left,
.bottom_right {
  /* background:blue; */
  width: 20%;
  height: 400px;
}
.bottom_center {
  /* background:white; */
  width: 60%;
  height: 400px;
}
.dv-border-box-9 {
  width: 95%;
  height: 95%;
}
.left,
.right,
.bottom_left,
.bottom_right,
.bottom_center {
  display: flex;
  justify-content: center;
}
.nowtime {
  margin-left: 13px;
  margin-top: 20px;
  font-size: 17px;
  color: #46b9eb;
  font-weight: 400;
}
.buttom {
  font-size: 17px;
  color: #46b9eb;
  font-weight: 400;
  margin-top: 20px;
  margin-left: 22px;
}
</style>
