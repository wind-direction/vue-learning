# 第9章 Method

## 9.1 如何绑定事件

- 内联方式
- methods配置
    - methods中定义的方法内的this始终指向创建的Vue实例
    - 与事件绑定的方法支持参数event即原生DOM事件的传入
    - 普通元素，只能监听原生DOM事件
    - 自定义元素，还能监听子组件触发的自定义事件
- `$events` 应用

    ```html
    <button v-on:click="say('hello!', $event)"></button>
    ```

## 9.2 如何使用修饰符

- prevent
- stop
- capture
- self