# 第12章 表单校验

## 12.1 安装

## 12.2 基本使用

- 校验结果由两部分组成
    - 表单整体校验结果
        - `valid`：所有字段是否通过
        - `invalid`：`valid`取反
        - `touched`：只要有一个校验字段所在元素获得过焦点就返回true
        - `untouched`：`touched`取反
        - `modified`：只要一个字段对应元素值域初始值不同就返回true
        - `dirty`：只要一个校验字段对应元素值改变过至少一次就返回true
        - `pristine`：`dirty`取反
        - `errors`：如果整体校验没通过，则返回错误字段信息数组，否则返回`undefined`

    - 单个字段校验结果
        - `valid`：字段校验是否通过 【true|false】
        - `invalid`：`valid` 取反
        - `touched`：字段所在元素是否获得过焦点，是：true, 否：false
        - `untouched`：`touched`取反
        - `modified`：当元素值与初始值不同时返回true
        - `dirty`：字段值改变过至少一次返回true
        - `pristine`：`dirty`取反
        - `errors`：如果校验没通过，则返回错误字段信息数组，否则返回`undefined`

## 12.4 语法

    v-validate:[:filed]="array literal | object literal | binding"

### 12.4.1 检验字段名field

- 使用v-validate指令进行校验。`<input type="text" v-validate:comment="{minlength: 16, maxlength: 128}">`

### 12.4.2 校验规则定义

1. 数组字面量，如`['String','required']`
2. 对象字面量, 如`{required: true, minlength: 3}`
3. 实例数据属性，可以用来动态绑定校验规则，如`<input type="text" v-validate:id="rules"> // 此处跟实例的data.rules进行绑定`
4. 与terminal 指令同时使用，需要将其包裹在div元素中，否则不能用。

## 12.5 内置验证规则

常用的验证规则：

- `required`：输入值不能为空
- `pattern`：必须匹配pattern表示的正则表达式
- `minlength`：输入值长度不能小于minlength表示的值
- `maxlength`：输入值长度不能小于maxlength表示的值
- `min`：输入值不能小于min表示的值
- `max`：输入值不能大于max表示的值

可用的表单元素

|元素                       |`required`|`minlength`|`maxlength`|`pattern`|`min`|`max`|
|:--------------------------|:---------|:----------|:----------|:--------|:----|:----|
|`input[type="text"]`       |yes       |yes        |yes        |yes      |yes  |yes  |
|`input[type="number"]`     |yes       |yes        |yes        |yes      |yes  |yes  |
|`input[type="password"]`   |yes       |yes        |yes        |yes      |no   |no   |
|`input[type="email"]`      |yes       |yes        |yes        |yes      |no   |no   |
|`input[type="tel"]`        |yes       |yes        |yes        |yes      |no   |no   |
|`input[type="url"]`        |yes       |yes        |yes        |yes      |no   |no   |
|`input[type="checkbox"]`   |yes       |yes        |yes        |no       |no   |no   |
|`input[type="radio"]`      |yes       |no         |no         |no       |no   |no   |
|                           |          |           |           |         |     |     |
|`textarea`                 |yes       |yes        |yes        |yes      |yes  |yes  |
|`select`                   |yes       |yes        |yes        |no       |no   |no   |

## 12.6 与 `v-model` 同时使用：会自动校验通过v-model动态设置的值。
## 12.7 重置校验结果，使用`$resetValidation()`方法动态重置校验结果。
## 12.8 表单元素