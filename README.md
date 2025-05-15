# 프로그래밍언어론 20213039 이상진  
## 과제B2

### 실습1  
피보나치 수열을 golang 방식을 이용하여 재귀, 반복 방식으로 구하는 실습  
  
**-코드-** 
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
  
**-결과-**  
```
FibonacciLoop: 0 1 1 2 3 5 8 13 21 34 
FibonacciRecursion: 0 1 1 2 3 5 8 13 21 34 
```  
  
**-스크린샷-**  
<img width="636" alt="스크린샷 2025-05-15 오전 10 16 53" src="https://github.com/user-attachments/assets/ec77d5f6-ea91-4946-bcce-3a7459e4c6f6" />  
  
**-후기-**  
배열의 중간부분을 잘라서 가져오는 slice가 있다는걸 알았고, 간단한 피보나치를 이용해 재귀, 반복을 연습할 수 있었다.  
  
### 실습2  
golang언어를 이용하여 하노이탑을 실습  
  
**-코드-**  
```go
package main
import (
	"fmt"
	"math"
)

type pole []int
var step int

func (p *pole) Push(a int) {
	*p = append(*p, a)
}

func (p *pole) Pop() int {
	r := (*p)[len(*p)-1]
	*p = (*p)[:len(*p)-1]
	return r
}

func move(n int, p *[3]pole, src, tgt, aux int) {
	if n < 1 {
		return
	}
	move(n-1, p, src, aux, tgt)
	p[tgt].Push(p[src].Pop())
	step++
	fmt.Printf("---[ %d ]-------\n1: %v\n2: %v\n3: %v\n", step, p[0], p[1], p[2])
	move(n-1, p, aux, tgt, src)
}

func main() {
	var p [3]pole
	var n = 3
	
	p[0] = pole{}
	p[1] = pole{}
	p[2] = pole{} 

	println("Towers of Hanoi")
	println("---------------")
	for i := n; i > 0; i-- {
		p[0].Push(i)
	}

	fmt.Printf("---[ %d ]-------\n1: %v\n2: %v\n3: %v\n", step, p[0], p[1], p[2])
	move(n, &p, 0, 2, 1)
	fmt.Printf("--> Optimal number of moves 2^n-1 = %.0f\n", math.Pow(2, float64(n))-1)
}
```  
  
**결과-**  
  
```
Towers of Hanoi
---------------
---[ 0 ]-------
1: [3 2 1]
2: []
3: []
---[ 1 ]-------
1: [3 2]
2: []
3: [1]
---[ 2 ]-------
1: [3]
2: [2]
3: [1]
---[ 3 ]-------
1: [3]
2: [2 1]
3: []
---[ 4 ]-------
1: []
2: [2 1]
3: [3]
---[ 5 ]-------
1: [1]
2: [2]
3: [3]
---[ 6 ]-------
1: [1]
2: []
3: [3 2]
---[ 7 ]-------
1: []
2: []
3: [3 2 1]
--> Optimal number of moves 2^n-1 = 7
```  
  
**-스크린샷-**  
<img width="626" alt="스크린샷 2025-05-15 오전 10 29 31" src="https://github.com/user-attachments/assets/4ec55bf2-a882-486c-9f45-712f888e8873" />  
  
**-후기-**  
math라이브러리에 대해서 알수있었다. 또한 C언어와 비슷한 printf 가 있었다는 걸 알우있었다.  
  
### 실습3  
golang을 이용하여 2개의 함수를 호출후 sleep을 통해 지연을 주어 출력을 보는 실습  
  
**-코드-**  
```go
package main

import (
	"fmt"
	"time"
)

func numbers() {
	for i:=1; i<=5; i++ {
		time.Sleep(250 * time.Millisecond)
		fmt.Printf("%d ", i)
	}
}

func alphabets() {
	for i:='a'; i<='e'; i++ {
		time.Sleep(400 * time.Millisecond)
		fmt.Printf("%c ", i)
	}
}

func main() {
	go numbers()
	go alphabets()
	time.Sleep(3000 * time.Millisecond)
	fmt.Println("main terminated")
}
```  
  
**-결과-**  
```
1 a 2 3 b 4 c 5 d e main terminated
```  
  
**-스크린샷-**  
<img width="631" alt="스크린샷 2025-05-15 오전 10 33 36" src="https://github.com/user-attachments/assets/cc4ebcb7-e9de-4ce0-b83f-591400798286" />  
    
**-후기-**  
sleep을 통해 지연을 줄수있고, 병렬로 함수를 처리하여, 각각 출력이 번갈아 나오는걸 알 수 있었다.  
  
### 실습4  
golang언어를 이용하여 이진탐색트리를 만들고 순회하며 값을 출력하는 실습  
  
**-코드-**  
```go
package main
import "fmt"

type Tree struct {
	Left  *Tree
	Value int
	Right *Tree
}

func Walk(t *Tree) {
    if t == nil { return }
    Walk(t.Left)
    fmt.Println(t.Value)
    Walk(t.Right)
}

func Insert(data int, t *Tree) {
    if data < t.Value {
        if t.Left != nil {
            Insert(data, t.Left)
        } else {
            t.Left = &Tree{Value: data}
        }
    } else {
        if t.Right != nil {
            Insert(data, t.Right)
        } else {
            t.Right = &Tree{Value: data}
        }
    }
}

func main() {
    t := &Tree{Value: 0}
    a := []int{7, -2, 8, -9, 4, 5}

    for _, i := range a {
        Insert(i, t)
    }
    Walk(t)
}

```  
  
**-결과-**  
```
-9
-2
0
4
5
7
8
```  
  
**-후기-**  
트리를 만들어봄으로써 연결(link)를 하는 포인터가 존재한다는걸 알 수 있었다.  
  
**-스크린샷-**  
<img width="635" alt="스크린샷 2025-05-15 오전 10 39 46" src="https://github.com/user-attachments/assets/8cbb843b-c5a0-4e80-acbd-04fd6009eb4d" />  
  
### 실습5  
golang언어를 이용하여 "(", ")" 로 이루어진 문자열이 균형잡혀있는지(괄호가 짝지어 닫히는지) 검사하는 실습  
  
**-코드-**  
```go
package main

import (
	"fmt"
	"strings"
)

func isBalanced(input string) string {
	if len(input) >0 {
		var stack []byte
		for i :=0; i < len(input); i ++ {
			if input[i] =='(' {
				stack = append(stack, input[i])
			} else {
				if len(stack) >0 {
					pair := string(stack[len(stack)-1]) + string(input[i])
					stack = stack[:len(stack)-1]
					if pair !="()" {
						return input +" is not balanced."
					}
				} else {
					return input +" is not balanced."
				}
			}
		}
		if len(stack) ==0 {
			return input +" is balanced."
		} else {
			return input +" is not balanced."
		}
	}
	return "Please enter a sequence of brackets."
}

func main() {
	text := "()()(())()"
	text = strings.TrimSpace(text)
	fmt.Println(isBalanced(text))
}

```
  
**-결과-**  
```
()()(())() is balanced.
```  
  
**-스크린샷-**  
<img width="635" alt="스크린샷 2025-05-15 오전 10 39 46" src="https://github.com/user-attachments/assets/9ca4fe55-61dd-4acd-a52d-fc103dd177de" />  
  
**-후기-**  
stack의 사용법, strings을 통해 문자열을 배열로 사용하는 법을 알 수 있었다.  
  
