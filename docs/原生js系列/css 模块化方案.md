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
  

## shadow dom
## 一、基本原理

- Shadow DOM 是 Web Components 的核心能力之一，它通过在宿主元素上挂载一个“影子根”（shadowRoot），把组件的内部 DOM 树与外部文档的 DOM 树隔离开。
- 组件的内部结构与样式都只存在于这个影子树中；普通的全局 CSS 不会影响影子树内部，影子树内部的样式也不会泄露到外部。
- 创建方式是给某个宿主元素附加 shadowRoot（open/closed 两种模式），随后把模板内容和样式插入到 shadowRoot 内部。
## 二、样式作用域与选择器规则

- 内部样式仅作用于影子树：在 shadowRoot 里的 <style> 或样式表只会影响影子树内的元素，天然实现局部作用域，不需要额外的属性选择器或哈希。
- :host 选择器：用于选择宿主元素本身（即挂载 shadowRoot 的那个元素），可以根据宿主的状态/属性写样式，例如 :host([disabled]) 控制组件禁用态。
- :host-context(...)：用于根据外部环境（祖先元素的类/属性）影响组件内部样式，典型用于主题或暗色模式；它不会直接选择外部元素，而是把外部环境作为条件作用到内部样式。
- ::slotted(...)：用于给通过插槽（slot）分发进来的“外部内容”应用样式。注意只匹配插入插槽的顶层节点，不能深选它的子孙（要在插入方自身定义样式或传入类名配合）。
- CSS 变量（自定义属性）可穿透：custom properties 会跨越 shadow 边界，这使得主题、动态值传递非常方便。通常在外部定义变量，在组件内部消费。
## 三、交互与事件机制

- 事件“重定向”（retargeting）：多数事件会在跨越 shadow 边界时重定向 target，外部监听到的目标元素显示为宿主元素而非影子内部具体节点，保证封装性。但事件依然能冒泡穿越边界（composed: true 的事件）。
- DOM 访问与模式：
  - open 模式：外部 JS 可通过 element.shadowRoot 访问影子树，利于调试与集成。
  - closed 模式：外部无法直接访问 shadowRoot，更强封装，但调试和集成成本更高。
