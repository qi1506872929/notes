# 经典面试题:

1. react/vue 中的 key 有什么作用？（key 的内部原理是什么？）
2. 为什么遍历列表时，key 最好不要用 index? 

## 虚拟 DOM 中 key 的作用：

key 是虚拟 DOM 对象的标识，当数据发生变化时，react 会根据【新数据】生成【新的虚拟 DOM】,随后 React 进行【新虚拟 DOM】与【旧虚拟 DOM】的 diff 比较，比较规则如下：

## 对比规则：

- 旧虚拟 DOM 中找到了与新虚拟 DOM 相同的 key

  - 若虚拟 DOM 中内容没变, 直接使用之前的真实 DOM
  - 若虚拟 DOM 中内容变了, 则生成新的真实 DOM，随后替换掉页面中之前的真实 DOM

- 旧虚拟 DOM 中未找到与新虚拟 DOM 相同的 key
  - 根据数据创建新的真实 DOM，随后渲染到到页面

## 用 index 作为 key 可能会引发的问题：

1.  若对数据进行：逆序添加、逆序删除等破坏顺序操作:
    会产生没有必要的真实 DOM 更新 ==> 界面效果没问题, 但效率低。
2.  如果结构中还包含输入类的 DOM：
    会产生错误 DOM 更新 ==> 界面有问题。

## 开发中如何选择 key?:

1. 最好使用每条数据的唯一标识作为 key, 比如 id、手机号、身份证号、学号等唯一值。

2. 如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，仅用于渲染列表用于展示，使用index作为key是没有问题的。

   ​                        使用index作为key是没有问题的。

```php+HTML
一、使用索引值(index)作为key：
    初次挂载组件：
    	1.初始的数据：
    		{id:'atguigu_001',name:'小张',age:18},
    		{id:'atguigu_002',name:'小李',age:19},
    	2.初始的虚拟DOM
    		<li key=0>小张-18 <input type="text"/> </li>
    		<li key=1>小李-19 <input type="text"/> </li>

    更新：
    	1.新的数据：
    		{id:'atguigu_003',name:'小王',age:20},
    		{id:'atguigu_001',name:'小张',age:18},
    		{id:'atguigu_002',name:'小李',age:19},
    	2.新的虚拟DOM
    		<li key=0>小王-20 <input type="text"/> </li>
    		<li key=1>小张-18 <input type="text"/> </li>
    		<li key=2>小李-19 <input type="text"/> </li>
二、使用唯一标识(id)作为key：
    初次挂载组件：
    	1.初始的数据：
    		{id:'atguigu_001',name:'小张',age:18},
    		{id:'atguigu_002',name:'小李',age:19},
    	2.初始的虚拟DOM
    		<li key="atguigu_001">小张-18 <input type="text"/> </li>
    		<li key="atguigu_002">小李-19 <input type="text"/> </li>

    更新：
    	1.新的数据：
    		{id:'atguigu_003',name:'小王',age:20},
    		{id:'atguigu_001',name:'小张',age:18},
    		{id:'atguigu_002',name:'小李',age:19},
    	2.新的虚拟DOM
    		<li key="atguigu_003">小王-20 <input type="text"/> </li>
    		<li key="atguigu_001">小张-18 <input type="text"/> </li>
    		<li key="atguigu_002">小李-19 <input type="text"/> </li>
```

