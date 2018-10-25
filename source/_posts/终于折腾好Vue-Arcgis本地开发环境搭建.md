---
title: 终于折腾好Vue+Arcgis本地开发环境搭建
date: 2018-10-25 16:08:40
urlname: vue_arcgis
tags: 
    - ArcGIS
    - VUE
categories: VUE
keywords:
description:
---
利用了两个下班的时间来折腾这个事儿，总是不成功，其中的坑还算可以，没想象的那么多，实在不想用ArcGIS在线的API，原因你懂的，巨慢无比，
有时候底图不出来你都不知道时网络的原因还是自己配置的问题，其实想来就需要注意两个问题：
1、eari-loader的版本问题
2、离线arcgis api for js 的修改（这个好坑，后面说）
首先要说的是这个esri-loader的版本问题，现在目前主要又两种方式，高版本和低版本，之间的版本发分界线具体没查,反正1.0之前算低的，2。0之后算高的了,我用的低版本是0.3
后面调用的arcgis的api是我用的本地的，如果你用esri的cdn，网速慢，出不来，影响你心情，需要翻墙，自己斟酌，我是下载下来的，在自己本地的tomcat发布的,例子是官方的例子，以下是两个基本的代码

# 首先看低版本：
```javascript
<template>
    <div id="map"> 
    </div>
</template>
<script>
import esriLoader from 'esri-loader';
export default {
    mounted: function() {    
        if(!esriLoader.isLoaded()) {
            esriLoader.bootstrap((err) => {
                if(err) {
                    console.error(err);
                } else {
                    this.createMap();
                }
            }, {
                url: 'http://localhost:8088/arcgis_js_api/library/3.21/3.21/init.js'
            });
        } else {
            this.createMap();
        }
    },
    methods: {
        createMap: function() {
            esriLoader.dojoRequire(['esri/map'], (Map) => {
                let map = new Map('map', {
                    center: [117.10, 39.10],
                    zoom: 10,
                    basemap: 'osm'　　　　　　　　　　　　　　
                });
            });
        }
    }
}
</script>
<style>
    @import url('http://localhost:8088/arcgis_js_api/library/3.21/3.21/esri/css/esri.css');
    #map{
        width: 80%;
        height: 90%;
    }
</style>
```
# 再看高版本的写法：
```javascript
<template>
    <div id="map"> 
    </div>
</template>
<script>
import esriLoader from 'esri-loader';
export default {
    mounted: function() {    
        esriLoader.loadScript ({ 
            url: 'http://localhost:8088/arcgis_js_api/library/3.21/3.21/init.js'
        });
        esriLoader.loadCss ('http://localhost:8088/arcgis_js_api/library/3.21/3.21/esri/css/esri.css');
        esriLoader.loadModules ([
            'esri/map'
        ]).then (Map => {
            this.createMap (Map);
        }).catch ((err) => {
            console.log (err.message);
        });
    },
    methods: {
        createMap: function(Map) {
                let map = new Map('map', {
                    center: [117.10, 39.10],
                    zoom: 10,
                    basemap: 'osm'　　　　　　　　　　　　　　
            });
        }
    }
}
</script>
<style>
    @import url('http://localhost:8088/arcgis_js_api/library/3.21/3.21/esri/css/esri.css');
    #map{
        width: 80%;
        height: 90%;
    }
</style>
```
如果按上面的代码，你的底图显示出来了，那么恭喜，你成功了，如果报错了，GG...
这是因为，我当时忘记了arcgis api离线部署需要改东西了，好烦，好久也没发现，一直报错，后来脑回路统领，才发现，具体方法是：
1、打开arcgis_js_api\library\3.2\dojo\dojo.js ，搜索 [HOSTNAME_AND_PATH_TO_JSAPI], 替换为localhost/arcgis_js_api/library/3.2/
2、打开arcgis_js_api\library\3.2\init.js ，搜索 [HOSTNAME_AND_PATH_TO_JSAPI], 替换为 localhost/arcgis_js_api/library/3.2/
注意:localhost 表示本地地址，如果是服务需使用IP或者域名，这样就ok啦！
如果还是没有解决，欢迎留言探讨~


