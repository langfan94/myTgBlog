---
title: 微信小程序WXS
date: 2018-07-23 15:27:52
tags:
---

# WXS

> WXS（WeiXin Script）是小程序的一套脚本语言，结合 WXML，可以构建出页面的结构。

在微信小程序的优化中遇到了渲染速度的问题：
在处理1000多长度的数组循环遍历之后 在wxml 页面上渲染处理完成之后的数据,经过测试渲染时间分别是:
* Android:<span style="color:red">  1-2秒 </span>
* ios: <span style="color:red">  4-5秒 </span>

测试代码如下:

```js
// test.wxs
    var forFn = function(arr) {
    var newArr = []
    var ArrAll = []
    console.log(arr, arr.length)
    for(var i = 0; i < 1000; i++) {
        var obj = {"answer":32,"chapterId":121,"collect":null,"course":null,"courseTypeId":null,"difficulty":3,"explains":"<p>违反《道路交通安全法》，属于违法行为。官方已无违规、违章的说法。</p>","falseCount":64784996,"id":480,"mediaContent":"","mediaHeight":0.0,"mediaType":0,"mediaWidth":0.0,"optionA":"违章行为","optionB":"违法行为","optionC":"过失行为","optionD":"违规行为","optionE":"","optionF":"","optionG":"","optionH":"","optionType":1,"question":"驾驶机动车在道路上违反道路交通安全法的行为，属于什么行为？","questionId":800000,"trueCount":254344319,"wrongRate":0.203005}
        ArrAll.push(obj)
        if(i < 10) {
            arr.push(obj)
        }
    }
    return arr;
}

module.exports.forFn = forFn
```

