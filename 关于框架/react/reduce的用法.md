## 组合函数

把函数f应用到x的g结果上，即组合f.g

```
f(g(x))
```

```
const compose = (...fns) => x => fns.reduceRight((v, f)=> f(v), x) 
```

```
const pipe = (...fn) => x=> fns.reduce((newRes, func) => func(newRes),x)
```

下面的是一个个执行