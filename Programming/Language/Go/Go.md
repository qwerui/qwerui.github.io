# Go

## 기본 명령어

```sh

# 버전 확인
go version

# 모듈 생성
go mod init [모듈명]

# 빌드
go build

```

## 기본 문법

```go

// 현재 코드가 속하는 패키지
package main

// 외부 패키지 import
import "fmt"

func main() {
    // 변수 선언
    var a int = 5
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

```