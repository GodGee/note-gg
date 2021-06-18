js 练习

1.let data = [{ value: '1,2' }, { value: '3' }, { value: '4,5' }] 输出 ['1', '2', '3', '4', '5']

```js
data.reduce((pre,cur) => [...pre, ...cur.value.split(',')] , [])

data.reduce((curr, next) => curr + next.value + ',', '').split(',').filter(v=>v)

data.reduce((p, c) => p.concat(c.value.split(',')), [])


data.map(item=>item.value).join().split(',') 

Array.of(data.map(item => item.value).join(','));

data.map(({ value }) => value.split(',')).flat()

data.flatMap(e=>e.value.split(','))



JSON.stringify(data).split("").filter(Number)

JSON.stringify(data).split("").filter(i=>i==+i)

JSON.stringify(data).match(/\d+/g)


```

