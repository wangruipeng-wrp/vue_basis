# 简述 Object.defineProperty 方法
在本例中可见想要为person对象代理一个age属性，但是person对象中其实是没有age这个属性的，是通过`Object.defineProperty`方法为person对象挂载了一个age属性上去，而不是代理了一个本身就存在于person对象中的属性。事实上，我们无法代理一个本身就存在于person对象上的数据，直接代理见如下代码：
```javascript
<script type="text/javascript" >
    let person = { age: 18 },

    Object.defineProperty(person,'age',{
        get(){
            return person.age
        },
        set(value){
            person.age = value
        }
    })
</script>
```
我们没办法这么做这个事情，因为这里面出现了一个死循环，我们在获取和修改`person.age`的方法里面又去获取和修改了`person.age`，于是陷入了死循环。正确的写法应该是在本例中的写法，将age用另外的一个变量number去表现出来，然后获取和修改都是在对number做操作。所以`Object.defineProperty`方法做的事情其实是将一个不属于person对象的变量挂载到person对象身上，但是对外表现为：这个number变量属于person对象的age属性，而对于person对象的age属性的修改则通过`Object.defineProperty`的getter和setter来完成。

一个有意思的问题，也是一个面试题：  
> 什么样的 a 可以满足 (a === 1 && a === 2 && a === 3) === true 呢？(注意是 3 个 =，也就是严格相等)???

```javascript
let current = 1
Object.defineProperty(window, 'a', {
    get () {
        return current++
    }
})

console.log(a === 1 && a === 2 && a === 3) // true
```

# Vue 中的数据代理
1. Vue中的数据代理：通过vm对象来代理data对象中属性的操作（读/写）
2. Vue中数据代理的好处：更加方便的操作data中的数据
3. 基本原理：
    - 通过Object.defineProperty()把data对象中所有属性添加到vm上。
    - 为每一个添加到vm上的属性，都指定一个getter/setter。
    - 在getter/setter内部去操作（读/写）data中对应的属性。

# Vue监视数据的原理：
1. vue会监视data中所有层次的数据。

2. 如何监测对象中的数据？
    - 通过setter实现监视，且要在new Vue时就传入要监测的数据。
        > (1).对象中后追加的属性，Vue默认不做响应式处理  
        > (2).如需给后添加的属性做响应式，请使用如下API：
        `Vue.set(target，propertyName/index，value)`或 `vm.$set(target，propertyName/index，value)`

3. 如何监测数组中的数据？
    - 通过包裹数组更新元素的方法实现，本质就是做了两件事：
        > (1).调用原生对应的方法对数组进行更新。  
        > (2).重新解析模板，进而更新页面。

4. 在Vue修改数组中的某个元素一定要用如下方法：
    - 使用这些API:push()、pop()、shift()、unshift()、splice()、sort()、reverse()
    - Vue.set() 或 vm.$set()
				
> 特别注意：Vue.set() 和 vm.$set() 不能给vm 或 vm的根数据对象 添加属性！！！