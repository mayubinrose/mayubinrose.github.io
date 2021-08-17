---
layout: titile
title: React
date: 2018-12-27 10:57:11
tags: React
categories: 前端
---
# React简介
## JSX
JSX语法中，可以使用自己创建的组件，一般大写字母开头
 <!--more--> 
## 生命周期函数
某一个时刻组件会自动调用执行的函数
例如：
render()：props或者state发生改变的时候自动调用
constructor()
componentWillMount()：组件即将被挂载到页面的时刻执行
componentDidMount()：组件挂载完之后执行
shouldComponentUpdate()：组件被更新之前会执行
componentWillUpdate()：shouldComponentUpdate函数之后render之前执行
componentDidUpdate()：组件更新完成之后执行
componentWillReceiveProps()：从父组件接收了参数，只要父组件的render函数重新被执行了，子组件的生命周期函数就会被执行，如果这个组件第一次存在于父组件之中，不会执行，这个组件已经存在于父组件中，才会执行
componentWillUnmount()：当这个组件即将被从页面移除的时候执行
### Updation

用store
 import {connect} from 'react-redux';
 export default connect()(Topic);

 