# 第三章 指令

## 3.1 内部指令

### 3.1.1 `v-if`

- 包装元素 `<template></template>`
- 切换`v-if`模块时，有一个局部的**编译/卸载**的过程，真实的条件渲染，涉及子组件、事件监听器。
- 是**惰性**的，只会在条件第一个变为真时才会进行编译（会缓存起来）。

### 3.1.2 `v-show`

- **`v-show`不支持`<template>语法`**

- 基于css切换，元素始终被编译和保留。

### 3.1.3 `v-else`

- 尽量搭配`v-if`使用。
- `v-show`可以使用表达式进行`v-else`的同意转述。

    ```html
    <custom-component v-show="condition"></custom-component>
    <p v-show="!condition"></p>
    ```

### 3.1.4 `v-model`

- 用来创建双向数据绑定。
- 支持参数
    - number：自动进行类型转换，NaN，则返回原值
    - lazy：将数据刷新放到目标target的change事件中
    - debounce：延时同步

### 3.1.5 `v-for`

- 支持 `in`、`of` 分隔符
- 得到一个特殊的作用域，需要明确指定props属性（`:item`，`:index`）传递数据
- 我们应该尽量避免直接设置数据绑定的数组元素，因为这些变化不会被VueJs检测到，也不会更新视图渲染。可以使用`$set`方法。

    ```javascript
    // 例如 demo.item[0] = ... 不会触发视图的更新
    demo.items.$set(0, { childMsg: 'Changed!'});
    ```

- 默认情况下，v-for 通过数据对象的特征决定对已有作用域和DOM元素的复用程度。某些情况下我们可能需要使用全新的对象替换数
  组。而这可能导致重新渲染整个列表。如果每一个对象有一个唯一的ID，那么使用`track-by`可以让VueJs尽可能的复用已有实例。
- track-by的弊端：
    - DOM 节点不在映射数组元素顺序的改变，不能同步临时状态（比如`<input>`元素的值，以及组件的私有状态）。
- 使用下面的方式对数组进行变更VueJs是没法检测到变化的：
    - 直接使用索引设置元素，如`vm.items[0] = {}`。 可以使用`$set`方法来实现监听。
    - 修改数据的长度，如`vm.items.length = 0`。可以使用新的items来替换。
- 与内置过滤器或排序数据一起使用

    |名字    |语法                                |版本支持|示例                                                                                             |
    |:-------|:-----------------------------------|:-------|:------------------------------------------------------------------------------------------------|
    |filterBy| filterBy searchKey [in dataKey...] | 0.12   | `<input v-model="searchText" /><li v-for="user in users | filterBy searchText in 'name'"></li>` |
    |orderBy | orderBy sortKey[reverseKey]        | 0.12   | `<li v-for="user in users | orderBy field reverse"></li>`                                       |

### 3.1.6 `v-text`

- 用来更新元素的textContent。

### 3.1.7 `v-html`

- `v-html`质量可以更新元素的innerHTML。
- 会忽略数据绑定。
- 不建议在网站上直接动态渲染任意的HTML片段，很容易导致XSS攻击。

### 3.1.8 `v-bind`

- 响应更新HTML特性
- 可以简写成`:`
- 绑定class和style时，支持其他类型的值，如数组或对象（注意，v-bind后边跟有参数class）

    ```html
    <div v-bind:class="[classA, {classB: isB, classC: isC}]"></div>
    ```

- 没有参数时，可以绑定到一个对象。 注意此时class和style绑定暂不支持数组和对象(v-bind后边没有参数，不支持数组和对象是因为对象key会被转换成小写)

    ```html
    <div v-bind="{id: someProp, 'OTHERAttr': otherProp}"></div>
    ```

- 在绑定prop时，必须在子组件中声明prop。不同的修饰符可以指定不同的绑定类型：
    - .sync：双向绑定，只能作用于prop绑定
    - .once：单次绑定，只能用于prop绑定
    - .camel：将绑定的特性名字转换回驼峰命名。只能用于HTML特性的绑定。

### 3.1.9 `v-on`

- 绑定事件监听器。
- 普通元素只能监听原生DOM事件，`$event`可以获取DOM event事件
- 自定义元素组件上时，可以出发自定义的事件，`$arguments`属性获取子组件`$emit`回调的参数
- 修饰符
    - .stop：`event.stopPropagation()`
    - .prevent：`event.preventDefault()`
    - .capture：使用capture模式添加事件监听器。
    - .self：只当事件从侦听器的元素本身触发时才触发回调。
    - .{keyCode | keyAlias}：只在指定按键上触发回调。

### 3.1.10 `v-ref`

- 在父组件上注册一个子组件的引用，便于直接访问。必须提供参数ID
- 使用`v-ref:some-ref`设置`this.$refs.someRef`，从而实现驼峰法的支持

### 3.1.11 `v-el`

- 为DOM元素注册一个索引。通过$els访问这个元素。

### 3.1.12 `v-pre`

- 编译时跳过当前元素和他的子元素。

### 3.1.13 `v-clock`

- 保持在元素上直到关联实例结束编译。
- 搭配`[v-clock]{display:none;}`这个指令可以隐藏未变异的Mustache标签直到实例准备结束。否则渲染页面时，有可能会先看到Mustache标签。

## 3.2 自定义指令

### 3.2.1 基础

- 使用`Vue.directive(id, definition)`方法注册一个全局自定义指令。

#### 钩子函数

- `bind`：只调用一次，在指令第一次绑定到元素上时调用。
- `update`：在bind之后立即以初始值为参数第一次调用。之后每当绑定值发生变化时调用。
- `unbind`：只调用一次，在指令从元素上解绑时调用。

#### 指令实例属性

- el：绑定的元素
- vm：拥有该指令的上下文ViewModel
- expression：表达式，不包括参数和过滤器
- arg：指令的参数
- name：指令的名字，不包含前缀
- modifiers：对象，包含指令的修饰符
- descriptor: 对象，包含指令的解析结果

#### 对象字面量

- 指令可以使用任意合法的JavaScript表达式

#### 字面修饰符

- 当指令使用了字面修饰符时，他的值将按照普通字符串处理并传递给update方法。
- update方法只会调用一次，因为普通字符串不能响应数据变化。

#### 元素指令

- 可以看作是一个轻量组件
- 不接受参数或者表达式
- 是终结性的，这意味着一旦Vue遇到一个元素指令，他将跳过该元素机器子元素。
- 只有该元素指令本身可以操作该元素及其子元素。

### 3.2.2 高级选项

自定义指令提供了一种机制将数据的变化映射到DOM行为上。

#### params

- 一个特性数组
- 编译器自动提取绑定元素的这些特性
- 此API也支持动态属性。`this.params[key]`会自动保持更新，可以指定回调在值发生变化时调用。

#### deep

- 当自定义指令作用在一个对象上时，对象内部属性发生变化时，会触发update。则在指令定义对象中指定`deep:true`。

#### twoWay

- `twoWay:true`允许在指令中使用`this.set(value)`来想Vue实例中写回数据。

#### acceptStatement

- 可以让自定义指令接收内联语句

#### Terminal(^@1.0.19)

- Vue 通过递归遍历DOM树来编译模块。
- 遇到terminal指令时会停止遍历这个元素的后代元素，这个指令将接管这个元素和他的后代元素的编译任务
- v-if, v-for 都是terminal指令。

#### priority

- 可以给指令指定优先级
- 默认为1000
- terminal指令的默认优先级为2000
- 流程控制指令 v-if 和 v-for 在编译过程始终拥有最高的优先级。

## 3.3 内部指令解析

## 3.4 常见问题解析