## useCallback 使用的判断原则
### 🎯 核心原则：引用稳定性需求
判断是否使用 useCallback 的关键问题是： 这个函数的引用稳定性是否重要？

### 📋 具体判断清单 ✅ 必须使用 useCallback 的场景
1. 作为其他 Hook 的依赖项
   
   ```javascript
   const fetchData = 
   useCallback(() => {
     // 逻辑
   }, [dependency]);
   
   useEffect(() => {
     fetchData();
   }, [fetchData]); // ← 这里
   依赖了 fetchData
   ```
2. 自定义 Hook 的返回函数
   
   ```javascript
   // 自定义 Hook 中
   const setPageSize = 
   useCallback((size) => {
     // 逻辑
   }, [key]);
   
   return { setPageSize }; // 
   ← 对外暴露的函数
   ```
3. 传递给使用了 React.memo 的子组件
   
   ```javascript
   const handleClick = 
   useCallback(() => {
     // 逻辑
   }, []);
   
   return <MemoizedChild 
   onClick={handleClick} />;
   ```
   ❌ 不需要使用 useCallback 的场景
1. 简单的事件处理函数（不作为依赖项）
   
   ```javascript
   const handleClick = () => {
     console.log('clicked');
   };
   
   return <button onClick=
   {handleClick}>Click</
   button>;
   ```
2. 每次都需要最新闭包的函数
   
   ```javascript
   const getCurrentValue = () 
   => {
     return currentState; // 
     总是需要最新的 state
   };
   ```
3. 组件内部使用且不传递的函数
   
   ```javascript
   const processData = (data) 
   => {
     return data.filter(/* 逻
     辑 */);
   };
   
   // 只在组件内部使用，不传递给其
   他地方
   const result = processData
   (someData);
   ```
### 🤔 边界情况的判断 情况1：函数可能被作为依赖项
```javascript
// 如果不确定这个函数是否会被作为
依赖项使用
// 建议使用 useCallback，预防性
优化
const utilityFunction = 
useCallback(() => {
  // 逻辑
}, [dependency]);
``` 情况2：性能敏感的场景
```javascript
// 在性能敏感的组件中，可以考虑使
用
const expensiveHandler = 
useCallback(() => {
  // 复杂逻辑
}, []);
```
### 📝 实用的决策流程
```
这个函数会被用作其他 Hook 的依赖
项吗？
├─ 是 → 使用 useCallback
└─ 否 → 继续判断
    |
    这个函数是自定义 Hook 的返回
    值吗？
    ├─ 是 → 使用 useCallback
    └─ 否 → 继续判断
        |
        这个函数会传递给使用了 
        React.memo 的组件吗？
        ├─ 是 → 使用 
        useCallback
        └─ 否 → 通常不需要 
        useCallback
```
### 🎯 总结
简单记忆法则：

- Hook 相关 ：作为依赖项或 Hook 返回值 → 用 useCallback
- 组件优化 ：传递给 memo 组件 → 用 useCallback
- 普通使用 ：组件内部简单使用 → 不用 useCallback
核心思想： useCallback 是为了解决 引用稳定性 问题，而不是为了性能优化。只有当函数的引用稳定性对程序正确性或性能有影响时，才需要使用它。
