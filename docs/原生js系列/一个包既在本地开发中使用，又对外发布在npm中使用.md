如果一个包既在本地开发中使用，又对外发布在npm中使用，这时如果按照正常引入可能会有问题：
该文件下得pacakage.json文件中配置如下

{
"main": "./dist/index.js",
  "module": "./dist/index.mjs",
  "browser": "dist/index.umd.js",
  "types": "./dist/index.d.ts",
}

当本地文件引用这个模块时，因为它有pacakage.json文件，运行时vite会把它当作设置得type对应得类型解析，没有设置默认是commonjs模块，由于"main": "./dist/index.js",设置了 "./dist/index.js",  这个dist目录只有在打包时才会有的，运行时是没有得，但是本地开发时又需要使用，怎么办呢？

在vite中配置引用别名即可：

resolve: {
    alias: {
      'xxxx-detail-modal': path.resolve(__dirname, './src/xxxx/xxxx/index.tsx'),
      // 如果您的项目根目录不是 vite.config.ts 所在的目录，请相应调整路径
    },
  },
