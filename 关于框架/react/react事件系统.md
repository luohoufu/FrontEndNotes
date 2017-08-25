[参考](https://segmentfault.com/a/1190000008253968)

react实现了一个SyntheticEvent层，所有定义的事件处理器都可以接受到一个SyntheticEvent对象的实例，跨浏览器的原生的包装，和原生事件有同样的接口，包括stopPropagation()和preventDefault()；

## 合成事件的使用方式

react并没有把所有的时间绑定在dom上，而是采用事件代理的方法，绑定在最顶层，事件发生之后有这个监听器处理，随后找到真正的事件处理函数进行调用，因为在系统中，事件处理器越多，占据的内存就越大。react自动帮助开发者提高了性能。

## 使用原生事件

必须使用原生事件的时候：

🌰：

我们要实现这样的一个功能，在页面上有个button，当点击它会出现一个图片。当点击页面的其他部分的时候，这个图片会自动的消失，那么这样的需求我们就不得不使用原生的事件。

```javascript
import React from 'react';

class App extends React.Component {

  constructor(props){
    super(props);
    
    this.state = {
      show: false
    }
    
    this.handleClick = this.handleClick.bind(this)
    this.handleClickImage = this.handleClickImage.bind(this);
  }
  
  handleClick(){
   this.setState({
     show: true
   })
  }
  
  componentDidMount(){
    document.body.addEventListener('click', e=> {
      this.setState({
        show: false
      })
    })
  }
  
  componentWillUnmount(){
    document.body.removeEventListener('click');
  }
    
  render(){
    return (
      <div className="container">
        <button onClick={this.handleClick}>Open Image</button>
          <div className="img-container" style={{ display: this.state.show ? 'block': 'none'}} >
            <img src="http://blog.addthiscdn.com/wp-content/uploads/2014/11/addthis-react-flux-javascript-scaling.png" />
          </div>
      </div>
    )
  }
}
ReactDOM.render(<App />, document.getElementById('root'));

```

点击img，也会消失，加入阻止了冒泡，发现并没有效果，因为

* **合成事件中stopPropagation()无法阻止原生冒泡事件**，
* **原生事件中阻止冒泡就不会触发合成事件**，因为合成事件是依靠原声冒泡事件冒泡到顶层从而

经过同时绑定原生事件和合成事件，去测试，发现是，先原生事件依次冒泡，到顶层组件，之后**合成事件创建**，之后依次冒泡。原因就是react将时间监听放在最顶层，根据这一个去进行时间的响应，所以必须得**原生事件冒泡到最顶层，才会触发合成事件**！

**原生事件注意removeListener**，因为react不会帮你做

