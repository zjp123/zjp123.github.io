它们并不冲突！ `error.tsx` 和 `global-error.tsx` 在Next.js App Router中有不同的作用域和优先级：

错误处理的层级顺序：

1. error.tsx - 页面级错误边界（优先级更高）
   
   - 捕获页面组件、子组件中的错误
   - 不会捕获 layout.tsx 中的错误
   - 保留根布局的导航和UI结构
2. global-error.tsx - 全局错误边界（兜底处理）
   
   - 只有当 error.tsx 无法处理时才会触发
   - 主要捕获根 layout.tsx 中的错误
   - 会替换整个应用的根布局（包括 <html> 和 <body> 标签）
实际渲染逻辑：

- 普通页面错误 → 渲染 error.tsx
- 根布局错误 → 渲染 global-error.tsx
- error.tsx 本身出错 → 渲染 global-error.tsx
