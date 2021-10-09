<template>
  <div class="bg">
    <div class="conic"></div>
    <div class="top">
      <div class="version">
        <div class="buttom">用户数 3,510,254</div>
        <div class="buttom">版本号: v1.0</div>
      </div>
      <div class="title">伊克塞兽药可视化大数据</div>
      <dv-decoration-5 :color="['#61d6f7']"/>
      <div class="date">
        <div class="nowtime">{{ nowDate }}</div>
        <div class="buttom">透视</div>
        <div class="buttom">对比</div>
        <div class="buttom">设置</div>
      </div>
    </div>
    <div class="middle">
      <div class="left">
        <dv-border-box-9 :color="['#61d6f7', '#2798dc']" ></dv-border-box-9>
      </div>
      <div id="main" class="center"></div>
      <div class="right">
        <dv-border-box-9 :color="['#61d6f7', '#2798dc']">
          <dv-active-ring-chart :config="config" style="width:200px;height:200px" />
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
  ] },
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
        // console.log('test', this.document.getElementById('main'))
        echarts.init(document.getElementById('main')).setOption({
          tooltip: {
          show: true
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
                borderWidth: 0.5
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
        ]
        })
      }
    }
  }
</script>
<style scoped>
.bg {
    background: #09355a;
}
.title {
    width:60%;
    font-size: xx-large;
    color: #58aadc;
    letter-spacing: 7px;
    text-align:center;
    padding-top:10px;
    position: relative;
}
.dv-decoration-5 {
    position: absolute;
    width: 1000px;
    height: 60px;
    left: 32.5em;
    top: 25px;
}
.version, .date {
    display: flex;
    width:20%;
    font-size: xx-large;
    color: white;
}
.top {
    /* background:blue; */
    width:100%;
    height:80px;
    display: flex;
}
.middle {
    display:flex;
    width:100%;
    height:600px
}
.left, .right {
    /* background:green; */
    width:20%;
    height:600px
}
.center {
    /* background:red; */
    width:60%;
    height:600px;
}
.bottom {
    /* background:gray; */
    display:flex;
    width:100%;
    height:400px;
    margin-top: 20px;
}
.bottom_left, .bottom_right {
    /* background:blue; */
    width:20%;
    height:400px
}
.bottom_center {
    /* background:white; */
    width:60%;
    height:400px
}
.dv-border-box-9 {
    width: 95%;
    height: 95%;
}
.left,.right,.bottom_left,.bottom_right,.bottom_center {
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
