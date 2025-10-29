## 第一种
```javascript
export default {
  mounted() {
    // 获取所有查询参数
    console.log(this.$route.query) // { name: 'john', age: '25' }
    
    // 获取单个参数
    const name = this.$route.query.name
    const age = this.$route.query.age
  },
  
  watch: {
    // 监听路由变化
    '$route.query': {
      handler(newQuery, oldQuery) {
        console.log('查询参数变化:', newQuery)
      },
      deep: true
    }
  }
}

```
## 第二种

```javascript
// 获取当前页面的查询参数
    const urlParams = new URLSearchParams(window.location.search)
    
    // 获取单个参数
    const name = urlParams.get('name')
    const age = urlParams.get('age')
```
