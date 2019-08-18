---
title: echarts - Angular 1.x指令集成
tags: [Tool, Javascript, Angular]
date: 2019-08-18 16:15:38
---

## html
```html
<div ng-app="demo" ng-controller="myCtrl" id="demo">
<div style="width: 640px; height: 320px;" bar-chart e-data="data"></div>
<div style="width: 640px; height: 320px;" pie-chart e-data="data"></div>
</div>
<!-- ... 省略文件中其他无关内容 -->
<script src="js/echarts.min.js"></script>
```

## js
```javascript
var app = angular.module('demo', []); //定义一个模块
angular.module('demo').controller("myCtrl", function($scope, $http, $interval) { //定义控制器
  $interval(function() { //调用$interval服务执行循环任务，3秒自动更新一次数据
    $http.get("/demo_get") //调用$http服务获取数据
      .success(function(data) {
        if (data.status == '0') {
          $scope.data = data.data; //将数据放在model中
        }
      }).error(function() {
        $scope.data[0].value = Math.floor(Math.random() * 2000); //模拟数据改变测试
      });
  }, 3000);
  //假设获取到的数据如下： 
  $scope.data = [{
    'value': 335,
    'name': '直接访问'
  }, {
    'value': 310,
    'name': '邮件营销'
  }, {
    'value': 234,
    'name': '联盟广告'
  }, {
    'value': 135,
    'name': '视频广告'
  }, {
    'value': 748,
    'name': '搜索引擎'
  }];

});
angular.module('demo').directive('barChart', function($window) { //定义条形图指令
  return {
    restrict: 'A', //以属性的方式调用指令
    link: function($scope, element, attrs) { //attrs是DOM元素的属性集合
      var myChart = echarts.init(element[0]); //element是一个jqlite对象，如果jQuery在AngularJS之前引入,则是一个jQuery对象,可使用jQuery所有的方法
      $scope.$watch(attrs.eData, function(newValue, oldValue, scope) { //监听属性e-data的值，当数据发生改变时执行作为第二个参数的函数
        var xData = [],
          sData = [],
          data = newValue; //获取$scope.data
        angular.forEach(data, function(val) {
          xData.push(val.name);
          sData.push(val.value);
        });
        var option = {
          title: {
            text: '某站点用户访问来源',
            subtext: '纯属虚构',
            x: 'center'
          },
          color: ['#3398DB'],
          tooltip: {
            trigger: 'axis',
            axisPointer: {
              type: 'shadow'
            }
          },
          grid: {
            left: '3%',
            right: '4%',
            bottom: '3%',
            containLabel: true
          },
          xAxis: [{
            type: 'category',
            data: xData,
            axisTick: {
              alignWithLabel: true
            }
          }],
          yAxis: [{
            type: 'value'
          }],
          series: [{
            name: '直接访问',
            type: 'bar',
            barWidth: '60%',
            data: sData
          }]
        };
        myChart.setOption(option);
      }, true); //$watch的第三个参数，是否深度监听，true监听对象所有属性的变化。
      $window.onresize = function() { //调用window服务，使图表能响应式调整大小
        myChart.resize();
      };
    }
  };
});

angular.module('demo').directive('pieChart', function($window) { //定义饼图指令
  return {
    restrict: 'A',
    link: function($scope, element, attrs) { //attrs是DOM元素的属性集合
      var myChart = echarts.init(element[0]);
      $scope.$watch(attrs.eData, function(newValue, oldValue, scope) {
        var legend = [];
        angular.forEach(newValue, function(val) {
          legend.push(val.name);
        });
        var option = {
          title: {
            text: '某站点用户访问来源',
            subtext: '纯属虚构',
            x: 'center'
          },
          tooltip: {
            trigger: 'item',
            formatter: "{a} <br/>{b} : {c} ({d}%)"
          },
          legend: {
            orient: 'vertical',
            left: 'left',
            data: legend
          },
          series: [{
            name: '访问来源',
            type: 'pie',
            radius: '55%',
            center: ['50%', '60%'],
            data: newValue,
            itemStyle: {
              emphasis: {
                shadowBlur: 10,
                shadowOffsetX: 0,
                shadowColor: 'rgba(0, 0, 0, 0.5)'
              }
            }
          }]
        };
        myChart.setOption(option);
      }, true);
      window.addEventListener("resize", function() {  //这里使用$window.onresize方法会使前面的图表无法调整大小
        myChart.resize();
      });
    }
  };
});
```

## 参考
[ECharts - 可高度个性化定制的数据可视化图表](https://www.echartsjs.com/index.html)
[ECharts - API](https://www.echartsjs.com/api.html#echarts)
[在AngularJS中使用Echarts](https://codepen.io/chj/pen/ZpLpQo)