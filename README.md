# 프로그래밍언어론 20213039 이상진  
## 과제B2 - golang

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
math라이브러리에 대해서 알수있었다. 또한 C언어와 비슷한 printf 가 있었다는걸 알 수 있었다.  
  
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
   
**-스크린샷-**  
<img width="635" alt="스크린샷 2025-05-15 오전 10 39 46" src="https://github.com/user-attachments/assets/8cbb843b-c5a0-4e80-acbd-04fd6009eb4d" />  
  
**-후기-**  
트리를 만들어봄으로써 연결(link)를 하는 포인터가 존재한다는걸 알 수 있었다.  
  
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
  
  
--------------------------------------------------------------------------------------------  
  
## 과제B2 - swift
  
### 실습1  
swift를 이용해서 hello, world 출력 실습  
  
**-코드-**  
```swift
import Foundation

print("Hello, World!")
```  
  
**-결과-**  
```
Hello, World!
Program ended with exit code: 0
```  
  
**-스크린샷-**  
 <img width="550" alt="스크린샷 2025-05-22 오후 12 38 12" src="https://github.com/user-attachments/assets/f4cbbecb-4d82-43eb-88d1-0ee90b113a1a" />  
  
**-후기-**  
ios개발용인 swift를 mac에 설치하여, 가장 쉬운 hello, world를 실행해보았다.  
주로 개발용으로 쓰여서 실습이 많이 없는 점이 아쉽다.  
  
  
### 실습2  
swift를 이용해서 Optional 타입 실험 실습  
  
**-코드-**  
```swift
func greetUser(name: String?) {
    if let unwrappedName = name {
        print("안녕하세요, \(unwrappedName)님!")
    } else {
        print("이름이 없습니다.")
    }
}

greetUser(name: "상진")
greetUser(name: nil)
```  
  
**-결과-**  
```
안녕하세요, 상진님!
이름이 없습니다.
Program ended with exit code: 0
```  
  
**-스크린샷-**  
 <img width="558" alt="스크린샷 2025-05-22 오후 12 45 41" src="https://github.com/user-attachments/assets/65ad20ad-a668-4a67-b62d-2dc52558a0ce" />  
  
**-후기-**  
Optional타입 String?은 nil(null)을 가질수 있다는걸 알았고 if let 구문은 옵셔널 바인딩을 통해 안전하게 값을 꺼낼 수 있다는걸 알았다.  
  
  
### 실습3  
swift를 이용해서 코드 블록을 변수처럼 전달할 수 있는 클로저 실습  
  
**-코드-**  
```swift
let multiply: (Int, Int) -> Int = { a, b in
    return a * b
}

let result = multiply(3, 4)
print("곱셈 결과: \(result)") 
```  
  
**-결과-**  
```
곱셈 결과: 12
Program ended with exit code: 0
```  
  
**-스크린샷-**  
<img width="583" alt="스크린샷 2025-05-22 오후 12 52 10" src="https://github.com/user-attachments/assets/aa7d9ff9-61e6-4559-9ff7-79cdbf9d27f4" />  
  
**-후기-**  
{ a, b in return a * b }은 두 정수를 받아 곱한 값을 반환하는 클로저 표현식이다. 이를 통해 swift는 함수형 프로그래밍 스타일을 지원한다는걸 알았다.  
  
  
### 실습4  
swift를 이용해서 이전값 접근 실습  
  
**-코드-**  
```swift
var steps: Int = 0 {
    didSet {
        print("걸음 수가 \(oldValue)에서 \(steps)으로 변경됨")
    }
}

steps = 100
steps = 250

}

let result = multiply(3, 4)
print("곱셈 결과: \(result)") 
```  
  
**-결과-**  
```
걸음 수가 0에서 100으로 변경됨
걸음 수가 100에서 250으로 변경됨
Program ended with exit code: 0
```  
  
**-스크린샷-**  
<img width="560" alt="스크린샷 2025-05-22 오후 12 55 16" src="https://github.com/user-attachments/assets/78051b67-133a-4f0d-ba2c-d25c7b2957f5" />  
  
**-후기-**  
didset은 값이 변경된 직후 실행되지만, 이전 값을 oldValue로 접근 할 수 있다는걸 알았다. 이를 통해 유용성 검사를 할 수 있을거같다.  
  
  
### 실습5  
swift를 이용해서 값을 함께 저장하는 실습  
  
**-코드-**  
```swift
enum Media {
    case book(title: String, author: String)
    case movie(title: String, director: String)
}

let favorite = Media.book(title: "해리포터", author: "J.K. 롤링")

switch favorite {
case .book(let title, let author):
    print("책: \(title) by \(author)")
case .movie(let title, let director):
    print("영화: \(title) by \(director)")
}

```  
  
**-결과-**  
```
책: 해리포터 by J.K. 롤링
Program ended with exit code: 0
```  
  
**-스크린샷-**  
<img width="591" alt="스크린샷 2025-05-22 오후 1 00 13" src="https://github.com/user-attachments/assets/a65914d0-accd-4c85-9e81-cfa4418ceb3b" />  

  
**-후기-**  
case마다 관련 데이터를 포함 할 수 있고, switch로 분기 처리 시 패턴 매칭으로 값 추출이 가능하다는걸 알았다.  
  
  
### 실습6  
swift를 이용해서 객체지향, 프로토콜 지향 실습  
  
**-코드-**  
```swift
protocol Flyable {
    func fly()
}

struct Bird: Flyable {
    func fly() {
        print("새가 날고 있어요!")
    }
}

struct Airplane: Flyable {
    func fly() {
        print("비행기가 이륙합니다!")
    }
}

let flyers: [Flyable] = [Bird(), Airplane()]
flyers.forEach { $0.fly() }

}

```  
  
**-결과-**  
```
새가 날고 있어요!
비행기가 이륙합니다!
Program ended with exit code: 0
```  
  
**-스크린샷-**  
<img width="574" alt="스크린샷 2025-05-22 오후 1 05 54" src="https://github.com/user-attachments/assets/e162e50f-a550-4ab6-9ee5-f01f505aebb9" />  
  
**-후기-**  
Flyable이라는 프로토콜을 정의하고, 이를 채택한 타입은 fly()메소드를 구현함으로써 프로토콜 지향이라는걸 알 수 있다. 이를 통해 다양한 타입을 공통 인터페이스로 처리 할 수 있다.  
  
  