```html
    <!-- test.wxml -->
    <wxs module="m1" src="./test.wxs" />
    <swiper class="swiperContainer">
        <block wx:for="{{m1.forFn(examArr)}}" wx:key="{{index}}">
            <swiper-item item-id="{{item.swIndex}}">      <!-- 单选题 -->  
                <scroll-view scroll-y class="scrollStyle" wx:if="{{item.optionType != 2}}">   
                    <view class="examQue">
                        <text class="queType">{{item.examType}}</text> 
                        <text class="queCon">{{item.question}}</text> 
                    </view>
                    <view class="preImgView" wx:if="{{item.mediaType == 1}}">              <!-- 图片 -->  
                        <image class="preImg" mode="aspectFit" src="{{item.mediaContent}}" data-src="{{item.mediaContent}}" bindtap="previewImg"></image>
                    </view>
                    <view class="videoBg" wx:elif="{{item.mediaType == 2}}">            <!-- 视频 --> 
                        <image class="videoStyle" class="preImg" mode="aspectFit" src="../../assets/images/videoBg.jpg" data-src="{{item.mediaContent}}" bindtap="videoClick"></image>
                    </view>
                    <view class="examAns" wx-if="{{item.optionA && item.optionA != ''}}" data-id="{{item.id + ' A'}}" capture-catch:tap="selectAnsFn">
                        <view wx:if="{{item.selectAns && item.selectAns == 'A'}}">
                            <icon class="iconfont icon-sel-correct　iconCorrect" wx:if="{{item.selectAns == item.answerStr}}"/>
                            <icon class="iconfont icon-sel-wrong iconWrong" wx:else/>
                        </view>
                        <view class="circleStyle" wx:else>A</view>
                        <text>{{item.optionA}}</text>
                    </view>
                    <view class="examAns" wx-if="{{item.optionB && item.optionB != ''}}" data-id="{{item.id + ' B'}}" capture-catch:tap="selectAnsFn">
                        <view wx:if="{{item.selectAns && item.selectAns == 'B'}}">
                            <icon class="iconfont icon-sel-correct iconCorrect" wx:if="{{item.selectAns == item.answerStr}}"/>
                            <icon class="iconfont icon-sel-wrong iconWrong" wx:else/>
                        </view>
                        <view class="circleStyle" wx:else>B</view>
                        <text>{{item.optionB}}</text>
                    </view>
                    <view class="examAns" wx-if="{{item.optionC && item.optionC != ''}}" data-id="{{item.id + ' C'}}" capture-catch:tap="selectAnsFn">
                        <view wx:if="{{item.selectAns && item.selectAns == 'C'}}">
                            <icon class="iconfont icon-sel-correct iconCorrect" wx:if="{{item.selectAns == item.answerStr}}"/>
                            <icon class="iconfont icon-sel-wrong iconWrong" wx:else/>
                        </view>
                        <view class="circleStyle" wx:else>C</view>
                        <text>{{item.optionC}}</text>
                    </view>
                    <view class="examAns" wx-if="{{item.optionD && item.optionD != ''}}" data-id="{{item.id + ' D'}}" capture-catch:tap="selectAnsFn">
                        <view wx:if="{{item.selectAns && item.selectAns == 'D'}}">
                            <icon class="iconfont icon-sel-correct iconCorrect" wx:if="{{item.selectAns == item.answerStr}}"/>
                            <icon class="iconfont icon-sel-wrong iconWrong" wx:else/>
                        </view>
                        <view class="circleStyle" wx:else>D</view>
                        <text>{{item.optionD}}</text>
                    </view>
                    <view class="standardAnsWrap" wx:if="{{btnActiveIndex == '1' || (item.isDo && item.selectAns != item.answerStr)}}">
                        <view class="standardAnsStyle">
                            <text>标准答案:</text>
                            <text wx:for="{{item.answerArr}}" wx:key="{{index}}" wx:for-item="answerItem">{{answerItem + ' '}}</text>
                        </view>
                        <rich-text class="tipStyle" nodes="{{item.explains}}"></rich-text>
                    </view>
                </scroll-view>
                <scroll-view scroll-y class="scrollStyle" wx:else>   
                    <view class="examQue">
                        <text class="queType">{{item.examType}}</text> 
                        <text class="queCon">{{item.question}}</text> 
                    </view>
                    <view class="examAns" wx-if="{{item.optionA && item.optionA != ''}}" data-id="{{item.id + ' A'}}" capture-catch:tap="selectAnsFn">
                        <view wx:if="{{item.isDo}}">
                            <view class="circleStyle" wx:if="{{item.selectAns && !item.selectAns.A}}">A</view>
                            <icon class="iconfont icon-sel-correct　iconCorrect" wx:elif="{{item.selectAns.result}}"/>
                            <icon class="iconfont icon-sel-wrong iconWrong" wx:else/>
                        </view>
                        <view class="circleStyle {{item.selectAns.A ? 'activeSel' : ''}}" wx:else>A</view>
                        <text>{{item.optionA}}</text>
                    </view>
                    <view class="examAns" wx-if="{{item.optionB && item.optionB != ''}}" data-id="{{item.id + ' B'}}" capture-catch:tap="selectAnsFn">
                        <view wx:if="{{item.isDo}}">
                            <view class="circleStyle" wx:if="{{item.selectAns && !item.selectAns.B}}">B</view>
                            <icon class="iconfont icon-sel-correct　iconCorrect" wx:elif="{{item.selectAns.result}}"/>
                            <icon class="iconfont icon-sel-wrong iconWrong" wx:else/>
                        </view>
                        <view class="circleStyle {{item.selectAns.B ? 'activeSel' : ''}}" wx:else>B</view>
                        <text>{{item.optionB}}</text>
                    </view>
                    <view class="examAns" wx-if="{{item.optionC && item.optionC != ''}}" data-id="{{item.id + ' C'}}" capture-catch:tap="selectAnsFn">
                        <view wx:if="{{item.isDo}}">
                            <view class="circleStyle" wx:if="{{item.selectAns && !item.selectAns.C}}">C</view>
                            <icon class="iconfont icon-sel-correct　iconCorrect" wx:elif="{{item.selectAns.result}}"/>
                            <icon class="iconfont icon-sel-wrong iconWrong" wx:else/>
                        </view>
                        <view class="circleStyle {{item.selectAns.C ? 'activeSel' : ''}}" wx:else>C</view>
                        <text>{{item.optionC}}</text>
                    </view>
                    <view class="examAns" wx-if="{{item.optionD && item.optionD != ''}}" data-id="{{item.id + ' D'}}" capture-catch:tap="selectAnsFn">
                        <view wx:if="{{item.isDo}}">
                            <view class="circleStyle" wx:if="{{item.selectAns && !item.selectAns.D}}">D</view>
                            <icon class="iconfont icon-sel-correct　iconCorrect" wx:elif="{{item.selectAns.result}}"/>
                            <icon class="iconfont icon-sel-wrong iconWrong" wx:else/>
                        </view>
                        <view class="circleStyle {{item.selectAns.D ? 'activeSel' : ''}}" wx:else>D</view>
                        <text>{{item.optionD}}</text>
                    </view>
                    <view wx:if="{{!item.isDo}}" class="commiteStyle" data-id="{{item.id}}" bindtap="commiteBtn">确定</view>
                    <view class="standardAnsWrap" wx:if="{{btnActiveIndex == '1' || item.isDo && !item.selectAns.result}}">
                        <view class="standardAnsStyle">
                            <text>标准答案:</text>
                            <text wx:for="{{item.answerArr}}" wx:key="{{index}}" wx:for-item="answerItem">{{answerItem + ' '}}</text>
                        </view>
                        <rich-text class="tipStyle" nodes="{{item.explains}}"></rich-text>
                    </view>
                </scroll-view>
            </swiper-item>
        </block>
    </swiper>
    <button type="default" size="defaultSize" bindtap="defaultTap">Button</button>
```
```js
    // test.js
    Page({
    data: {
        imgUrls: [
            'http://img02.tooopen.com/images/20150928/tooopen_sy_143912755726.jpg',
            'http://img06.tooopen.com/images/20160818/tooopen_sy_175866434296.jpg',
            'http://img06.tooopen.com/images/20160818/tooopen_sy_175833047715.jpg',
            'http://img02.tooopen.com/images/20150928/tooopen_sy_143912755726.jpg',
            'http://img06.tooopen.com/images/20160818/tooopen_sy_175866434296.jpg',
            'http://img06.tooopen.com/images/20160818/tooopen_sy_175833047715.jpg',
            'http://img02.tooopen.com/images/20150928/tooopen_sy_143912755726.jpg',
            'http://img06.tooopen.com/images/20160818/tooopen_sy_175866434296.jpg',
            'http://img06.tooopen.com/images/20160818/tooopen_sy_175833047715.jpg'
        ],
        indicatorDots: false,
        autoplay: false,
        interval: 5000,
        duration: 1000,
        examArr: [],
        listAll: [],
    }
})
```

