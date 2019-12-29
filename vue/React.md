#### 函数组件和class组件

 ##### 1.函数组件

~~~javascript
function Welcome(props) {
        return <h1>Hello,{props.name}</h1>
    }
~~~

##### 2.class组件

~~~javascript
class Welcome extends React.Component {
        render() {
            return <h1>Hello,{this.props.name}</h1>
        }
    }
~~~

```
上述两个组件在 React 里是等效的。
```

##### 自定义组件

~~~javascript
const element = <Welcome name="Jackson" />;
~~~

当 React 元素为用户自定义组件时，它会将 JSX 所接收的属性（attributes）转换为单个对象传递给组件，这个对象被称之为 “props”。

例如，这段代码会在页面上渲染 “Hello, Jackson”：

~~~javascript
function Welcome(props) {
    return <h1>Hello,{props.name}</h1>
}
const element = <Welcome name="Jackson" />;
ReactDOM.render(
    element,
    document.getElementById('root')
)
~~~

1. 我们调用 `ReactDOM.render()` 函数，并传入 `<Welcome name="Sara" />` 作为参数。
2. React 调用 `Welcome` 组件，并将 `{name: 'Sara'}` 作为 props 传入。
3. `Welcome` 组件将 `<h1>Hello, Sara</h1>` 元素作为返回值。
4. React DOM 将 DOM 高效地更新为 `<h1>Hello, Sara</h1>`

##### 组合组件

~~~javascript
 function Welcome(props) {
        return <h1>Hello,{props.name}</h1>
    }
    function App() {
        return (
            <div>
                <Welcome name="Jackson1" />
                <Welcome name="Jackson2" />
                <Welcome name="Jackson3" />
            </div>
        )
    }
    ReactDOM.render(
        <App />,
        document.getElementById('root')
    );
~~~

页面中渲染 “Hello, Jackson1  Hello, Jackson2   Hello, Jackson3”.

##### 提取组件

详情：https://react.docschina.org/docs/components-and-props.html

~~~javascript
   function Comment(props) {
        return (
          <div className="Comment">
              <UserInfo user={props.author}/>
              <div className="Comment-text">
                  {props.text}
              </div>
              <div className="Comment-date">
                  {formatName(props.date)}
              </div>
          </div>
        );
    }
    //首先，提取Avatar组件
    function Avatar(props) {
        return (
            <img className="Avatar"
                 src={props.user.avatarUrl}
                 alt={props.user.name}
            />
        )
    }
    //然后，提取UserInfo组件
    function UserInfo(props) {
        return (
            <div className="UserInfo">
                <Avatar user={props.user}/>
                <div className="UserInfo-name">
                    {props.user.name}
                </div>
            </div>
        )
    }
~~~

**组件无论是使用函数声明还是通过 class 声明，都决不能修改自身的 props。**

#### State和生命周期

详情：https://react.docschina.org/docs/state-and-lifecycle.html

##### 将函数组件转换为class组件

通过以下五步将 `Clock` 的函数组件转成 class 组件：

1. 创建一个同名的 ES6，并且继承于 `React.Component`。
2. 添加一个空的 `render()` 方法。
3. 将函数体移动到 `render()` 方法之中。
4. 在 `render()` 方法中使用 `this.props` 替换 `props`。
5. 删除剩余的空函数声明。

##### 向class组件中添加局部state 

我们通过以下三步将 `date` 从 props 移动到 state 中：

1. 把 `render()` 方法中的 `this.props.date` 替换成 `this.state.date` ：
2. 添加一个 class 构造函数，然后在该函数中为 `this.state` 赋初值：
3. 移除 `<Clock />` 元素中的 `date` 属性：

##### 将生命周期方法添加到class中

当 `Clock` 组件第一次被渲染到 DOM 中的时候，就为其设置一个计时器。这在 React 中被称为“挂载（mount）”。

同时，当 DOM 中 `Clock` 组件被删除的时候，应该清除计时器。这在 React 中被称为“卸载（unmount）”。

这些方法叫做“生命周期方法”。

`componentDidMount()` 方法会在组件已经被渲染到 DOM 中后运行，

