https://tour.go-zh.org/moretypes/13

# 用make创建切片

切片可以用内置函数make来创建，这也是你创建动态数组的方式

make函数会分配一个元素为零值的数组并且返回一个引用了它的切片

```js
a := make([] int,5)//len(a) =5
```
要指定它的容量，需要向make传入第三个参数
```js
b := make([]int, 0, 5) // len(b)=0, cap(b)=5

b = b[:cap(b)] // len(b)=5, cap(b)=5
b = b[1:]      // len(b)=4, cap(b)=4
```
```js
package main

import "fmt"

func main() {
	a := make([]int, 5)
	printSlice("a", a)

	b := make([]int, 0, 5)
	printSlice("b", b)

	c := b[:2]
	printSlice("c", c)

	d := c[2:5]
	printSlice("d", d)
}

func printSlice(s string, x []int) {
	fmt.Printf("%s len=%d cap=%d %v\n",
		s, len(x), cap(x), x)
}
```
