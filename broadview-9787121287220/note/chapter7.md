# 第7章 Class 与 Style 绑定

- 对象语法
    - 尽管可以用Mustache标签绑定class, 比如class="{{className}}"， 但是我们不推荐这种写法和v-bind:class会用

- 数组语法
    - 在1.0.19 版本之后，可以在数组语法中使用对象语法。

- 自动添加前缀
    -  prefix 原理：检测输入的属性是否在浏览器的`element.style`中存在。
    - VueJs只会添加FireFox的-moz-、IE的-ms-、Safari和Chrome 的-webkit- 这三种前缀。
