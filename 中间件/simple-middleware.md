```typescript

// 创建一个基础的状态管理器
const createStore = (initialState) => {
  let state = initialState;
  
  return {
    getState: () => state,
    setState: (newState) => {
      state = typeof newState === 'function' 
        ? newState(state)
        : newState;
      return state;
    }
  };
};

// 日志中间件
const withLogger = (store) => {
  const originalSetState = store.setState;
  
  store.setState = (newState) => {
    console.log('之前的状态:', store.getState());
    const result = originalSetState(newState);
    console.log('之后的状态:', result);
    return result;
  };
  
  return store;
};

// 验证中间件
const withValidator = (store) => {
  const originalSetState = store.setState;
  
  store.setState = (newState) => {
    if (typeof newState === 'function' || typeof newState === 'object') {
      return originalSetState(newState);
    }
    console.error('setState只接受对象或函数类型的参数');
    return store.getState();
  };
  
  return store;
};

// 创建store并直接链式调用中间件
const store = withLogger(withValidator(createStore({ count: 0 })));

// 测试
store.setState({ count: 1 }); // 正确的更新
store.setState(123); // 验证失败
store.setState((state) => ({ count: state.count + 1 })); // 正确的更新

```
