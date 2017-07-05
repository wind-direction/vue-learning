# 第四章 计算属性

## 4.1 什么是计算属性？

- 就是当其依赖属性的值发生变化时，这个属性的值会自动更新，与之相关的DOM部分也会同步自动更新。
- 通过`getter`,`setter`进行计算属性的自动刷新。

## 4.2 计算属性缓存

- 在0.12.8 版本开始，默认提供了缓存开关。在计算属性对象中指定cache字段来控制是否开启缓存。

## 4.3 其他问题

1. 计算属性getter不执行的场景

    当包含计算属性的节点被移除并且模板中其他地方没有再引用该属性时，那么对应的计算属性的getter方法不会执行。

2. 在 v-repeat 中使用计算属性