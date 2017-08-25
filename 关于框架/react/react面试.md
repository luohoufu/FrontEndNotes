* setState之后发生了什么？

  将参数和现在的state进行合并，触发调和过程，重建虚拟dom树，与之前的dom树进行diff算法对比，找到最小的变化，进行UI界面的更新。

* element和component的区别？

  element是个js对象，也就是我们平时说的虚拟节点，component是根据我们传入的参数，生成的对象树，或者说虚拟节点树

* react中的refs

  ```

  ```

* react中的事件处理

  为了解决跨浏览器兼容性问题，React 会将浏览器原生事件（Browser Native Event）封装为合成事件（SyntheticEvent）传入设置的事件处理器中。这里的合成事件提供了与原生事件相同的接口，不过它们屏蔽了底层浏览器的细节差异，保证了行为的一致性。另外有意思的是，React 并没有直接将事件附着到子元素上，而是以**单一事件监听器的方式将所有的事件发送到顶层进行处理**。这样 React 在更新 DOM 的时候就不需要考虑如何去处理附着在 DOM 上的事件监听器，最终达到优化性能的目的。

  ```

  ```

* props.children

  ```
  this.props.children.map() 会有问题，以为children可能又有一个对象，而不是数组，
  因此使用React.children.map()
  React.Children.map(this.props.children, function (child) {
      return <li>{child}</li>;
  })
  ```

* 回调渲染模式

  ```

  ```