### ref和reactive

```
ref() 和reactive()都是接收一个普通的原始数据，再将其转换为响应式对象。
区别
1.ref可以同时处理基本数据类型和对象，而reactive只能处理处理对象而不支持基本数据类型。
2.ref是通过一个中间对象RefImpl持有数据，并通过重写它的set和get方法实现数据劫持的，本质上依旧是通过Object.defineProperty 对RefImpl的value属性进行劫持。reactive则是通过Proxy进行劫持的。Proxy无法对基本数据类型进行操作，进而导致reactive在面对基本数据类型时的束手无策。
3.返回类型不同
ref返回：RefImpl {__v_isShallow: false, dep: undefined, __v_isRef: true, _rawValue: 0, _value: 0}
reactive返回：Proxy(Object) {count: 0}
4.访问数据的方式不同
ref()返回的是RefImpl的一个实例对象，该对象通过_value私有变量持有原始数据，并重写了value的get方法。因此，当想要访问原始对象的时候，需要通过xxx.value的方式触发get函数获取数据。同样的，在修改数据时，也要通过xxx.value = yyy的方式触发set函数。
reactive() 返回的是原始对象的代理，代理对象具有和原始对象相同的属性，因此我们可以直接通过.xxx的方式访问数据
5.原始对象的可变性不同：可以给ref的值重新分配给一个新对象，而reactive只能修改当前代理的属性
ref通过一个RefImpl实例持有原始数据，进而使用.value属性访问和更新。而对于一个实例而言，其属性值是可以修改的。因此可以通过.value的方式为ref重新分配数据，无需担心RefImpl实例被改变进而破坏响应式
reactive返回的是原始对象的代理，因此不能对其重新分配对象，只能通过属性访问修改属性值，否则会破坏掉响应式
6.ref借助reactive实现对Object类型数据的深度监听
ref在发现被监听的原始对象是Object类形时，会将原始对象转换成reactive并赋值给_value属性。而此时ref.value返回的并不是原始对象，而是它的代理。
7.对侦听属性的影响不同
watch()默认情况下只监听ref.value的更改，而对reactive执行深度监听。

```



### watch和watchEffect

```
watchEffect handle函数会立即触发 默认immediate为true
watch handle函数不会立即触发 默认immediate为false

```



### vue3全局变量

```javascript
const app = createApp({})
app.config.globalProperties.$xxx = ()
```





