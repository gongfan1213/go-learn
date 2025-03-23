https://tour.go-zh.org/moretypes/17

# range 遍历（续）
可以将下标或者值赋予_来忽略它
```js
for i,_ :=range pow
for _,value :=range pow
```
如果你只需要索引，忽略第二个变量就可以了
```js
for i := range pow
```
```js
package main

import "fmt"

func main() {
	pow := make([]int, 10)
	for i := range pow {
		pow[i] = 1 << uint(i) // == 2**i
	}
	for _, value := range pow {
		fmt.Printf("%d\n", value)
	}
}
```