`componentWillUnmount()`方法会在组件被删除时执行

#### React

视图层框架

* 一个构建用户界面的框架
* 声明式的框架
* 数据驱动DOM，再用事件反馈给数据

组件化开发

* 组件组合而不是继承
* state&&props
* 生命周期

JSX语法

* 一种JS扩展的表达式
* 带有逻辑的标记语法，有别于HTML模板
* 对样式，逻辑表达式和事件的支持

虚拟DOM

* 对DOM进行模拟
* 比较操作前后的数据差异
* 如果有数据差异，统一操作DOM

优点：简洁、灵活、高效

##### JSX语法

* 基本语法

  ~~~react
  let style = {
      color: 'red',
      fontSize: '24px'
  };
  ReactDOM.render(
      (
          <div className="jsx" style="style">
              <p>Hello React</p>
          </div>
      ),
      document.getElementById('app')
  );
  ~~~

* 数据逻辑处理

  ~~~react
  let name = "Jackson";
  let flag = false;
  let names = ['Rose','Geely','Jimin'];
  <div className="jsx">
      {/*变量使用*/}
      <p>Hello React {name}</p>
      {/*条件判断*/}
      {
      flag ? <p>Hello React {name}</p>: <p>Hello React is {name}</p>
      }
      {/*数组循环*/}
      {
      names.map((name,index) => <p key={index}>Hello, I am {name}</p>)
      }
  </div>
  ~~~

##### React组件

* 组件基本结构

  ~~~react
  function Component(){
      return <p>Hello React111</p>
  }
  
  class ES6Components extends React.Component{
      render() {
          return <h1>I am Jackson</h1>
      }
  }
  
  ReactDOM.render(
      <div>
          <Component/><ES6Components/>
      </div>,
      document.getElementById('app')
  );
  ~~~

* state&&props

  ~~~react
  class Component extends React.Component{
      constructor(props){
          super(props);
          this.state = {
              name: 'Jackson1'
          }
      }
      render() {
          setTimeout(() => {
              this.setState({
                  name: 'Jackson test'
              })
          },2000);
          return (
              <div>
                  <h1>I am {this.state.name}</h1>
                  <h1>I am {this.props.name}</h1>
              </div>
          )
      }
  }
  
  ReactDOM.render(
      <div>
          <Component name="Rosen 123"/>
      </div>,
      document.getElementById('app')
  );
  ~~~

* 事件处理

  ~~~react
  //事件处理方式1
  class Component extends React.Component{
      constructor(props){
          super(props);
          this.state = {
              name: 'Jackson',
              age: 18
          };
          this.handleClick = this.handleClick.bind(this);
      }
      handleClick(){
          this.setState({
              age: this.state.age + 1
          })
      }
      render() {
          return (
              <div>
                  <h1>I am {this.state.name}</h1>
                  <p>I am {this.state.age} years old</p>
                  <button onClick={this.handleClick}>加一岁</button>
              </div>
          )
      }
  }
  //事件处理方式2---箭头函数处理作用域
  class Component extends React.Component{
      constructor(props){
          super(props);
          this.state = {
              name: 'Jackson',
              age: 18
          };
      }
      handleClick(){
          this.setState({
              age: this.state.age + 1
          })
      }
      handelChange(e){
          this.setState({
              age: e.target.value
          })
      }
      render() {
          return (
              <div>
                  <h1>I am {this.state.name}</h1>
                  <p>I am {this.state.age} years old</p>
                  <button onClick={(e) => {this.handleClick(e)}}>加一岁</button>
                  <input type="text" onChange={(e) => {this.handelChange(e)}}/>
              </div>
          )
      }
  }
  ~~~

* 组件的组合方式

  ~~~react
  class Title extends React.Component{
      constructor(props){
          super(props);
      }
      render(props) {
          return (
              <div>
                  <h1>{this.props.title}</h1>
                  <h1>{this.props.children}</h1>
              </div>
          )
      }
  }
  class App extends React.Component{
      render() {
          return (
              <div className="">
                  {/*容器组件*/}
                  <Title title="App test">
                      <span>App part 1</span>
                      <a href="#">Link</a>
                  </Title>
                  <hr/>
                  {/*单纯组件*/}
                  <Component/>
              </div>
          );
      }
  }
  ~~~

