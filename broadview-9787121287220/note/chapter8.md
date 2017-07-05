# 第8章 过渡

当插入或者删除带有transition特性的元素时，VueJs执行的操作：

- 尝试以ID 为`my-transition`查找过渡钩子对象，如果有则在不同的阶段触发不同的钩子。
- 自动嗅探目标元素是否有css 过渡或动画。
- 前两步不存在，则在下一帧中立即执行。

## 8.1 CSS过渡

- 添加过渡属性`transition="expand"`
- 添加`.expand-transition`,`.expand-enter`,`.expand-leave`添加CSS规则
- 钩子：
    - beforeEnter
    - enter
    - afterEnter
    - enterCancelled
    - leave
    - afterLeave
    - leaveCancelled
- 默认类型（transitionName = transitionValue || 'v'）
    - `.transitionName-transition`: 始终停留在元素上
    - `.transitionName-enter`: 进入过渡的开始状态，只用一帧，然后立即删除
    - `.transitionName-leave`: 离开过渡的结束状态
- 自定义CSS类名(1.0.14 之后才支持)

    ```javascript
    Vue.transition('transitionName', {
        enterClass: 'otherEnterClass',
        leaveClass: 'otherOutClass'
    });
    ```