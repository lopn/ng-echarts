angularjs封装echarts2


=======

如何使用？
```
npm install

```
然后执行
```
gulp
```

ng-echarts只需要一个变量：
> * setting：
    setting.option为echarts中的option，因此你直接可以把官网的例子拷进来用
    其他参数的配置项
    * event：绑定事件
    * dataLoaded：数据是否加载（用于Loading）
    * loadingOption：加载效果配置项同官网

一个简单示例：
html中
```
<div ng-controller="MainCtrl">
     <ng-echarts class="col-md-6" style="height: 400px;" setting="setting"></ng-echarts>
</div>
```
js中
```
    angular.module('app',['ng-echarts'])
                .controller('MainCtrl', function ($scope,$timeout,$interval) {
                    var opt = {
                        title : {
                            text: '未来一周气温变化(5秒后自动轮询)',
                            subtext: '纯属虚构'
                        },
                        tooltip : {
                            trigger: 'axis'
                        },
                        legend: {
                            data:['最高气温','最低气温']
                        },
                        toolbox: {
                            show : true,
                            feature : {
                                mark : {show: true},
                                dataView : {show: true, readOnly: false},
                                magicType : {show: true, type: ['line', 'bar']},
                                restore : {show: true},
                                saveAsImage : {show: true}
                            }
                        },
                        calculable : true,
                        xAxis : [
                            {
                                type : 'category',
                                boundaryGap : false,
                                data : ['周一','周二','周三','周四','周五','周六','周日']
                            }
                        ],
                        yAxis : [
                            {
                                type : 'value',
                                axisLabel : {
                                    formatter: '{value} °C'
                                }
                            }
                        ],
                        series : [
                            {
                                name:'最高气温',
                                type:'line',
                                data:[11, 11, 15, 13, 12, 13, 10],
                                markPoint : {
                                    data : [
                                        {type : 'max', name: '最大值'},
                                        {type : 'min', name: '最小值'}
                                    ]
                                },
                                markLine : {
                                    data : [
                                        {type : 'average', name: '平均值'}
                                    ]
                                }
                            },
                            {
                                name:'最低气温',
                                type:'line',
                                data:[1, -2, 2, 5, 3, 2, 0],
                                markPoint : {
                                    data : [
                                        {name : '周最低', value : -2, xAxis: 1, yAxis: -1.5}
                                    ]
                                },
                                markLine : {
                                    data : [
                                        {type : 'average', name : '平均值'}
                                    ]
                                }
                            }
                        ]
                    };
    
                    var setting = {
                        dataLoaded: false,
    //                        loadingOption: {
    //                            text : '数据加载中...',
    //                            effect : 'bubble',
    //                            textStyle : {
    //                                fontSize : 40
    //                            }
    //                        },
                        event: [{click:onClick}],
                        option: {}
                    };
    
                    function onClick(params){
                        alert(params.name);
                    };
    
                    $scope.setting = setting;
    
                    $timeout(function () {
                        $scope.setting.option = opt;
                        $scope.setting.dataLoaded = true;
    
                    },2000);
    
                    var index = 1;
                    $interval(function () {
    
                        var loadText = ['数据加载中...','请等一等!','数据哪里跑','我靠，数据呢？'];
                        var effects = ['spin', 'bar' , 'ring' , 'whirling' , 'dynamicLine' , 'bubble'];
                        $scope.setting.loadingOption = {
                            text : loadText[_.random(0,loadText.length-1)],
                            effect : effects[_.random(0,effects.length-1)],
                            textStyle : {
                                fontSize : 20
                            }
                        };
                        $scope.setting.dataLoaded = false;
    
                        $timeout(function () {
                            var ndata = [];
                            for(var i=0;i<7;i++){
                                ndata[i] = Math.round(Math.random()*10)+10;
                            }
    
                            $scope.setting.option.series[0].data = ndata;
                            $scope.setting.option.title.text = '未来一周气温变化('+index+')';
                            index++;
    
                            $scope.setting.dataLoaded = true;
                        },2500);
    
                    }, 5000);
                });
```

