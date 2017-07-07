# 第11章 组件

我们可以把组件代码按照template、style、script的拆分方式，放置到对应的文件中。

组件核心选项：

- 模板
- 初始数据
- 接收的外部参数(props)，参数默认都是单向的。
- 方法
- 生命周期钩子函数

## 11.1 基础

### 11.1.1 注册

1. 全局注册

    ```javascript
    Vue.component('componentId', Component);
    ```

2. 局部注册

    ```javascript
    var Parent = Vue.extend({
        components: {
            'childComponent': {
                template: '<div>...</div>'
            }
        }
    });
    ```

### 1.1.2 数据传递

三种数据传递方式：

1. props
2. 组件通信
3. slot

#### props

- 组件数据的一个字段
- 组件实例的作用域是孤立的
- 可以是字面量，表达式，绑定修饰符
    - 字面量
        ```javascript
        var child = Vue.extend({
            props: ['myComponent'],
            template: '<div>{{myComponent}}</div>',
            replace: true
        })
        ```

        ```html
        <child my-component="hello!"></child>
        ```
    - 动态语法：用`v-bind` 将动态props绑定到父组件的数据

        ```javascript
        var child = Vue.extend({
            props: ['comeFromParentProps'],
            template: '<div>{{comeFromParentProps}}</div>'
        });

        var Parent = Vue.extend({
            template: '<p>....<child :didi-props="hello"></child></p>',
            data: function() {
                return { 'hello': 'hello,' };
            },
            components: {
                'child': Child
            }
        })
        ```

    - 绑定修饰符
        - .sync
        - .once
        - 如果props是一个对象或者数组，那么他是按引用传递的。
    - props 验证

        ```javascript
        Vue.component('example', {
            props: {
                propA: 'null' // 任何类型都可以
                propB: [String, Number],
                propC: {
                    type: String,   // String, Number, Boolean, Object, Function, Array, function(){  }使用instance
                    required: true
                }
            }
        })
        ```

    - prop 转换函数：coerce函数，VueJs 1.0.12 新增
    - props & propsData & data
        - props 父子组件数据传递
        - data 组件的私有数据存在
        - postData 用来组件初始化后覆盖props中的属性

#### 组件通信

- `this.$parent` 访问父组件
- `this.$children` 访问子组件
- `this.$root` 访问根实例

- 可以在作用域上传递事件
- 每个Vue实例都是一个事件触发器
    - `$on()` 监听事件
    - `$emit()` 把事件沿着作用域向上派送
    - `$dispatch()` 派发事件，事件沿着父链冒泡
    - `$broadcast()` 广播事件，事件向下传导给所有的后代
        - 建议在子组件的定义中触发事件
        - 在父组件中，通过v-on:customEvent="handle" 绑定子组件自定义事件的处理方法（在父组件中实现），从而实现事件的传递。

#### slot 分发内容：用来很合父组件的内容和子组件自己的模板

- 编译作用域
- 单个slot
    - `<slot>`标签的内容视为回退内容。回退内容在子组件的作用域内编译。
- 具名slot
    - 可以用一个特殊特性name配置如何分发内容
    - 仍然可以有一个匿名slot，作为找不到匹配的内容片段的回退插槽
- 原理解析

### 11.1.3 混合

- 混合对象可以包含任意的组件选项
- 同名选项时，这些选项将以适当的策略合并。同名的钩子会并入同一个数组中，因此都会被调用。
- 值为对象的选项，将合并到同一个对象中，如果键值冲突，则组件的选项优先。
- Vue.extend()使用同样的合并策略

### 11.1.4 动态组件

使用一个挂载点，使用`<component>`元素的`is`特性，动态绑定元素。

1. keep-alive: 可以保留它在内存中，可避免重新渲染。

    ```html
    <component :is="currentView" keep-alive></component>
    ```

2. activate 钩子：只作用于动态组件切换或者静态组件初始化渲染的过程中，不作用于使用实例方法手工插入的过程。
3. transition-mode 用于指定两个动态组件如何过渡
    - in-out
    - out-in

## 11.2 相关拓展

### 11.2.1 组件和v-for

1. 组件的作用域是孤立的
2. 使用props将数据传递到组件内部

    ```html
    <component v-for="item in items" :item="item" :index="$index"></component>
    ```
3. 显示声明数据来源可以让组件复用在其他地方

### 11.2.2 编写可复用组件
### 11.2.3 异步组件

- 工厂函数
    - 用来动态解析组件的定义，从而实现组件的按需加载
    - VueJs只会在组件需要渲染的时候出发工厂函数
    - 接收一个resolve回调，在收到从服务器下载的组件定义时调用。

### 11.2.4 资源命名约定
### 12.2.5 内联模板属性（inline-template）

- 内联模板的优先级比template高。
- 内联属性让模板的作用域难以理解，并且不能缓存模板编译结果。

### 12.2.6 片段实例

- 推荐模板的顶级元素始终是单个元素。
- 下面几种情况会让实例变成一个片段实例：
    - 模板包含多个顶级元素
    - 模板只包含普通文本
    - 模板只包含其他组件
    - 模板只包含一个元素指令
    - 模板根节点有一个流程控制指令
- 片段实例没有根节点，他的`$el`指向一个锚节点，即一个空的文本节点。
- 组件元素上的非流程控制指令，非props特性和过渡将被过滤，因为没有根节点。

## 11.3 生命周期

- init：开始初始化
- created：结束解析，事件绑定，计算属性，方法等都已建立，但还没有开始DOM编译，`$el`还不存在。
- beforeCompile
- compiled：所有的指令已生效。但不担保`$el`已插入文档。
- ready
- attached：`vm.$el`插入DOM时调用。
- detached：`v.$el`从DOM中删除时调用。
- beforeDestroy：销毁实例时调用。
- destroyed

## 11.4 开发组件

- 为了避免随着项目增大带来的难于维护，我们开始尝试前端组件化，把view拆分成不同的组件。并为每个组件编写逻辑和样式。

### 11.4.2 基于第三方组件的开发

## 11.5 常见问题解析

1. camelCase & kebab-case

    HTML 标签中的属性名不区分大小写。设置prop名字未camelCase形式的时候，需要转换为kebeb-case形式。

2. 字面量语法 & 动态语法

    ```html
    <comp some-prop="1"></comp> <!--传递一个字符串"1"，字面量语法-->
    <comp :some-prop="1"></comp><!--传递一个数字1，动态语法-->
    ```

3. 组件选项问题

    `data`和`el`这两个选项是不能用在`Vue.extend()`中的。

4. 模板解析

    Vue的模板是DOM模板，使用浏览器原生的解析器而不是自己实现一个。一些HTML元素对内部元素有限制，常见的限制有：

    - a 不能包含其他交互元素，比如按钮，连接。
    - ul 和 ol 只能直接包含li
    - select 只能包含option和optgroup
    - table 只能包含thead, tbody, tfoot, tr, caption, colgroup
    - tr 只能包含td,th

    自定义标签不能用在ul、select、table等对内部元素有限制的标签内。放在这些元素内部的自定义标签将被提到元素的外面，因为渲染不正确。

5. 动态组件与异步组件结合
6. 如何解决数据层次结构太深的问题：使用`vm.$set`的方法。
7. 后端数据交互
    - 在`created`钩子中进行数据的获取
8. data中没有定义计算属性，他是如何被使用的？
    - 通过源码，可以发现是通过`Object.defineProperty(this, key, def)`直接定义到了实例上。
