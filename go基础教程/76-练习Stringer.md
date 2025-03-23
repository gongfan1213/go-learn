# 练习: Stringer
通过让IPAddr类型实现fmt.Stringer来打印点号分隔的地址

https://tour.go-zh.org/methods/18

例如：IPAddr{1,2,3,4}应当打印位"1.2.3.4"。

```js
package main

import "fmt"

type IPAddr [4]byte

// TODO: 为 IPAddr 添加一个 "String() string" 方法。
func (ip IPAddr) String() string {
    return fmt.Sprintf("%d.%d.%d.%d", ip[0], ip[1], ip[2], ip[3])
}
func main() {
	hosts := map[string]IPAddr{
		"loopback":  {127, 0, 0, 1},
		"googleDNS": {8, 8, 8, 8},
	}
	for name, ip := range hosts {
		fmt.Printf("%v: %v\n", name, ip)
	}
}

```
