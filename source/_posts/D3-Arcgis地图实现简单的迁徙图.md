---
title: D3+Arcgis地图实现简单的迁徙图
date: 2018-09-01 10:41:48
urlname:
tags: 
    - D3.js
    - ArcGIS
    - Javascript
    - 迁徙图
categories: 可视化
---
&nbsp;项目中有需要很多数据可视化的图表，用的最多的也就数当前的Echarts了，用过的同学都应该清楚，基本的图表来说Echarts做的已经很好了，
但是诟病最多的也当属Echarts的定制度不高，可配置性差，有时候需要调一点样式，都很难实现，所以最近想开始学习一下D3.js，D3的优势
就之一也就是其高度的可定制化，完全都是自己来画的，但这也是其上手比较难的主要原因，需要有基本的前端Dom、JS、CSS基础才能做出比较
炫酷的图表，不过也不至于灰心，慢慢去做就好了。
&nbsp;目前项目中遇到一个ArcGIS地图上做迁徙图的功能，网上也有Echarts结合ArcGIS做的实例，本着爱折腾的态度，正合适去学习一下D3，自己着手去做了
一下小demo，也没去完善，给大家一个参考。
&nbsp;首先看下最终效果图：
![迁徙图效果图](/images/D3_Arcgis/overlay.png)
## 首先引用D3.js目前是V5版本：
`<script src="https://d3js.org/d3.v5.min.js"></script>`
可以去[D3.js官网](https://d3js.org/)下载
## 引入ArcGIS类库
我用的是4.6，目前最新的是4.8了
`<link rel="stylesheet" href="https://js.arcgis.com/4.8/esri/css/main.css">`
`<script src="https://js.arcgis.com/4.8/"></script>`
可以去[ArcGIS官网](https://developers.arcgis.com/javascript/latest/guide/get-api/index.html)下载
## 初始化加载ArcGIS地图
```javascript
function init() {
	    	require(["esri/views/MapView",
		  		"esri/Map",
		  		"esri/layers/OpenStreetMapLayer",
		  		"esri/geometry/Extent",
		  		"esri/geometry/Point",
		  		"esri/geometry/ScreenPoint",
		  		"esri/tasks/support/Query",
		  		"esri/tasks/QueryTask"
		  		], function(MapView,
                  Map,
                  OpenStreetMapLayer,
                  Extent,
                  Point,
                  ScreenPoint,
                  Query,
                  QueryTask) { 
		  	        myMap = new Map({
		  	            basemap:'osm'
		  	        });
		  	        view = new MapView({
		  	            map: myMap,
		  	            container: "mapContainer",
		  	            center : [112.53,37.87],
		  			    zoom : 12,
		  	        });
                    //由于我用的是测试数据，我在这自己发布了一个图层，调取他们的坐标
		  	        var layer="http://xx.xx.x.x:6080/y/MapServer"
		      		var queryTask = new QueryTask(layer+"/0");
		      	    var query= new Query();
		      	    query.where = "1 = 1";
		      	    query.outFields = ["*"];
		      	    query.returnGeometry = true;
		      	    queryTask.execute(query).then(function(results){
		      	    	if (results.features.length >= 1) {
		        	    	reyuan = results.features;
		        	    }
		      	    });
                    //监听平移，缩放和旋转
		      	    view.watch('center, scale, rotation',function(){
		      	    	if(initstate){
		      	    		geoToscreen();
		      	    	}
		      	    })
		  		});
	      }
```
## 点击绘制的方法
```javascript
function play(){
    initstate=true;//用来一个变量区别初始化，逻辑可忽略
    //(".esri-view-surface")是我把svg绘制添加到了这个view里面
    var poitMoveSvg = d3.select(".esri-view-surface")
        .append("svg")
        .attr("id", "d3_svg")
        .attr("class", "svgTransform")
    g = d3.select("#d3_svg")
        .append("g")
        .attr("id", "pointMove");
    geoToscreen();
}
```
## 主要的绘制方法
```javascript
function geoToscreen(){
    document.getElementById("d3_svg").pauseAnimations()
    d3.selectAll("#pointMove path").remove();
    d3.selectAll("#pointMove circle").remove()
    var s_screen=view.toScreen(reyuan[0].geometry);
    for(var i=1,len=reyuan.length;i<len;i++){
        var color='#17A05D';
        var e_screen=view.toScreen(reyuan[i].geometry);
        var path='M'+s_screen.x+','+s_screen.y+'A800,800 0 0,1 '+e_screen.x+','+e_screen.y
        var arrow='m0,0L-20,-10,-13,0,-20,10,0,0';
            g.append("path")//画路径
                .attr("d", path)
                .attr("stroke", color)
                .attr("fill", "none")
                .attr("stroke-width", "2");
            g.append("path")//画箭头
                .attr("d", arrow)
                .attr("stroke", "#0188FB")
                .attr("fill", "#0188FB")
                .attr("stroke-width", "2")
                .append('animateMotion')
                .attr('path', path)
                .attr('rotate', "auto")
                .attr('dur', "8s")
                .attr('repeatCount', "indefinite");
            g.append("circle")//起始点的圆圈
                .attr("r", 8)
                .attr("fill", "#E9A938")
                .attr("cx",s_screen.x)
                .attr("cy",s_screen.y);
            g.append("circle")
                .attr("r", 8)
                .attr("fill", "#81D841")
                .attr("cx",e_screen.x)
                .attr("cy",e_screen.y)
        
    }
    document.getElementById("d3_svg").unpauseAnimations()
}
```
## dom部分
```html
<!DOCTYPE html>
<html>
<head>
	<script src="https://d3js.org/d3.v5.min.js"></script>
	<link rel="stylesheet" href="https://js.arcgis.com/4.8/esri/css/main.css">
    <script src="https://js.arcgis.com/4.8/"></script>
	<style>
		html,body,#mapContainer{
		    padding:0px;
		    margin:0px;
			width:100%;
			height:900px;
		}
		.svgTransform{
			position: absolute;
			left:0px;
			top:0px;
			width:100%;
			height:100%;
		}
	</style>
</head>
<body onload="init()">
    <button style="position:absolute;left:20px;top:20px;" onclick="play()">飞行图</button>
	<div id="mapContainer"></div>
</body>
</html>
```
以上，代码比较low，demo比较简洁，后期有时间会慢慢学习，欢迎批评指正！