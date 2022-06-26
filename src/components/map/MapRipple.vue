<!--
 * @Author: shencanlong
 * @Date: 2022-06-26 23:23:37
 * @LastEditors: git config user.name && git config user.email
 * @LastEditTime: 2022-06-27 00:56:09
 * @FilePath: \learn-gaode-map\src\components\map\MapRipple.vue
 * @Description:  增加涟漪动画的 marker
 * 
-->


<template>
  <div class="map-ripple">
    <!-- <map-gao-de /> -->
    <div id="ripple"></div>
    <span class="alert1" id="alert1"></span>
    <span class="alert3" id="alert3"></span>
  </div>
</template>


<script>
import AMapLoader from "@amap/amap-jsapi-loader";
export default {
  name: "MapRipple",
  components: {
    // MapGaoDe
  },
  data: () => {
    return {
      map: null,
    };
  },
  /**
   * @Author: canlong.shen
   * @description:
   * @param {*}
   * @return {*}
   */
  methods: {
    initMap() {
      AMapLoader.load({
        key: "ae348621690674d2b123fefaf08ba272", // 申请好的Web端开发者Key，首次调用 load 时必填
        version: "2.0", // 指定要加载的 JSAPI 的版本，缺省时默认为 1.4.15
        plugins: [""], // 需要使用的的插件列表，如比例尺'AMap.Scale'等
      }).then((AMap) => {
        // 创建地图
        this.map = new AMap.Map("ripple", {
          //设置地图容器
          resizeEnable: true,
          viewMode: "2D",
          zoom: 10,
          center: [121.611511, 29.914939],
        });
        this.map.on("complete", () => {
          console.log("地图加载完了~");
        });

        var marker1 = new AMap.Marker({
          //icon: "https://webapi.amap.com/theme/v1.3/markers/n/mark_b.png",
          position: [121.611554, 29.914888],
          draggable: true,
          content: document.getElementById("alert1"),
          animation: "AMAP_ANIMATION_DROP",
        });

        // 构造点标记
        var marker2 = new AMap.Marker({
          //icon: "https://webapi.amap.com/theme/v1.3/markers/n/mark_b.png",
          position: [121.611554, 29.914888],
          draggable: true,
          content: '<span class="alert2"></span>',
          animation: "AMAP_ANIMATION_DROP",
        });
        var marker3 = new AMap.Marker({
          //icon: "https://webapi.amap.com/theme/v1.3/markers/n/mark_b.png",
          position: [121.611554, 29.914888],
          draggable: true,
          content: document.getElementById("alert3"),
          //   offset: new AMap.Pixel(-9, -9),
          anchor: "center",
        });

        this.map.add([marker3]);

        this.map.setFitView(marker3);

        //鼠标点击marker弹出自定义的信息窗体
        marker1.on("click", this.openInfo(AMap));
        marker2.on("click", this.openInfo(AMap));
      });
    },
    //在指定位置打开信息窗体
    openInfo(AMap) {
      //构建信息窗体中显示的内容
      var info = [];
      info.push(
        '<div class=\'input-card content-window-card\'><div><img style="float:left;" src=" https://webapi.amap.com/images/autonavi.png "/></div> '
      );
      info.push(
        '<div style="padding:7px 0px 0px 0px;"><h4>漏刻有时地图可视化</h4>'
      );
      info.push(
        "<p class='input-item'>地址:漏刻有时地图可视化</p></div></div>"
      );

      let infoWindow = new AMap.InfoWindow({
        content: info.join(""), //使用默认信息窗体框样式，显示信息内容
      });

      infoWindow.open(this.map, this.map.getCenter());
    },
  },
  mounted() {
    this.initMap();
  },
};
</script>
<style>
#ripple {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}

.alert1 {
  display: inline-block;
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: #ff0000;
  margin: auto 5px;
  -webkit-animation: animation1 1s infinite;
}

.alert2 {
  display: inline-block;
  width: 16px;
  height: 16px;
  border-radius: 50%;
  background: #00e921;
  margin: auto 5px;
  -webkit-animation: animation2 1s infinite;
}

/*高德地图标注动画*/
@keyframes animation1 {
  0% {
    -moz-box-shadow: 0px 0px 10px 5px #6200ff;
    box-shadow: 0px 0px 10px 5px #e100ff;
  }
  50% {
    -moz-box-shadow: 0px 0px 20px 5px #ff0000;
    box-shadow: 0px 0px 20px 5px #ff0000;
  }
  100% {
    -moz-box-shadow: 0px 0px 10px 5px #ff0000;
    box-shadow: 0px 0px 10px 5px #ff0000;
  }
  150% {
    -moz-box-shadow: 0px 0px 50px 5px #ff0000;
    box-shadow: 0px 0px 50px 5px #ff0000;
  }
}

@keyframes animation2 {
  0% {
    -moz-box-shadow: 0px 0px 10px 5px #00e921;
    box-shadow: 0px 0px 10px 5px #00e921;
  }
  50% {
    -moz-box-shadow: 0px 0px 20px 5px #00e921;
    box-shadow: 0px 0px 20px 5px #00e921;
  }
  100% {
    -moz-box-shadow: 0px 0px 10px 5px #00e921;
    box-shadow: 0px 0px 10px 5px #00e921;
  }
  150% {
    -moz-box-shadow: 0px 0px 50px 5px #00e921;
    box-shadow: 0px 0px 50px 5px #00e921;
  }
}

@keyframes GreenDot {
  0% {
    transform: scale(0.5);
    opacity: 1;
  }
  30% {
    opacity: 0.5;
  }
  100% {
    transform: scale(1.4);
    opacity: 0;
  }
}

.alert3 {
  width: 10px;
  height: 10px;
  background-color: #f8636b;
  border-radius: 50%;
  display: inline-block;
  position: relative;
}

.alert3::before {
  content: "";
  width: 180px;
  height: 180px;
  position: absolute;
  left: 50%;
  top: 50%;
  margin-left: -90px;
  margin-top: -90px;
  border-radius: 50%;
  animation: GreenDot 1s ease-out 0s infinite;
  background-color: rgba(238, 28, 28, 0.5);
}
</style>