* 组件间的数据通信

  * 父--->子(props)

    ~~~
    super(props);
    ~~~

  * 子--->父

    ~~~react
    class Child extends React.Component{
        constructor(props){
            super(props);
        }
        handleClick(){
            this.props.changeColor('red');
        }
        render() {
            return (
                <div>
                    <h1>父组件的背景色：{this.props.bgColor}</h1>
                    <button onClick={(e) => {this.handleClick(e)}}>改变父组件颜色</button>
                </div>
            )
        }
    }
    class Father extends React.Component{
        constructor(props){
            super(props);
            this.state = {
                bgColor: '#999'
            }
        }
        onBgColorChange(color){
            this.setState({
                bgColor: color
            })
        }
        render(props) {
            return (
                <div  style={{background:this.state.bgColor}}>
                    <Child bgColor={this.state.bgColor} changeColor={(color) => {this.onBgColorChange(color)}}/>
                </div>
            )
        }
    }
    ~~~

  * 子1--->子2(子1先将数据传给父组件，父组件再将修改的数据给子2)

    ~~~react
    class Child1 extends React.Component{
        constructor(props){
            super(props);
        }
        handleClick(){
            this.props.changeChild2Color('red');
        }
        render() {
            return (
                <div style={{background: this.props.bgColor}}>
                    <h1>Child1：{this.props.bgColor}</h1>
                    <button onClick={(e) => {this.handleClick(e)}}>改变Child2颜色</button>
                </div>
            )
        }
    }
    class Child2 extends React.Component{
        constructor(props){
            super(props);
        }
        render() {
            return (
                <div style={{background: this.props.bgColor}}>
                    <h1>Child2：{this.props.bgColor}</h1>
                </div>
            )
        }
    }
    class Father extends React.Component{
        constructor(props){
            super(props);
            this.state = {
                bgColor: '#999'
            }
        }
        onBgColorChange(color){
            this.setState({
                bgColor: color
            })
        }
        render(props) {
            return (
                <div>
                    <Child1 changeChild2Color={(color) => {this.onBgColorChange(color)}}/>
                    <Child2 bgColor={this.state.bgColor}/>
                </div>
            )
        }
    }
    ~~~

##### 生命周期

* Mounting: 挂载阶段

* Updating: 运行时阶段

* Unmounting: 卸载阶段

* Error Handling: 错误处理

  ~~~react
  class Component extends React.Component{
      //构造函数
      constructor(props){
          super(props);
          this.state= {
             data: "old state"
          };
          console.log('初始化数据','constructor');
      }
      //组件将要加载
      componentWillMount() {
          console.log('组件将要加载','componentWillMount');
      }
      //组件加载完成
      componentDidMount() {
          console.log('组件加载完成','componentDidMount');
      }
      //将要接收父组件传来的props
      componentWillReceiveProps() {
          console.log('将要接收父组件传来的props','componentWillReceiveProps');
      }
      //子组件是不是应该更新
      shouldComponentUpdate() {
          let flag = true;
          console.log('子组件是不是应该更新',flag,'shouldComponentUpdate');
          return flag;
      }
      //组件将要更新
      componentWillUpdate() {
          console.log('组件将要更新','componentWillUpdate');
      }
      //组件更新完成
      componentDidUpdate() {
          console.log('组件更新完成','componentDidUpdate');
      }
      //组件将要销毁
      componentWillUnmount() {
          console.log('组件将要销毁','componentWillUnmount');
      }
      //处理点击事件
      handleClick(){
          console.log("这里是更新数据");
          this.setState({
              data: "new state"
          })
      }
      //渲染
      render() {
          console.log('渲染','render');
          return (
              <div>
                  <div>{this.state.data}</div>
                  <button onClick={()=>{this.handleClick()}}>更新组件</button>
                  <div>{this.props.data}</div>
              </div>
          );
      }
  }
  class App extends React.Component{
      constructor(props){
          super(props);
          this.state= {
              data: "father state",
              hasChild: true
          };
      }
      handleClick(){
          console.log("这里是更新props");
          this.setState({
              data: "new father state"
          })
      }
      onDestoryChild(){
          console.log("这里是删除子组件");
          this.setState({
              hasChild: false
          })
      }
      render() {
          return (
              <div>
                  {
                      this.state.hasChild?<Component data={this.state.data}/>:null
                  }
                  <button onClick={()=>{this.handleClick()}}>改变props</button>
                  <button onClick={()=>{this.onDestoryChild()}}>删除子组件</button>
              </div>
          );
      }
  }
  ReactDOM.render(
      <App/>,
      document.getElementById('app')
  );
  ~~~

  ~~~
  ## 默认输出：
  初始化数据 constructor
  组件将要加载 componentWillMount
  渲染 render
  组件加载完成 componentDidMount
  ## 点击更新组件输出：
  这里是更新数据
  子组件是不是应该更新 true shouldComponentUpdate
  组件将要更新 componentWillUpdate
  渲染 render
  组件更新完成 componentDidUpdate
  ## 点击改变props输出：
  这里是更新props
  将要接收父组件传来的props componentWillReceiveProps
  子组件是不是应该更新 true shouldComponentUpdate
  组件将要更新 componentWillUpdate
  渲染 render
  组件更新完成 componentDidUpdate
  ## 点击删除子组件输出：
  这里是删除子组件
  app.js:10290 组件将要销毁 componentWillUnmount
  ~~~

