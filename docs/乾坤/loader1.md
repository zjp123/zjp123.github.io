## 一句话版本

- loader 是 qiankun 的“HTML Entry 流式加载与注入器”，负责把子应用的入口页面按流式插入到主应用容器中，在沙箱内正确转换、加载并按顺序执行脚本与样式，最终拿到子应用暴露的对象。
核心职责

- 拉取子应用入口（HTML Entry），进行流式解析与注入到指定容器。
- 转换和控制资源标签（script/link/style），确保执行顺序，支持 defer/script 队列。
- 与沙箱协作，定位子应用入口脚本暴露的全局对象，并作为 load 结果返回。
- 提供扩展点：流式转换（streamTransformer）、节点转换（nodeTransformer）、资源转译选项（fetch、脚本/样式转译）


## 工作流程（面试可讲的“过程描述”）

- 请求入口资源：根据 entry（URL 或已拿到的 Response），使用 opts.fetch 获取 HTML 流。
- 流式处理：通过 TextDecoderStream 解码，再经 createTagTransformStream 做标签替换（如 head 标签替换为 qiankun 专用 head），最后用 WritableDOMStream 逐段写入到容器 DOM。
- 节点转换与资源控制：
  - 可选 nodeTransformer 对每个节点做转译（比如替换脚本地址、注入属性、重写 link 等）。
  - 对阻塞资源（同步外链 script、匹配的 stylesheet）进行阻塞/预加载，确保插入与执行顺序正确。
  - 对标记了 defer 的脚本建立队列，保证在入口 HTML 完成后按队列执行。
- 入口脚本与沙箱协作：
  - 只支持“外链 + 带 entry 属性”的入口脚本，监听其 onload/onerror；
  - 当入口脚本加载完成，从 sandbox.latestSetProp 获取子应用暴露对象并 resolve 结果；
  - 如果没有发现入口脚本，HTML 流结束后会用最新的沙箱设置（latestSetProp）做兜底处理。
- 错误处理：入口脚本失败、响应体为空等会抛出明确错误。
  
## 可扩展点（体现你对模块的理解与工程能力）

- LoaderOpts
  - streamTransformer：自定义 HTML 流的转换（如做更多标签替换、注入 meta 等）。
  - nodeTransformer：对每个节点做定制处理（CDN 替换、跨域属性、SRI、兼容性修正、脚本注入）。
  - fetch 与转译选项：配合共享库中的转译配置，改写资源加载策略。
  - sandbox：和沙箱强绑定，确保子应用只影响自己的隔离环境。
- 性能与可靠性
  - 流式注入缩短“首屏可见”时间，阻塞资源做预加载，降低资源等待损耗。
  - 通过队列与 onload/onerror 监听保证可控执行与错误兜底。


## 面试时可以用的表达亮点

- loader 不是简单的“fetch + innerHTML”，它是“真正的流式 HTML 注入器”，在写入的同时管控资源的加载与脚本执行顺序，并且与沙箱联动拿到子应用暴露对象。
- 入口脚本必须是外链且带 entry 属性，这样 loader 才能在 onload 时准确返回子应用的暴露对象；没有入口脚本时有 latestSetProp 的兜底。
- 提供 streamTransformer 和 nodeTransformer 两个扩展点，让企业在不同环境下灵活做标签/节点转译与资源替换，兼顾性能与安全。
