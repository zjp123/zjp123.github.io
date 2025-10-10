## scoped 
  如vue中的实现方案，其原理是：
  编译期加私有标识 + 选择器重写”，

  ```
  核心机制

  唯一 scopeId：编译器为每个组件生成一个唯一的标识（例如 data-v-123abc），在渲染时把这个属性加到该组件模板内的每个 DOM 节点上。   
  选择器重写：带 scoped 的样式会在编译阶段被重写，在每个选择器后面追加对应的属性选择器，例如 .title 会变成 .title[data-v-123abc]，div 会变成 div[data-v-123abc]。   
  这样这些规则只会匹配当前组件 DOM 上带 data-v-123abc 的元素，从而实现样式隔离。   
  工具链实现：Vue 2 由 vue-loader 完成模板打属性和样式选择器重写，Vue 3 由 @vitejs/plugin-vue/SFC 编译器完成同样的工作。运行时只负责把 scopeId 属性渲染到节点上。

  额外的语法：:deep

  ```

  ## CSS Modules
  
- 构建期重写类名实现局部作用域：打包工具在编译 .module.css（或 Vue 的 <style module>）时，会把本文件内的类名重写为带哈希的唯一类名，例如 .btn -> .btn__abc123，仅在当前模块内使用，从而避免全局冲突。
- 生成类名映射供 JS 使用：当你在 JS/TS 中 import styles from './x.module.css'，得到的是一个对象映射，styles.btn 的值就是编译后的唯一类名 'btn__abc123'。通过这个映射把样式绑定到组件的 DOM。
- 选择器与作用域的处理：
- 类选择器会被重写；标签选择器不会被本地化（如 div 不会变），因此模块内建议用类选择器。
- 提供 :local 和 :global 切换作用域；默认是本地作用域，:global(.reset) 可声明全局规则。
- 支持 composes 组合：在一个类中“组合”另一个模块的类，从而实现样式复用且保持本地作用域。
- 额外语法   :global 等

## CSS-in-JS 的原理
两大类实现：
- 运行时注入（styled-components、Emotion、JSS、Stitches 等）：在浏览器或 Node（SSR）运行时把 JS 中定义的样式序列化为 CSS，生成唯一类名（哈希），并插入到 <style> 标签（CSSOM）中。支持根据 props/状态动态生成不同的样式。
- 编译期抽取/零运行时（Linaria、Vanilla Extract 等）：构建时分析样式，把可静态确定的部分抽取为独立 CSS 文件或类名，运行时没有或很少注入；动态部分通常通过 CSS 变量或条件类名来实现。
  
  
