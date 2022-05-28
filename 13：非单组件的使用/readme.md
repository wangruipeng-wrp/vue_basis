# Vue中使用组件的三大步骤

## 步骤一、定义（创建）组件（如何定义一个组件？）
使用Vue.extend(options)创建，其中options和new Vue(options)时传入的那个options几乎一样，小区别如下：
1. el不要写，为什么？ ——— 最终所有的组件都要经过一个vm的管理，由vm中的el决定服务哪个容器。
2. data必须写成函数，为什么？ ———— 避免组件被复用时，数据存在引用关系。
> 备注：使用template可以配置组件结构。

## 步骤二、注册组件（如何注册组件？）
1. 局部注册：靠new Vue的时候传入components选项
2. 全局注册：靠Vue.component('组件名',组件)

## 步骤三、使用组件(写组件标签)
组件标签：`<school></school>`

## 四、几个注意点：
1. 关于组件名:
    - 一个单词组成：
        - 第一种写法(首字母小写)：school
        - 第二种写法(首字母大写)：School
    - 多个单词组成：
        - 第一种写法(kebab-case命名)：my-school
        - 第二种写法(CamelCase命名)：MySchool (需要Vue脚手架支持)
    - 备注：
        1. 组件名尽可能回避HTML中已有的元素名称，例如：h2、H2都不行。
        2. 可以使用name配置项指定组件在开发者工具中呈现的名字。

2. 关于组件标签:
    - 第一种写法：<school></school>
    - 第二种写法：<school/>
    - 备注：不用使用脚手架时，<school/>会导致后续组件不能渲染。

3. 一个简写方式：
    `const school = Vue.extend(options)` 可简写为：`const school = options`