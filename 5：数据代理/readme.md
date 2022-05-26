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

# Vue 中的数据代理
1. Vue中的数据代理：通过vm对象来代理data对象中属性的操作（读/写）
2. Vue中数据代理的好处：更加方便的操作data中的数据
3. 基本原理：
    - 通过Object.defineProperty()把data对象中所有属性添加到vm上。
    - 为每一个添加到vm上的属性，都指定一个getter/setter。
    - 在getter/setter内部去操作（读/写）data中对应的属性。