```
render: function(nextElement, container, callback) {
	
	//查看组件是否为初次挂载
    var prevComponent = instancesByReactRootID[getReactRootID(container)];

	//不是初次挂载，则查看是否更新
    if (prevComponent) {
      var prevElement = prevComponent._currentElement;
      //利用shouldUpdateReactComponent，查看前后两个virtual DOM是否相同，需要更新
      if (shouldUpdateReactComponent(prevElement, nextElement)) {
        return ReactMount._updateRootComponent(
          prevComponent,
          nextElement,
          container,
          callback
        ).getPublicInstance();
      } else {
        ReactMount.unmountComponentAtNode(container);
      }
    }
    
    
	//首次挂载，实例化
    var reactRootElement = getReactRootElementInContainer(container);
    var containerHasReactMarkup =
      reactRootElement && ReactMount.isRenderedByReact(reactRootElement);

    var shouldReuseMarkup = containerHasReactMarkup && !prevComponent;

    var component = ReactMount._renderNewRootComponent(
      nextElement,
      container,
      shouldReuseMarkup
    ).getPublicInstance();
    if (callback) {
      callback.call(component);
    }
    return component;
  }
```