##### Router

* 普通路由

~~~
//页面路由
window.location.href = 'http://www.baidu.com';
history.back();

// hash 路由
window.location = '#hash';
window.onhashchange = function () {
  console.log('current hash:',window.location.hash);
};

// h5 路由
// 推进一个状态
history.pushState('name','title','/path');
// 替换一个状态
history.replaceState('name','title','/path');
// popstate
window.onpopstate = function () {
    console.log(window.location.href);
    console.log(window.location.pathname);
    console.log(window.location.hash);
    console.log(window.location.search);
};
~~~

* React-Router

  ~~~
  React官方提供的路由插件，单页面应用必备
  版本：react-router-dom@v4.2.2
  动态路由，纯react组件
  安装： yarn add react-router-dom@4.2.2
  ~~~

  常用组件：

  * 路由方式：`<BrowserRouter>/<HashRouter>`

  * 路由规则：`<Router>`

  * 路由选项：`<Switch>`

  * 跳转导航：`<Link>/<NavLink>`

  * 自动跳转：`<Redirect>`

    ~~~react
    import { BrowserRouter as Router, Route, Link, Switch} from 'react-router-dom'
    
    class A extends React.Component{
        constructor(props) {
            super(props);
        }
        render() {
            return (
                <div>
                    Component A
                    <Switch>
                        <Route exact path={`${this.props.match.path}`}
                            render={(route) => {
                                return <div>当前组件是不带参数的A</div>
                            }}/>
                        <Route path={`${this.props.match.path}/sub`}
                            render={(route) => {
                                return <div>当前组件是sub</div>
                            }}/>
                        <Route path={`${this.props.match.path}/:id`}
                            render={(route) => {
                                return <div>当前组件是带参数的A，参数是： {route.match.params.id}</div>
                            }}/>
                    </Switch>
                </div>
            )
        }
    }
    class B extends React.Component{
        constructor(props) {
            super(props);
        }
        render() {
            return (
                <div>Component B</div>
            )
        }
    }
    class Wrapper extends React.Component{
        constructor(props) {
            super(props);
        }
        render() {
            return (
                <div>
                    <Link to="/a">组件A</Link>
                    <br/>
                    <Link to="/a/123">带参数的组件A</Link>
                    <br/>
                    <Link to="/a/sub">sub</Link>
                    <br/>
                    <Link to="/b">组件B</Link>
                   {this.props.children}
                </div>
            )
        }
    }
    ReactDOM.render(
        <Router>
            <Wrapper>
                <Route path='/a' component={A}/>
                <Route path='/b' component={B}/>
            </Wrapper>
        </Router>,
        document.getElementById('app')
    );
    ~~~

##### React数据管理



