<template>
  <div class="com-map-gaode">
    <div id="map1"></div>
    <div id="map2"></div>
  </div>
</template>
<script type="text/javascript">
window._AMapSecurityConfig = {
  securityJsCode: "b498c9ac7ed700af7f9b00e0c9c89358",
};
</script>
<script>
import AMapLoader from "@amap/amap-jsapi-loader";

export default {
  data() {
    return {
      map: null,
    };
  },
  methods: {
    initMap() {
      AMapLoader.load({
        key: "ae348621690674d2b123fefaf08ba272", // 申请好的Web端开发者Key，首次调用 load 时必填
        version: "2.0", // 指定要加载的 JSAPI 的版本，缺省时默认为 1.4.15
        plugins: [""], // 需要使用的的插件列表，如比例尺'AMap.Scale'等
      })
        .then((AMap) => {
          // 创建地图1
          this.map = new AMap.Map("map1", {
            //设置地图容器id
            viewMode: "2D", //是否为3D地图模式
            zoom: 10, //初始化地图级别
            // pitch: 75, // 地图俯仰角度，有效范围 0 度- 83 度
            center: [105.602725, 37.076636], //初始化地图中心点位置
            // terrain: true, // 开启地形图
            // layers: [new AMap.TileLayer.Satellite()], //设置图层,可设置成包含一个或多个图层的数组
            // mapStyle: "amap://styles/whitesmoke", //设置地图的显示样式
          });

          // 地图加载完后的事件
          this.map.on("complete", () => {
            console.log("地图加载完了~");
            /** S  地图状态  >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> */

            /** S 设置地图中心点 */
            // 传入经纬度，设置地图中心点
            // const position = new AMap.LngLat(116, 39); // 标准写法
            //  // 简写 const position = [116, 39];
            // this.map.setCenter(position);
            // // 获取地图中心点
            // const currentCenter = this.map.getCenter();
            // console.log("获取地图的中心点", currentCenter);
            /** E 设置地图中心点 */

            /** S 设置地图级别 */
            // 地图缩放级别
            // 设置地图级别，级别为数字。
            // 参数zoom可设范围：[2,20]
            // this.map.setZoom(6);
            // const curZoom = this.map.getZoom();
            // console.log("获取地图级别", curZoom);
            /** E 设置地图级别 */

            /** S 同时设置地图中心和缩放级别 */

            // 同时传入缩放级别和中心点经纬度
            // this.map.setZoomAndCenter(14, [116.205467, 39.907761]);

            /** E 同时设置地图中心和缩放级别 */

            /** S 根据覆盖物范围调整视野 */

            // 创建一条折线覆盖物
            var path = [
              new AMap.LngLat("116.368904", "39.913423"),
              new AMap.LngLat("116.382122", "39.901176"),
              new AMap.LngLat("116.387271", "39.912501"),
              new AMap.LngLat("116.398258", "39.904600"),
            ];
            var polyline = new AMap.Polyline({
              path: path,
              borderWeight: 2, // 线条宽度，默认为 1
              strokeColor: "red", // 线条颜色
              lineJoin: "round", // 折线拐点连接处样式
            });
            this.map.add(polyline);

            // 创建两个点标记
            // var marker1 = new AMap.Marker({
            //   position: new AMap.LngLat(117.39, 39.9), // 经纬度对象，如 new AMap.LngLat(116.39, 39.9); 也可以是经纬度构成的一维数组[116.39, 39.9]
            //   title: "北京",
            // });
            // var marker2 = new AMap.Marker({
            //   position: new AMap.LngLat(118.39, 39.9), // 经纬度对象，如 new AMap.LngLat(116.39, 39.9); 也可以是经纬度构成的一维数组[116.39, 39.9]
            //   title: "北京",
            // });
            // this.map.add(marker1);
            // this.map.add(marker2);

            // 自动适配到合适视野范围
            // 无参数，默认包括所有覆盖物的情况
            // this.map.setFitView();
            // 传入覆盖物数组，仅包括polyline和marker1的情况
            //  this.map.setFitView([polyline, marker1]);

            /** E 根据覆盖物范围调整视野 */

            /** E 地图状态  >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> */

            /** S 地图的交互与事件 >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> */

            this.map.on("click", function (ev) {
              //地图事件是对 Map 底图操作后触发的事件，事件回调中返回 MapsEvent对象。该对象包含触发的对象目标、触发所在经纬度等信息。

              console.log("click 事件 >>>", ev);

              // 触发事件的对象
              var target = ev.target;
              // 触发事件的地理坐标，AMap.LngLat 类型
              var lnglat = ev.lnglat;
              // 触发事件的像素坐标，AMap.Pixel 类型
              var pixel = ev.pixel;
              // 触发事件类型
              var type = ev.type;
            });

            /** E 地图的交互与事件 >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> */

            /** S 覆盖物or图层管理 >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> */
            /** S 添加覆盖物 */
            // 构造点标记
            var marker = new AMap.Marker({
              icon: "https://webapi.amap.com/theme/v1.3/markers/n/mark_b.png",
              position: [116.405467, 39.907761],
            });
            // 构造矢量圆形
            var circle = new AMap.Circle({
              center: new AMap.LngLat("116.403322", "39.920255"), // 圆心位置
              radius: 1000, //半径
              strokeColor: "#F33", //线颜色
              strokeOpacity: 1, //线透明度
              strokeWeight: 3, //线粗细度
              fillColor: "#ee2200", //填充颜色
              fillOpacity: 0.35, //填充透明度
            });

            // 将以上覆盖物添加到地图上
            // 单独将点标记添加到地图上
            this.map.add(marker);
            // add方法可以传入一个覆盖物数组，将点标记和矢量圆同时添加到地图上
            this.map.add([marker, circle]);
            /** E 添加覆盖物 */
            /** S 获取覆盖物 */
            // 获取已经添加的覆盖物
            const allOverlays = this.map.getAllOverlays();
            console.log("allOverlays", allOverlays);
            // 获取已经添加的marker
            const markerOverlays = this.map.getAllOverlays("marker");
            console.log("markerOverlays", allOverlays);
            /** E 获取覆盖物 */
            /** S 移除覆盖物 */

            // 使用remove方法移除覆盖物，参数可以为单个覆盖物对象，也可以是一个包括多个覆盖物的数组
            // 单独移除点标记
            //this.map.remove(markerOverlays);
            // 同时移除点标记和矢量圆形
            // this.map.remove([marker,circle]);

            // // 使用clearMap方法删除所有覆盖物
            // this.map.clearMap();
            /** E 移除覆盖物 */
            /** S 添加图层 */

            // 构造官方卫星、路网图层
            var layer1 = new AMap.TileLayer.Satellite();
            var layer2 = new AMap.TileLayer.RoadNet();
            var layers = [layer1, layer2];
            //添加到地图上
            this.map.add(layers);

            /** E 添加图层 */
            /** S 设置图层 */
            // 构造官方卫星、路网图层
            // var layers = [
            //   new AMap.TileLayer.Satellite(),
            //   new AMap.TileLayer.RoadNet(),
            // ];
            // // 地图上设置图层
            // this.map.setLayers(layers);
            /** E 设置图层 */
            /** S 获取图层 */
            const layersInfo = this.map.getLayers();
            console.log("获取图层", layersInfo);

            /** E 获取图层 */
            /** E 移除图层 */
            // 移除一个图层
            // this.map.remove(layer1);
            /** E 移除图层 */

            /** E 覆盖物or图层管理 >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> */
            /** S 自定义地图 >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> */
            /** S 使用官方地图样式 */
            // this.map.setMapStyle("amap://styles/whitesmoke");

            /** E 使用官方地图样式 */
            /** S 设置地图的显示内容 */
            // this.map.setFeatures("road");  // 单一种类要素显示
            //    this.map.setFeatures(['road','point','building']); // 多个种类要素显示

            /** E 设置地图的显示内容 */

            /** E 自定义地图 >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> */
            /** S 覆盖物 >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> */

            /** S 点标记 */

            //设置图标大小，偏移等属性，比单纯设置 URL 更加灵活。创建方式如下

            var icon1 = new AMap.Icon({
              size: new AMap.Size(40, 50), // 图标尺寸
              image: "//webapi.amap.com/theme/v1.3/images/newpc/way_btn2.png", // Icon的图像
              imageOffset: new AMap.Pixel(0, -60), // 图像相对展示区域的偏移量，适于雪碧图等
              imageSize: new AMap.Size(40, 50), // 根据所设置的大小拉伸或压缩图片
            });
            var markerbj1 = new AMap.Marker({
              position: new AMap.LngLat(116.39, 39.9), // 经纬度对象，也可以是经纬度构成的一维数组[116.39, 39.9]
              offset: new AMap.Pixel(-10, -10),
              icon: icon1, // 添加 Icon 实例
              title: "北京",
              zoom: 13,
            });

            var markerbj2 = new AMap.Marker({
              position: new AMap.LngLat(100.39, 39.9), // 经纬度对象，也可以是经纬度构成的一维数组[116.39, 39.9]
              title: "北京2",
            });
            var markerbj3 = new AMap.Marker({
              position: new AMap.LngLat(90.39, 39.9), // 经纬度对象，也可以是经纬度构成的一维数组[116.39, 39.9]
              title: "北京3",
            });
            var markerbj4 = new AMap.Marker({
              position: new AMap.LngLat(80.39, 39.9), // 经纬度对象，也可以是经纬度构成的一维数组[116.39, 39.9]
              title: "北京4",
            });
            // 导入多个标记点

            // 自定义内容标注
            const content = "<div  >55555</div>";
            var marker = new AMap.Marker({
              icon: "//a.amap.com/jsapi_demos/static/demo-center/marker/marker-bottom-left.png", // 自定义点标记
              position: [116.418481, 39.90833], // 基点位置
              offset: new AMap.Pixel(0, 0), // 设置点标记偏移量
              anchor: "bottom-left", // 设置锚点方位
              content: content,
            });
            this.map.add(marker);

            this.map.add(marker);

            /** E 点标记 */
            /** S LabelMarke */

            // 创建图标对象
            var icon = {
              // 图标类型，现阶段只支持 image 类型
              type: "image",
              // 图片 url
              image:
                "https://a.amap.com/jsapi_demos/static/demo-center/marker/express2.png",
              // 图片尺寸
              size: [64, 30],
              // 图片相对 position 的锚点，默认为 bottom-center
              anchor: "center",
            };

            // 设置文字对象

            var text = {
              // 要展示的文字内容
              content: "中邮速递易",
              // 文字方向，有 icon 时为围绕文字的方向，没有 icon 时，则为相对 position 的位置
              direction: "right",
              // 在 direction 基础上的偏移量
              offset: [-20, -5],
              // 文字样式
              style: {
                // 字体大小
                fontSize: 12,
                // 字体颜色
                fillColor: "#22886f",
                // 描边颜色
                strokeColor: "#fff",
                // 描边宽度
                strokeWidth: 2,
              },
            };

            // 创建 LabelMarker 实例
            var labelMarker = new AMap.LabelMarker({
              name: "标注2", // 此属性非绘制文字内容，仅最为标识使用
              position: [116.466994, 39.984904],
              zIndex: 16,
              // 将第一步创建的 icon 对象传给 icon 属性
              icon: icon,
              // 将第二步创建的 text 对象传给 text 属性
              text: text,
            });

            labelMarker.on("click", function (e) {
              labelMarker.setOpacity(0.5);
            });
            //创建 LabelsLayer
            var labelsLayer = new AMap.LabelsLayer({
              zooms: [3, 20],
              zIndex: 1000,
              // 该层内标注是否避让
              collision: true,
              // 设置 allowCollision：true，可以让标注避让用户的标注
              allowCollision: true,
            });
            //将已创建的 LabelMarker 添加到 LabelsLayer 上

            // 添加一个 labelMarker
            labelsLayer.add(labelMarker);

            // 批量添加 labelMarker
            // labelsLayer.add([labelsMarker1, labelsMarker2]);

            // 将 LabelsLayer 添加到地图实例
            this.map.add(labelsLayer);
            /** E LabelMarke */
            /** S 海量点标记MassMarks */
            // 创建样式对象
            // var styleObject = {
            //   url: "//vdata.amap.com/icons/b18/1/2.png", // 图标地址
            //   size: new AMap.Size(11, 11), // 图标大小
            //   anchor: new AMap.Pixel(5, 5), // 图标显示位置偏移量，基准点为图标左上角
            // };

            // var massMarks = new AMap.MassMarks({
            //   zIndex: 5, // 海量点图层叠加的顺序
            //   zooms: [3, 19], // 在指定地图缩放级别范围内展示海量点图层
            //   style: styleObject, // 设置样式对象
            // });

            // var data = [
            //   {
            //     lnglat: [100.405285, 39.904989], //点标记位置
            //     name: "beijing",
            //     id: 1,
            //   },
            //   {
            //     lnglat: [99.405285, 39.904989], //点标记位置
            //     name: "beijing",
            //     id: 2,
            //   },
            //   {
            //     lnglat: [98.405285, 39.904989], //点标记位置
            //     name: "beijing",
            //     id: 3,
            //   },
            //   {
            //     lnglat: [97.405285, 39.904989], //点标记位置
            //     name: "beijing",
            //     id: 4,
            //   },
            //   {
            //     lnglat: [96.405285, 39.904989], //点标记位置
            //     name: "beijing",
            //     id: 5,
            //   },
            //   {
            //     lnglat: [95.405285, 39.904989], //点标记位置
            //     name: "beijing",
            //     id: 6,
            //   },
            //   {
            //     lnglat: [94.405285, 39.904989], //点标记位置
            //     name: "beijing",
            //     id: 7,
            //   },
            //   {
            //     lnglat: [93.405285, 39.904989], //点标记位置
            //     name: "beijing",
            //     id: 8,
            //   },
            // ];

            // massMarks.setData(data);

            // // 将海量点添加至地图实例
            // massMarks.setMap(this.map);

            /** E 海量点标记MassMarks */
            /** S 矢量图形 */
            /** S 折线Polyline */

            // 折线的节点坐标数组，每个元素为 AMap.LngLat 对象
            var path = [
              new AMap.LngLat(100.368904, 39.913423),
              new AMap.LngLat(116.382122, 39.901176),
              new AMap.LngLat(116.387271, 39.912501),
              new AMap.LngLat(116.398258, 39.9046),
            ];

            // 创建折线实例
            var polyline = new AMap.Polyline({
              path: path,
              borderWeight: 2, // 线条宽度，默认为 1
              strokeColor: "red", // 线条颜色
              lineJoin: "round", // 折线拐点连接处样式
            });

            // 折线的移除
            // 移除一条已创建的折线
            // map.remove(polyline);

            // // 一次性移除多条已创建的折线
            // var polylines = [polyline1, polyline2, polyline3];
            // map.remove(polylines);

            //折线的显示与隐藏
            // 显示一条已创建的 polyline
            // polyline.show();

            // // 隐藏一条已创建的 polyline
            // polyline.hide();

            // 将折线添加至地图实例
            // this.map.add(polyline);

            /** E 折线Polyline */
            /** S 多边形Polygon */
            // 多边形轮廓线的节点坐标数组
            // var path = [
            //   new AMap.LngLat(116.368904, 39.913423),
            //   new AMap.LngLat(116.382122, 39.901176),
            //   new AMap.LngLat(116.387271, 39.912501),
            //   new AMap.LngLat(116.398258, 39.9046),
            // ];

            // var polygon = new AMap.Polygon({
            //   path: path,
            //   fillColor: "#fff", // 多边形填充颜色
            //   borderWeight: 2, // 线条宽度，默认为 1
            //   strokeColor: "red", // 线条颜色
            // });

            // this.map.add(polygon);

            /** E 多边形Polygon */

            /** S  圆形Circle */
            // var circle = new AMap.Circle({
            //   center: new AMap.LngLat(116.39, 39.9), // 圆心位置
            //   radius: 1000, // 圆半径
            //   fillColor: "red", // 圆形填充颜色
            //   strokeColor: "#fff", // 描边颜色
            //   strokeWeight: 2, // 描边宽度
            // });

            // this.map.add(circle);
            /** E  圆形Circle */
            /** S  矩形Rectangle*/

            //         var southWest = new AMap.LngLat(116.356449, 39.859008)
            // var northEast = new AMap.LngLat(116.417901, 39.893797)

            // var bounds = new AMap.Bounds(southWest, northEast)

            // var rectangle = new AMap.Rectangle({
            //     bounds: bounds,
            //     strokeColor:'red',
            //     strokeWeight: 6,
            //     strokeOpacity:0.5,
            //     strokeDasharray: [30,10],
            //     // strokeStyle还支持 solid
            //     strokeStyle: 'dashed',
            //     fillColor:'blue',
            //     fillOpacity:0.5,
            //     cursor:'pointer',
            //     zIndex:50,
            // })

            // this.map.add(rectangle)
            /** E  矩形Rectangle*/
            /** S  椭圆形覆盖物 Ellipse*/
            var ellipse = new AMap.Ellipse({
              center: [116.433322, 39.900255],
              radius: [2000, 1000], //半径
              borderWeight: 3,
              strokeColor: "#FF33FF",
              strokeOpacity: 1,
              strokeWeight: 6,
              strokeOpacity: 0.2,
              fillOpacity: 0.4,
              // 线样式还支持 'dashed'
              strokeStyle: "dashed",
              strokeDasharray: [10, 10],
              fillColor: "#1791fc",
              zIndex: 50,
            });

            this.map.add(ellipse);
            /** E  椭圆形覆盖物 Ellipse*/

            /** E 矢量图形 */

            this.map.setFitView([ellipse]);

            /** E 覆盖物 >>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>> */
          });

          // 3 秒后销毁地图
          //  setTimeout(() => {
          //     this.map.destroy()
          //     alert('地图被销毁了~')
          //  }, 3000);

          /** S 多地图创建  */

          //     const map2 = new AMap.Map("map2", {
          //     //设置地图容器id
          //     viewMode: "2D", //是否为3D地图模式
          //     zoom: 5, //初始化地图级别
          //     center: [105.602725, 37.076636], //初始化地图中心点位置

          //     mapStyle: "amap://styles/whitesmoke", //设置地图的显示样式
          //   });

          /** E 多地图创建  */
        })
        .catch((e) => {
          console.log(e);
        });
    },
  },
  mounted() {
    //DOM初始化完成进行地图初始化
    this.initMap();
  },
};
</script>

<style  scoped>
#map1 {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
}
#map2 {
  height: 200px;
  width: 200px;
}
</style>