本来使用js处理的循环放入到wxs中进行处理之后,执行速度如下:
* Android:<span style="color:red">  1-2秒 </span>
* ios: <span style="color:red">  1-2秒 </span>

Android上处理速度并没有明显变化,但是ios上处理速度快了很多.

注意：
1. wxs 不依赖于运行时的基础库版本，可以在所有版本的小程序中运行。
2. wxs 与 javascript 是不同的语言，有自己的语法，并不和 javascript 一致。
3. wxs 的运行环境和其他 javascript 代码是隔离的，wxs 中不能调用其他  javascript 文件中定义的函数，也不能调用小程序提供的API。
4. wxs 函数不能作为组件的事件回调。
5. 由于运行环境的差异，在 iOS 设备上小程序内的 wxs 会比 javascript 代码快 2 ~ 20 倍。在 android 设备上二者运行效率无差异。

但是如果js中setData改变数据之后,视图也会更新吗？

接下来改变代码测试：

test.js 文件更改如下:

```js 
    Page({
    data: {
        imgUrls: [
            'http://img02.tooopen.com/images/20150928/tooopen_sy_143912755726.jpg',
            'http://img06.tooopen.com/images/20160818/tooopen_sy_175866434296.jpg',
            'http://img06.tooopen.com/images/20160818/tooopen_sy_175833047715.jpg',
            'http://img02.tooopen.com/images/20150928/tooopen_sy_143912755726.jpg',
            'http://img06.tooopen.com/images/20160818/tooopen_sy_175866434296.jpg',
            'http://img06.tooopen.com/images/20160818/tooopen_sy_175833047715.jpg',
            'http://img02.tooopen.com/images/20150928/tooopen_sy_143912755726.jpg',
            'http://img06.tooopen.com/images/20160818/tooopen_sy_175866434296.jpg',
            'http://img06.tooopen.com/images/20160818/tooopen_sy_175833047715.jpg'
        ],
        indicatorDots: false,
        autoplay: false,
        interval: 5000,
        duration: 1000,
        examArr: [],
        listAll: [],
    },
    // add
    defaultTap() {
        console.log(this.data.examArr);
        this.setData({
            examArr: []
        })
    }
})
```

测试发现视图并不会更新....并且js data中的数据是wxs处理之前的数据.

这是因为wxs 的运行环境和其他 javascript 代码是隔离的.

wxs不能调用javascript代码, javascript也不能调用wxs代码.

不过wxs可以编写工具类或者公共方法去处理数据之后渲染,对于ios是有速度上的优化的.

总结:
针对微信小程序架构的缺点，微信团队推出了 WXS。WXS 就是在 page-frame 中运行的 JS，可以对 view 数据做一些变换。WXS 对性能的贡献就只有一点：与 WXML 是在同一个线程运行的，避免了跨线程通信的开销。别的性能上的提升？完全没有。
