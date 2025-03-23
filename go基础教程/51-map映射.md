https://tour.go-zh.org/moretypes/19

# map映射

map映射将键映射到值

映射的零值为nil，nil映射不仅没有键，也不能添加键

make函数会返回给定类型的映射，并且将其初始化备用

```js
package main

import "fmt"

type Vertex struct {
	Lat, Long float64
}

var m map[string]Vertex

func main() {
	m = make(map[string]Vertex)
	m["Bell Labs"] = Vertex{
		40.68433, -74.39967,
	}
	fmt.Println(m["Bell Labs"])
}
```
