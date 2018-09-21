---
title: Mapbox tiles-server 本地化探索
date: 2018/7/16 19:00:00
tags:
- 数据可视化 
- mapboxgl
- 地理信息可视化
categories: 数据可视化
---

 上次我们谈到了由于404等原因, Mapbox在国内可能会出现加载较慢的情况. 亦或是项目需要部署在内网的情景, 这时候就要考虑将Mapbox服务本地化部署. 

 Mapbox地图服务使用MBTiles存储数据, 目前很多地图服务都接受了这套标准(如: OpenStreatMap). 所以可以通过搭建自己的tiles-server以替代直接使用Mapbox的官方地图服务.<!-- more -->

### 容器化部署
------
 本次本地化部署用到了docker hub中的klokantech/openmaptiles-server镜像, 部署流程如下:
 1. 安装docker环境, 启动docker服务;

 2. 通过`docker pull klokantech/openmaptiles-server`命令下载镜像;

 3. 通过`docker run --rm -it -v $(pwd):/data -p 8080:80 klokantech/openmaptiles-server`命令启动容器; 命令中${pwd}指docker数据的映射地址, 例如/home/mapdata/, 该文件夹下存放服务的配置文件,,自定义的地图样式以及瓦片数据集;

    ```
    [root@izuf6g82g0du6i91amh60fz mapdata]# ll
      total 3643528
      -rw-r--r-- 1 root root 3727319040 Jul 17 21:02 2017-07-03_asia_china.mbtiles
      -rw-r--r-- 1 root root        706 Sep  3 18:52 config.json
      drwxr-xr-x 2 root root       4096 Sep  3 18:53 styles
    ```

 4. 访问位于`http://localhost:8080/`的Web界面，然后按照Web向导进行操作;
    * 选择下载整个星球，国家或城市的瓦片数据

    * 从准备好的地图样式（Bright，Basic，Positron和Dark Matter）中选择地图的设计或提供自己的地图样式

    * 选择地图和服务的语言

    * 光栅矢量静态地图服务

等待服务所需文件下载完毕以及服务的初始化, 至此本地化部署工作告一段落. 

### 将应用切换到本地瓦片服务
------
按照以下步骤改造style的配置项, 可以实现从本地服务请求数据

```
const osmUrl = 'http://localhost:8080' // 本地数据服务地址
const mapStyle = 'customDarkStyle'  // 需要使用的地图样式
const spriteUrl = 'http://localhost:8888/images/sprite' // 雪碧图地址
let map = new mapboxgl.Map({
      container: 'container id',
      style: {
       'version': 8,
       'sprite': `${spriteUrl}`,
       'glyphs': `${osmUrl}/fonts/{fontstack}/{range}.pbf`,
       'sources': {
          'osm-tiles': {
            'type': 'raster',
            'tiles': [
              `${osmUrl}/styles/${mapStyle}/{z}/{x}/{y}.png`
            ],
            'tileSize': 256
          }
       },
       'layers': [{
        'id': 'background',
        'type': 'raster',
        'source': 'osm-tiles',
        'minzoom': 0,
        'maxzoom': 22
      }],
      center: [131, 30],
      zoom: 9,
      bearing: 0,
      pitch: 0
    });
```



