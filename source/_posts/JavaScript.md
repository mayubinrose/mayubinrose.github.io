---
layout: title
title: JavaScript
date: 2018-12-19 15:14:03
tags: JavaScript
categories: 前端
---
# 写一点自己的理解吧
## 点击函数的写法
点击函数经常使用，最简单的就是
var bianliang =document.getElementById("id");
bianliang.onclick=function(){}或者bianliang.on("click",funciton()){}
或者在前端界面的id处写一个onclick="test()"，然后在JS中写function test()
 <!--more--> 
## ajax和getJSON
 $.getJSON(
     url,            //请求URL
     [data],         //传参，可选参数
     [callback]      //回调函数，可选参数
 　);
ajax异步传递数据，用的真的很频繁
$.ajax({
            type: "post/get",
            url: "",
            data: JSON.stringify({"key": value}),
            dataType: "json",
            contentType: "application/json;charset=UTF-8",
            success: [callback]{
            },
        })
## document
document.querySelector("#id")：读取某个id里面的元素
document.getElementById("id")
## canvas
canvas对象表示一个html画布
var canvas1 = document.getElementById('canvas1');
var context1 = canvas1.getContext('2d');
context1.drawImage(video,0, 0, 300, 300);//前两个确定左上角开始画的坐标，后两个确定宽和高
## 浏览器对象navigator
获取用户浏览器摄像头
navigator.getUserMedia = navigator.getUserMedia || navigator.webkitGetUserMedia || navigator.mozGetUserMedia || navigator.msGetUserMedia;