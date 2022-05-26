# Vue中有2种数据绑定的方式
1. 单向绑定(v-bind)：数据只能从data流向页面。
2. 双向绑定(v-model)：数据不仅能从data流向页面，还可以从页面流向data。  

> 备注：  
> 1. 双向绑定一般都应用在表单类元素上（如：input、select等）  
> 2. v-model:value 可以简写为 v-model，因为v-model默认收集的就是value值。  

# 利用双向绑定收集表单数据：
- 普通文本：`<input type="text"/>`，则v-model收集的是value值，用户输入的就是value值。
- 单选框：`<input type="radio"/>`，则v-model收集的是value值，且要给标签配置value值。
- 多选框：`<input type="checkbox"/>`
    1. 没有配置input的value属性，那么收集的就是checked（勾选 or 未勾选，是布尔值）  
    2. 配置input的value属性:  
        > (1)v-model的初始值是非数组，那么收集的就是checked（勾选 or 未勾选，是布尔值）  
        > (2)v-model的初始值是数组，那么收集的的就是value组成的数组  

- 备注：v-model的三个修饰符：
    - lazy：失去焦点再收集数据
    - number：输入字符串转为有效的数字
    - trim：输入首尾空格过滤