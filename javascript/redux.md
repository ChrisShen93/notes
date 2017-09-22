# Redux

标签（空格分隔）： JavaScript

---

## 核心调用关系

```javascript
store.dispatch(action) --> reducer(state, action) --> final state
```

## 最简例子

```javascript
// reducer()
// @param state 当前的state
// @param action 当前触发的行为 {type: 'xxx'}
// @return 新的state
var reducer = function(state, action) {
    switch (action.type) {
        case 'add_todo':
            return state.concat(action.text);
        default:
            return state;
    }
}

// 创建store
// @param reducer 用来修改state
// @param []， 默认的state值，不传则为undefined
var store = redux.createStore(reducer, []);

// 获取当前store的状态
store.getState();

// 分发action，通过reducer修改store的状态
store.dispatch({type: 'add_todo', text: '读书'})
```

## actionCreator

```javascript
// actionCreator(args) => action

var addTodo = function(text) {
    return {
        type: 'add_todo',
        text: text
    };
};

store.dispatch(addTodo('test'));
```


---

## 一个完整的例子

```javascript
var Redux = require('redux');
var React = require('react');
var ReactDOM = require('react-dom');

// action creator
var addTodoActions = function(text){
    return {
        type: 'add_todo',
        text: text
    };
};

// reducers
var todoReducer = function(state, action){

    if(typeof state === 'undefined'){
        return [];
    }

    switch(action.type){
        case 'add_todo':
            return state.slice(0).concat({
                text: action.text,
                completed: false
            });
            break;
        default:
            return state;
    }
};


var store = Redux.createStore(todoReducer);

// 在getInitialState里：通过store.getState()获取数据进行初始的渲染。
// 在componentDidMount里：监听store的状态变化，当状态变化时，触发onChange回调。
// 在handleAdd里：通过store.dispatch(addTodoActions(value))修改state。（步骤二对这个进行了监听）
// 在onChange里：获取最新的state，并重新渲染视图
var App = React.createClass({
    getInitialState: function(){
        return {
            items: store.getState()
        };
    },
    componentDidMount: function(){
        var unsubscribe = store.subscribe(this.onChange);
    },
    onChange: function(){
        this.setState({
            items: store.getState()
        });
    },
    handleAdd: function(){
        var input = ReactDOM.findDOMNode(this.refs.todo);
        var value = input.value.trim();

        if(value)
            store.dispatch(addTodoActions(value));

        input.value = '';
    },
    render: function(){
        return (
            <div>
                <input ref="todo" type="text" placeholder="输入todo项" style={{marginRight:'10px'}} />
                <button onClick={this.handleAdd}>点击添加</button>
                <ul>
                    {this.state.items.map(function(item){
                        return <li>{item.text}</li>;
                    })}
                </ul>
            </div>
            );
    }
});

ReactDOM.render(
    <App />,
    document.getElementById('container')
    );
```
