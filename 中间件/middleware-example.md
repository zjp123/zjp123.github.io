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

// 中间件类型定义：(store) => (next) => (args) => result

// 日志中间件
const logMiddleware = (store) => (next) => (newState) => {
  console.log('之前的状态:', store.getState());
  const result = next(newState);
  console.log('之后的状态:', result);
  return result;
};

// 验证中间件
const validateMiddleware = (store) => (next) => (newState) => {
  if (typeof newState === 'function' || typeof newState === 'object') {
    return next(newState);
  }
  console.error('setState只接受对象或函数类型的参数');
  return store.getState();
};

// 组合多个中间件
const compose = (...fns) => 
  fns.reduce((f, g) => (...args) => f(g(...args)));

// 应用中间件的函数
const applyMiddleware = (...middlewares) => (store) => {
  // 保存原始的setState方法
  const originalSetState = store.setState;
  
  // 将中间件串联起来，形成增强版的setState
  const chain = middlewares
    .map(middleware => middleware(store))
    .reverse();
  
  // 组合所有中间件，创建增强版的setState
  store.setState = compose(...chain)(originalSetState);
  
  return store;
};

// 使用示例
const store = createStore({ count: 0 });

// 应用中间件
const enhancedStore = applyMiddleware(
  logMiddleware,
  validateMiddleware
)(store);

// 测试
enhancedStore.setState({ count: 1 }); // 正确的更新
enhancedStore.setState(123); // 验证失败
enhancedStore.setState((state) => ({ count: state.count + 1 })); // 正确的更新

```
