# 프로그래밍언어론 20213039 이상진  
## 과제1  

**실습1**  
피보나치 수열을 golang 방식을 이용하여 재귀, 반복 방식으로 구하는 코드  
-코드-  
```go
package main

import (
	"fmt"
)

func FibonacciLoop(n int) int {
	f := make([]int, n +1, n +2)
	if n < 2 {
		f = f[0:2]
	}
	f[0] = 0
	f[1] = 1
	for i:=2; i<=n; i++ {
		f[i] = f[i-1] + f[i-2]
	}
	return f[n]
}

func FibonacciRecursion(n int) int {
	if n <= 1 {
		return n
	}
	return FibonacciRecursion(n-1)+ FibonacciRecursion(n-2)
}

func main() {
	fmt.Print("FibonacciLoop: ")
	for i :=0; i <=9; i++ {
		fmt.Print(FibonacciLoop(i), " ")
	}
	fmt.Print("\nFibonacciRecursion: ")
	for i :=0; i <=9; i ++ {
		fmt.Print(FibonacciRecursion(i), " ")
	}
}
```

-결과-  
```
FibonacciLoop: 0 1 1 2 3 5 8 13 21 34 
FibonacciRecursion: 0 1 1 2 3 5 8 13 21 34 
```
