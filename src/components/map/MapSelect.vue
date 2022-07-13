<!--
 * @Author: shencanlong
 * @Date: 2022-06-26 23:23:37
 * @LastEditors: git config user.name && git config user.email
 * @LastEditTime: 2022-07-14 01:13:29
 * @FilePath: \learn-gaode-map\src\components\map\MapSelect.vue
 * @Description:  增加涟漪动画的 marker
 * 
-->


<template>
  <div class="long-map-select">
    <div class="map_select">
      <label>请输入关键字：</label>
      <input id="tipinput" />
    </div>

    <div class="map_container" id="map_select"></div>
  </div>
</template>


<script>
import AMapLoader from "@amap/amap-jsapi-loader";
export default {
  name: "MapSelect",
  components: {
    // MapGaoDe
  },
  data: () => {
    return {
      map: null,
      locationMarker: null,
      curCityPosition: null,
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
        plugins: [
          "AMap.Geocoder",
          "AMap.Geolocation",
          "AMap.PlaceSearch",
          "AMap.AutoComplete",
        ], // 需要使用的的插件列表，如比例尺'AMap.Scale'等
      }).then((AMap) => {
        // 创建地图
        this.map = new AMap.Map("map_select", {
          //设置地图容器
          resizeEnable: true,
          viewMode: "2D",
          zoom: 10,
        });

        // 定位到当前城市 >>>

        let geolocation = new AMap.Geolocation({
          // 是否使用高精度定位，默认：true
          enableHighAccuracy: true,
          // 设置定位超时时间，默认：无穷大
          timeout: 10000,
          // 定位按钮的停靠位置的偏移量
          offset: [10, 20],
          //  定位成功后调整地图视野范围使定位位置及精度范围视野内可见，默认：false
          zoomToAccuracy: true,
          //  定位按钮的排放位置,  RB表示右下
          position: "RB",
        });

        geolocation.getCurrentPosition((status, result) => {
          if (status == "complete") {
            console.log("getCurrentPosition", result);
            const { position } = result;
            this.curCityPosition = position;
            // 简写 var position = [116, 39];
            // 定位到当前城市
            this.map.setCenter(position);
          } else {
            // onError(result)
          }
        });

        // 定位到当前城市  <<<
        //输入提示

        // 输入提示后查询 >>>
        let autoOptions = {
          input: "tipinput",
        };
        let auto = new AMap.AutoComplete(autoOptions);
        let placeSearch = new AMap.PlaceSearch({
          map: this.map,
        }); //构造地点查询类
        const select = (e) => {
          console.log("select", e);
          console.log("poi.location", e.poi.location);

          placeSearch.setCity(e.poi.adcode);
          placeSearch.search(e.poi.name); //关键字查询查询
        };
        auto.on("select", select); //注册监听，当选中某条记录时会触发

        // 输入提示后查询 <<<

        this.map.on("complete", () => {
          console.log("地图加载完了~");

          // 正向地理编码 逆向地理编码
          let geocoder = new AMap.Geocoder({
            city: "全国", // 城市，地理编码时，设置地址描述所在城市 可选值：城市名（中文或中文全拼）、citycode、adcode默认值：“全国”
            radius: 1000, //逆地理编码时，以给定坐标为中心点，单位：米取值范围：0 - 3000 默认值：1000
            batch: true, // 是否批量查询batch 设置为 false 时，只返回第一条记录
            extensions: "base", //默认值：base，返回基本地址信息 取值为：all，返回地址信息及附近poi、道路、道路交叉口等信息
          });

          // 信息窗体 >>>

          // 实现自定义窗体内容，返回拼接后的字符串
          const createInfoWindow = (lng, lat, formattedAddress) => {
            // 内容拼接 ...
            return `<h1>${formattedAddress}</h1> <h1>${lng}</h1><h1>${lat}</h1>`;
          };

          // 创建 infoWindow 实例
          let infoWindow = new AMap.InfoWindow({
            isCustom: true, //使用自定义窗体
            offset: new AMap.Pixel(16, -50),
          });

          // 信息窗体 <<<

          // 鼠标点击事件
          this.map.on("click", (e) => {
            console.log("e.LngLat>>>", e.lnglat);
            const { lng, lat } = e.lnglat;
            // 删除原理地图的marker
            let locationMarker = this.locationMarker;
            if (locationMarker) {
              this.map.remove(locationMarker);
            }
            // 创建marker
            locationMarker = new AMap.Marker({
              icon: "https://webapi.amap.com/theme/v1.3/markers/n/mark_b.png",
              position: [lng, lat],
              offset: new AMap.Pixel(0, 0), // 设置点标记偏移量
              anchor: "bottom-center", // 设置锚点方位
              draggable: true,
            });
            this.locationMarker = locationMarker;

            // 坐标获取地理位置信息
            geocoder.getAddress([lng, lat], (status, result) => {
              if (status === "complete" && result.info === "OK") {
                // result为对应的地理位置详细信息
                console.log("getAddress", result);
                const {
                  regeocode: { formattedAddress },
                } = result;

                // 设置信息窗体内容
                infoWindow.setContent(
                  createInfoWindow(lng, lat, formattedAddress)
                );
                // marker 放到地图上
                this.map.add([locationMarker]);
                // 打开信息窗体
                infoWindow.open(this.map, [lng, lat]);
              }
            });
          });

          geocoder.getLocation("博思高科技有限公司", function (status, result) {
            if (status === "complete" && result.info === "OK") {
              // result中对应详细地理坐标信息
              console.log("result", result);
            }
          });
        });
      });
    },
  },
  mounted() {
    this.$nextTick(() => {
      this.initMap();
    });
  },
};
</script>
<style  scoped>
.map_container {
  position: fixed;
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
  background-color: pink;
}
.map_select {
  position: relative;
  top: 0;
  z-index: 2;
}
</style>

