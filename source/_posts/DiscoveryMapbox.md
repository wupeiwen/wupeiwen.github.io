---
title: 探索 Mapbox GL JS
date: 2018/7/10 9:20:00
tags:
- 数据可视化 
- mapboxgl
- 地理信息可视化
categories: 数据可视化
---

Mapbox是移动和Web应用程序的位置数据平台。
Mapbox GL JS是一个JavaScript库，它使用WebGL从矢量切片和Mapbox样式来渲染交互式地图。 <!-- more -->

------
### **快速开始**
从mapbox 官方调用  
在HTML文件的`<head>`中引入JavaScript和CSS文件  
```
<script src='https://api.tiles.mapbox.com/mapbox-gl-js/v0.46.0/mapbox-gl.js'></script>
<link href='https://api.tiles.mapbox.com/mapbox-gl-js/v0.46.0/mapbox-gl.css' rel='stylesheet' />
```
在HTML文件的`<body>`写入以下代码
```
<div id='map' style='width: 400px; height: 300px;'></div>
<script>
    mapboxgl.accessToken = '访问密钥';
    var map = new mapboxgl.Map({
      container: 'map',
      style: 'mapbox://styles/mapbox/streets-v9',
      center: [131, 30],
      zoom: 9,
      bearing: 0,
      pitch: 0
    });
</script>
```
在浏览器中打开HTML页面，可以看到加载了streets-v9样式的地图

![地图效果](http://7xu37n.com1.z0.glb.clouddn.com/StyleDarkV9.png)

------

### **配置项说明**
1. accessToken<sup>1</sup>:[访问令牌](https://www.mapbox.com/account/access-tokens)，获取数据必备

2. container<sup>2</sup>:Mapbox GL JS将在其中呈现地图或元素字符串的HTML元素  id 。指定的元素必须没有子元素。

3. style<sup>3</sup>:地图的Mapbox样式。这必须是符合Mapbox样式规范中描述的模式的JSON对象 ，或者是此类JSON的URL。要加载从Mapbox API的样式，你可以使用以下格式的URL mapbox://styles/:owner/:style，在那里:owner你Mapbox帐户名和:style是样式ID。或者，可以使用以下 预定义的Mapbox样式之一：
```
  mapbox://styles/mapbox/streets-v10
  mapbox://styles/mapbox/outdoors-v10
  mapbox://styles/mapbox/light-v9
  mapbox://styles/mapbox/dark-v9
  mapbox://styles/mapbox/satellite-v9
  mapbox://styles/mapbox/satellite-streets-v10
  mapbox://styles/mapbox/navigation-preview-day-v2
  mapbox://styles/mapbox/navigation-preview-night-v2
  mapbox://styles/mapbox/navigation-guidance-day-v2
  mapbox://styles/mapbox/navigation-guidance-night-v2
```

![预定义样式效果图](http://7xu37n.com1.z0.glb.clouddn.com/defaultMapStyle.png)
从左到右依次，Light、Dark、Streets、Outdoors、Satellite Streets、Navigation

4. center<sup>4</sup>:地图的初始地理中心点。默认为  [0, 0] 注意：Mapbox GL使用经度，纬度坐标顺序来匹配GeoJSON。

5. zoom<sup>5</sup>:地图的初始缩放级别。如果未在样式中指定，则默认为  0 。

6. bearing<sup>6</sup>:地图的初始方位（旋转），以度为单位从北方逆时针测量。默认为  0 。

7. pitch<sup>7</sup>:地图的初始俯仰（倾斜），以度数远离屏幕平面（0-60）测量。默认为  0 。

------

### **Mapbox工作原理**
Mapbox的模板地图样式中显示的所有数据都来自于四种Mapbox tilesets中的一个。
* Mapbox Streets包括基于OpenStreetMap的街道，建筑物，区域，水和土地数据。
* Mapbox Terrain包含一个全球高程数据集，包括轮廓，山体阴影和土地覆盖数据。
* Mapbox Traffic提供每5分钟不断更新的拥塞信息，道路几何形状源自OpenStreetMap。
* Mapbox Satellite包含来自各种来源的全球卫星图像，由Mapbox处理和缝合在一起。

------

### **些许疑惑**
当你尝试了"快速开始"中的步骤后，第一眼看到mapbox的地图时，你可能会惊艳于它的美，可切换的视角(俯仰角度)，自定义的样式...  

在体验的过程中你可能会发现瓦片服务的请求很慢，等待的时间让你抓耳挠腮。有没有什么措施可以改进瓦片的加载速度呢？  

答案是肯定的。限于篇幅因素，下一篇我们再来聊一聊“Mapbox数据源本地化”的一些探索