# react-redux

### 本文只要是介绍了 react 中的 redux 管理数据的状态123

##### 1. 准备工作
```
// 我们在开始的时候需要下载一个 react 官方的 脚手架 或者 自己手动搭建一个 webpack 脚手架
// 脚手架 create-react-app [文件夹的名字]
// 示例 ： create-react-app my-app

// weback 见我的 webpack 篇
```

##### 2. 下载 react-redux redux 两个包
```
// 可直接复制以下代码
cnpm i react-redux redux
```

##### 3. 在 index.js 中引入 
```
// import { Provider } from 'react-redux'
// Provider 是一个组件， 它需要放在最外侧
// 示例： 
import { Provider } from "react-redux"
import store from "./store"

ReactDOM.render(
    <Provider store={store}>
        <Router>
            <App />
        </Router>
    </Provider>,
    document.getElementById('root')
);
```

##### 4. store 文件
```
// 引入 createStore 方法 用于创建 store
import { createStore } from 'redux'
import reducer from './reducer'

const store = createStore(reducer)

export default store
```

##### 5. reducer 文件
```
import { combineReducers } from 'redux'
import classify from './classify'

// 在这里进行了合并 reducer 使得多个 reducer 成为对象的 value 综合到一起 最后输出一个 reducer 

const reducer = combineReducers({
    classify
})

export default reducer
```

##### 6. classify 文件
```
// 每一个单独的 reducer 文件
const classify = (state = {name: 'classIfy', data: null}, action) => {
    switch(action.type){
        case 'GET_OFFSETHEIGHT':
            return {...state, data: action.data}
        default: 
            return state
    }
}

export default classify
```

##### 7. 在组件内
```
import { connect } from "react-redux"

// 在组建内链接
export default connect(null, null, null, { pure: false })(ClassIfy)

// 如果只需要第四个参数的话 ， 前面几个参数都可以传 null
// connect([mapStateToProps], [mapDispatchToProps], [mergeProps], [options])
export default connect(mapStateToProps, mapDispatchToProps, mergeProps, [option])(ClassIfy)

// connect 中有四个参数
1. mapStateToprops : [ mapStateToProps(state, [ownProps]): stateProps]
功能: 如果指定了此参数，则新组件将订阅Redux商店更新。这意味着，任何时候商店更新，mapStateToProps将被调用。结果mapStateToProps必须是一个普通的对象，它将被合并到组件的道具中。如果您不想订阅商店更新，请通过null或undefined取代mapStateToProps。
如果你的mapStateToProps函数被声明为带有两个参数，它将被称为商店状态作为第一个参数，并且道具被传递到连接的组件作为第二个参数，并且只要连接的组件接收到新的道具就会被重新调用由浅层平等比较确定。（第二个参数通常被称为ownProps惯例。）

注意：在需要更多控制渲染性能的高级方案中，mapStateToProps()也可以返回一个函数。在这种情况下，该功能将用于mapStateToProps()特定组件实例。这使您可以执行每个实例的记忆。您可以参考＃279以及它添加的测试以获取更多详细信息。大多数应用程序不需要这样

的mapStateToProps函数的第一个参数是整个Redux的商店的状态，并将其返回作为道具要传递的对象。它通常被称为选择器。使用重新选择有效地组合选择器并计算派生数据。

// 示例： 


2. mapDispatchToprops : [ mapDispatchToProps(dispatch, [ownProps]): dispatchProps] 
对象或函数: 如果一个对象被传递，它内部的每个函数被认为是一个Redux动作的创建者。一个具有相同函数名称的对象，但每个动作创建者都被包装成一个dispatch调用，以便它们可以直接调用，将​​被合并到组件的道具中。

如果传递一个函数，它将dispatch作为第一个参数给出。这取决于你以某种方式返回一个dispatch以你自己的方式绑定动作创建者的对象。（提示：您可以使用bindActionCreators()Redux 的助手。）

如果你的mapDispatchToProps函数被声明为接受两个参数，那么它将被dispatch作为第一个参数调用，并且作为第二个参数传给连接组件的道具被调用，并且只要连接的组件接收到新的道具就会被重新调用。（第二个参数通常被称为ownProps惯例。）

如果你没有提供你自己的mapDispatchToProps功能或对象完整的行动创造者，默认mapDispatchToProps实现只是注入dispatch到你的组件的道具。

注意：在需要更多控制渲染性能的高级方案中，mapDispatchToProps()也可以返回一个函数。在这种情况下，该功能将用于mapDispatchToProps()特定组件实例。这使您可以执行每个实例的记忆。您可以参考＃279以及它添加的测试以获取更多详细信息。大多数应用程序不需要这样


3. mergeProps : [ mergeProps(stateProps, dispatchProps, ownProps): props]
功能： 如果指定了，它被传递的结果mapStateToProps()，mapDispatchToProps()和母体props。您从它返回的普通对象将作为道具传递给包装组件。你可以指定这个函数来根据道具选择一个状态片段，或者将动作创建者绑定到道具上的特定变量。如果您省略，Object.assign({}, ownProps, stateProps, dispatchProps)则默认使用。

4. [option] object 如果指定，则进一步自定义连接器的行为。除了可以使用的选项connectAdvanced()（请参阅下面的选项）之外，还connect()可以接受以下其他选项

[ pure] （布尔值）：如果为真，connect()将避免重新渲染和调用mapStateToProps，mapDispatchToProps和mergeProps如果相关状态/道具对象基于其各自的相等检查保持相等。假定包装组件是一个“纯粹”组件，并且不依赖于除道具和所选Redux商店状态以外的任何输入或状态。默认值：true
[ areStatesEqual] （功能）：纯粹时，将进入的存储状态与先前的值进行比较。默认值：strictEqual (===)
[ areOwnPropsEqual] （功能）：纯粹时，比较传入的道具和前一个值。默认值：shallowEqual
[ areStatePropsEqual] （函数）：纯粹时，将结果与mapStateToProps之前的值进行比较。默认值：shallowEqual
[ areMergedPropsEqual] （函数）：纯粹时，将结果与mergeProps之前的值进行比较。默认值：shallowEqual
[ storeKey] （String）：从何处读取商店的上下文的关键字。如果您处于拥有多个商店的不可取的位置，您可能只需要这些。默认值：'store'

// options.pure为true时优化连接
当options.pure为真时，connect执行了用于避免不必要的呼叫数相等检查mapStateToProps，mapDispatchToProps，mergeProps，最终以render。这些措施包括areStatesEqual，areOwnPropsEqual，areStatePropsEqual，和areMergedPropsEqual
```
