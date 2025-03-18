### 问题
React 16 之前的版本比对更新 VirtualDOM 的过程是采用循环加递归实现的(Stack算法)，这种比对方式有一个问题，就是一旦任务开始进行就无法中断，如果应用中组件数量庞大，主线程被长期占用，直到整棵 VirtualDOM 树比对更新完成之后主线程才能被释放，主线程才能执行其他任务。这就会导致一些用户交互，动画等任务无法立即得到执行，页面就会产生卡顿, 非常的影响用户体验。

核心问题：递归无法中断，执行重任务耗时长。 JavaScript 又是单线程，无法同时执行其他任务，导致任务延迟页面卡顿，用户体验差。

#### Fiber是一种DOM比对算法的名字，以前（React 16 之前）的算法叫做Stack

#### 解决方案

1. 利用浏览器空闲时间执行任务，拒绝长时间占用主线程（如果有高优先级的任务，会先中断DOM对比，先执行高优先级任务）
2. 放弃递归只采用循环，因为循环可以被中断（因为递归不可中断，只能采取循环，把循环条件保存下来，下次可在中断的地方继续执行）
3. 任务拆分，将任务拆分成一个个的小任务（即使任务被中止，下次继续也不会重新渲染，会从中止地方继续执行，这样对比执行成本会降低，以前是把整个VDOM树对比看成任务，现在把树中的节点对比看成任务，那么大的任务就被成一个个的小任务了）

#### requestIdleCallback （核心 API 功能）介绍

利用浏览器的空余时间执行任务，如果有更高优先级的任务要执行时，当前执行的任务可以被终止，优先执行高级别任务。

```javascript
requestIdleCallback(function(deadline) {
  // deadline.timeRemaining() 获取浏览器的空余时间
})
```

#### 浏览器空余时间

页面是一帧一帧绘制出来的，当每秒绘制的帧数达到 60 时，页面是流畅的，小于这个值时， 用户会感觉到卡顿

1s 60帧，每一帧分到的时间是 1000/60 ≈ 16 ms，如果每一帧执行的时间小于16ms，就说明浏览器有空余时间

如果任务在剩余的时间内没有完成则会停止任务执行，继续优先执行主任务，也就是说 requestIdleCallback 总是利用浏览器的空余时间执行任务


### 实现思路

在 Fiber 方案中，为了实现任务的终止再继续，DOM比对算法被分成了两部分：

1. 构建 Fiber        (可中断)
2. 提交 Commit   (不可中断)

DOM 初始渲染: virtualDOM -> Fiber -> Fiber[] -> DOM

DOM 更新操作: newFiber vs oldFiber -> Fiber[] -> DOM

具体思路：JSX 语法通过 Babel 转化为 React.createElement 方法的调用，React.createElement 方法的调用后返回VDOM对象，采用循环方式从这个VDOM对象中为其下的每个 VDOM 对象创建 Fiber 对象，当所有节点的 Fiber 对象创建完成后，把它们存储在数组中，（接下来进行阶段二）循环该数组，根据当前 Fiber 节点的操作类型，把这个操作应用在真实DOM中。由于把所有节点的Fiber对象都存储在数组中，当对于把节点之间的关系抹平了，不清楚谁是谁的父级节点，谁是谁的子级节点了，所以 Fiber对象 还存储其父级节点和子级节点信息，这样才能根据这些信息去构建完整的真实的DOM树。

#### Fiber 对象

```
{
  type         节点类型 (元素, 文本, 组件)(具体的类型)
  props        节点属性
  stateNode    节点 DOM 对象 | 组件实例对象
  tag          节点标记 (对具体类型的分类 hostRoot || hostComponent || classComponent || functionComponent)
  effects      数组, 存储需要更改的 fiber 对象
  effectTag    当前 Fiber 要被执行的操作 (新增, 删除, 修改)
  parent       当前 Fiber 的父级 Fiber
  child        当前 Fiber 的子级 Fiber
  sibling      当前 Fiber 的下一个兄弟 Fiber
  alternate    Fiber 备份 fiber 比对时使用
}
```
