# 第5章 表单控件绑定

## 5.1 基本用法

在VueJs中，我们可以使用v-model指令同步用户输入的数据到Vue实例data属性中。

### 5.1.1 text

```html
<span></span>
<br>
<input type="text" v-model="name" placeholder="join DDFE">
```

### 5.1.2 checkbox

- 单个复选框，为`true`/`false`。
- 多个复选框，被选中的值将会放入一个数组中。

### 5.1.3 radio

- v-model 中的变量值会被赋值为对应的value值。

### 5.1.4 select

- 当被选中的option有value属性时，vm.selected为对应的value值，否则为option的text值。
- 多选的时候，被选中的值会放入一个数组中。

## 5.2 值绑定

借助v-bind来满足动态绑定实例属性的需求。

1. checkbox
    - true-value，false-value只适合同一个checkbox组只有一个checkbox的情况
    - 多个请使用`:value`进行值绑定

2. radio
    - `:value`
3. select
    - `:value`

## 5.3 v-model 修饰指令

使用修饰指令实现Model-View 同步发生的时机。

### 5.3.1 lazy

- 将同步改到在change中发生
- 原理：所有的表单控件都绑定change,只是在不设置lazy时，默认绑定input。

### 5.3.2 debounce

- 设置一个同步延迟的时间。在这个时间结束之后发生同步。
- 并不会延迟用户输入事件的执行。
- 使用`vm.$watch()`来监听msg的变化。
- 原理：核心是setTimeout，只是debounce没有默认的Wait值。

### 5.3.3 number

- 对同步的数据进行数值转型，如果转型结果为NaN，则保留原始值。
- 原理：调用util里面的toNumber方法。

