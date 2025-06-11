# Go

## 기본 명령어

```sh

# 버전 확인
go version

# 모듈 생성 (빌드시 반드시 필요)
go mod init [모듈명]

# 빌드
go build

```

## 기본 문법

```go

// 현재 코드가 속하는 패키지
// public은 파스칼케이스, private는 카멜케이스 사용
package main

// 외부 패키지 import
import (
    "fmt"
    "math"
    "golang.org/x/exp/constraints"
)

//메인 함수
func main() {
    // 변수 선언
    var a int = 5
    b := 6 // := 사용 시 타입 추론 및 대입, 함수 내부에만 사용 가능 하고 재할당 불가능

    // 배열 선언
    var arr [5]int = [5]int{1, 2, 3, 4, 5}
    // 포인터, nil == null
    var p *int = nil;

    // 객체 생성
    var stu1 = Student{}
    var stu2 = new(Student)

    // 출력
    fmt.Printf("%d", a);

    // 입력
    n, err := fmt.Scan(&a);

    // 열거형
    const (
        // iota는 0, 1, 2순차 시퀀스
        One int = iota
        Two
        Three
    )

    if a > 4 {
        // if문
    } else if a < 3 {

    } else {

    }

    // ;뒤가 조건문, ;앞은 초기문, 조건 검사 전에 실행하는 코드
    // switch도 같은 형식이 가능
    if one, two := Somefunc(); two {

    }

    // switch문은 기본적으로 케이스 1개 실행 후 break, 다음까지 이어서 실행하려면 fallthrough를 사용

    defer fmt.Println("블록 종료 시 호출, 아래부터 역순으로 실행")
    defer func() {
        if r := recover(); r != nil {
            // panic 처리
        }
    }

    panic("Assert(Fatal Error)")

    go Add(1, 2) // 고루틴 == 경량 스레드

    // 채널 (고루틴 간 메시지 큐)
    var messages chan string = make(chan string, 10) // 타입, 버퍼 크기
    messages <- "메시지 삽입"
    msg := <- messages // 메시지 수신

}

// 함수 형식 (반환값 타입이 메소드명 뒤에 붙음)
func Add(a int, b int) int {
    return a + b;
}

// 구조체
type Student struct {
    Name string
    Class string
    Age int
}

// 인터페이스
type Bird interface {
    Fly()
    Walk(speed float64)
}

func Sqrt(f float64) (float64, error) {
    if f < 0 {
        return 0, fmt.Errorf("Error") // 에러 발생
    }
    return math.Sqrt(f), nil
}

// 이 메소드를 선언하는 경우 에러로 사용가능
func Error() string {
    // type error interface {
    //     Error() string
    // }
    return "Error" // 에러 발생
}

// 제네릭
func multiply[T constraints.Integer | constraints.Float](a T, b T) T {
    return a * b
}

```

```go

package somepack

import "fmt"

func init() {
    // 자동 패키지 초기화 함수
}

// 슬라이스
var slice1 = []int{1, 5:2, 8:3} // {1, 0, 0, 0, 0, 2, 0, 0, 3}
var slice2 = make([]int, 10)
slice3 := append(slice1, 3) // 요소 추가(빈 공간이 있으면 slice1과 같은 주소를 참조, 없으면 다른 주소를 참조)

array := [5]int{1, 2, 3, 4, 5}
slice4 := array[1:3] // {2, 3, 4}


```