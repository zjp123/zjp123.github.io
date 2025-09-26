//a.ts中
import { BB } from './b';
export const AA = 11
console.log(BB)

//b.ts中
import {AA} from './a'

export const BB = 66
console.log(AA)



//index.ts 
import './a'


// 结果会报错得

- 顶层 import './a'，于是开始加载并执行 a.ts
- `a.ts` 顶层 import { BB } from './b'，于是转去加载并执行 b.ts
- `b.ts` 又顶层 import { AA } from './a'，形成 a <-> b 的循环
- 此时 b.ts 的模块体会执行到 console.log(AA)，但 a.ts 里 export const AA = 11 这个绑定在 a.ts 的模块体尚未完成初始化
- 在 ES Modules 中，import 是“活绑定”，但在模块初始化完成之前访问一个还未初始化的 const 绑定会命中 TDZ（临时死区），从而抛错“Cannot access 'AA' before initialization”
