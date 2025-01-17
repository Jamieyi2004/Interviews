# YL tech 2025/1/14 10:00 
## 1. 判断一个数组`a`是否是另一个数组`b`的子集
1. **使用map进行频率计数**
```go
package main

import "fmt"

func isSubsetWithMap(a, b []int) bool {
    freq := make(map[int]int) // 当你需要一个可以立即使用的映射时，应该使用 make 来初始化它
    
    // Count frequency of elements in b
    for _, num := range b {    
        freq[num]++ 
    }    
     
    // Check if all elements in a are present in b with enough frequency
    for _, num := range a {
        if freq[num] == 0 {
            return false
        }
        freq[num]--
    }
    
    return true
}

func main() {
    a := []int{1, 2, 3}
    b := []int{1, 2, 3, 4, 5}
    fmt.Println(isSubsetWithMap(a, b)) // Output: true
}
```

2. **排序后双指针遍历**

```go
package main

import (
    "fmt"
    "sort"
)

func isSubsetWithSort(a, b []int) bool {
    sort.Ints(a)
    sort.Ints(b)
    
    i, j := 0, 0
    
    for i < len(a) && j < len(b) {
        if a[i] == b[j] {
            i++
            j++
        } else if a[i] > b[j] {
            j++
        } else {
            return false
        }
    }
    
    return i == len(a)
}

func main() {
    a := []int{1, 2, 3}
    b := []int{3, 4, 5, 1, 2}
    fmt.Println(isSubsetWithSort(a, b)) // Output: true
}
```

### 注意
- 切片、映射和通道都是引用类型。这意味着它们实际上是指向底层数据结构的指针或引用。当你创建这些类型的变量时，你实际上是创建了一个指向该数据结构的引用，而不是直接创建数据结构本身。当你需要一个可以立即使用的引用类型时，应该使用 make 来初始化它。
- 如何遍历
```go
for _, num := range arr {}    

i, j := 0, 0
for i < len(a) && j < len(b) {}

for i := 1; i <= 10; i++ {}
```
- `fmt` 包是Go语言标准库中用于格式化I/O操作的包，它提供了丰富的函数来处理输入输出。下面介绍 `fmt` 包中最常用的几个输出函数：
1. `fmt.Println`
打印一个或多个容易类型参数到标准输出，并在每个输出后添加换行符。
2. `fmt.Printf`
根据提供的格式字符串打印输出。类似于C语言中的 `printf` 函数。
3. `fmt.Sprintf`
与 `fmt.Printf` 类似，但它不直接输出到标准输出，而是返回格式化后的字符串。
4. `fmt.Print`
类似于 `fmt.Println`，但是不会自动添加换行符。
5. `fmt.Fprintln` 和 `fmt.Fprintf`
6. 常见的格式动词
当你使用 `fmt.Printf` 或 `fmt.Sprintf` 时，可以使用以下常见的格式动词来指定如何格式化变量：
- `%v`：默认格式显示值。
- `%T`：显示值的类型。
- `%t`：布尔值格式为 `true` 或 `false`。
- `%d`：十进制整数。
- `%x`：十六进制整数。
- `%f`：浮点数。
- `%s`：字符串。
- `%q`：带引号的字符串，适合用作 Go 字面量。
- `%p`：指针地址。

## 2. 反转链表
```go
package main

import "fmt"

// 定义链表节点结构
type ListNode struct {
    Val  int       // 节点值
    Next *ListNode // 指向下一个节点的指针
}

// 打印链表（用于调试）
func printList(head *ListNode) {
    for head != nil {
        fmt.Printf("%d -> ", head.Val)
        head = head.Next
    }
    fmt.Println("nil")
}

// 反转链表
func reverseList(head *ListNode) *ListNode {
    var prev *ListNode = nil
    current := head
    for current != nil {
        next := current.Next // 暂存下一个节点
        current.Next = prev  // 反转当前节点的指针
        prev = current       // 移动prev到当前节点
        current = next       // 移动current到下一个节点
    }
    return prev // 返回新的头节点
}

// 创建测试链表并调用反转函数
func main() {
    // 创建链表: 1 -> 2 -> 3 -> 4 -> 5 -> nil
    head := &ListNode{Val: 1}
    head.Next = &ListNode{Val: 2}
    head.Next.Next = &ListNode{Val: 3}
    head.Next.Next.Next = &ListNode{Val: 4}
    head.Next.Next.Next.Next = &ListNode{Val: 5}

    fmt.Println("Original list:")
    printList(head)

    // 反转链表
    reversedHead := reverseList(head)

    fmt.Println("Reversed list:")
    printList(reversedHead)
}
```

### 注意
- 所有参数都是按值传递：包括指针在内的所有参数都是以值的方式传递。
- 指针传递的是地址的副本：当传递指针时，实际上传递的是地址的副本，但这不影响通过指针访问和修改原始数据的能力。
- 对于引用类型：如切片、映射、通道等，由于其内部机制包含指向底层数据结构的指针，因此对这些类型的修改会影响到原始对象。

- 初始化：`head := &ListNode{Val: 1}` 这里的Val可以省略

## 3. 使用指针来交换两个整数变量的值
```go
func swap(x, y *int) {
    // 临时变量用于存储 x 的值
    temp := *x
    // 将 y 的值赋给 x
    *x = *y
    // 将临时变量的值（原来的 x）赋给 y
    *y = temp
}

func modifyInt(x *int) {
    *x = 100 // 修改指针指向的值
}

func main() {
    a := 5
    modifyInt(&a)             // 传递变量 a 的地址
}
```

## 4. 接口
```go
package main

import (
	"fmt"
	"math"
)

// 定义 Shape 接口，包含 area 方法
type Shape interface {
	area() float64
}

// 定义 Rectangle 结构体
type Rectangle struct {
	width  float64
	height float64
}

// 实现 Shape 接口的 area 方法
func (r Rectangle) area() float64 {
	return r.width * r.height
}

// 定义 Circle 结构体
type Circle struct {
	radius float64
}

// 实现 Shape 接口的 area 方法
func (c Circle) area() float64 {
	return math.Pi * c.radius * c.radius
}

// 打印形状信息及面积
func printShapeInfo(s Shape) {
	fmt.Printf("Type: %T, Area: %.2f\n", s, s.area())
}

func main() {
	// 创建 Rectangle 实例并计算面积
	rect := Rectangle{width: 10, height: 5}
	printShapeInfo(rect)

	// 创建 Circle 实例并计算面积
	circle := Circle{radius: 7}
	printShapeInfo(circle)
}
```

### 注意
- 接口（interface）的定义与普通函数定义有所不同。当你在接口中声明方法时，不需要使用 func 关键字。相反，你只需要列出方法签名（即方法名称和参数列表以及返回值类型）
- 在Go语言中，接口的实现是隐式的，即只要一个类型实现了接口中定义的所有方法，它就被认为实现了该接口。如果多个接口都定义了相同签名的方法（例如 area 方法），那么当一个类型实现了这些方法时，它会同时实现所有这些接口。
- 通过定义接口，允许我们在不知道具体类型的情况下操作对象，从而实现了多态性。

## 5. goroutine
```go
package main

import (
	"fmt"
	"sync"
)

func printNumber(wg *sync.WaitGroup, ch chan int) {
	defer wg.Done()
	number := <-ch // 等待并接收前一个goroutine发送的数字
	fmt.Println(number)
	if number < 10 {
		ch <- number + 1 // 如果不是最后一个数字，则发送下一个数字
	}
}

func main() {
	var wg sync.WaitGroup
	ch := make(chan int, 1) // 创建一个带缓冲的通道，容量为1

	wg.Add(10) // 我们将启动10个goroutine

	for i := 1; i <= 10; i++ {
		go printNumber(&wg, ch)
	}

	ch <- 1 // 启动第一个goroutine

	wg.Wait() // 等待所有goroutine完成

	close(ch) // 关闭通道（可选）
}
```

### 注意
在go语言中，当 main 函数结束时，整个程序会立即终止，无论其他goroutine是否已经完成它们的工作。这意味着如果你的 main 函数提前结束，所有尚未完成的goroutine都会被强制终止，而不会有机会完成它们的任务。

## 6. C和CPP函数调用区别
- **C 是过程式编程**，没有类或对象；C++ 支持面向对象编程，允许定义类和成员函数。
- **C 不支持函数重载**；C++ 支持根据参数不同定义多个同名函数。
- **C 不支持默认参数**；C++ 允许为函数参数指定默认值。
- **C 没有模板机制**；C++ 支持模板，用于编写泛型代码。
- **C 没有构造函数和析构函数**；C++ 类可以有构造函数和析构函数。
- **C 没有内置异常处理**；C++ 支持 `try-catch` 异常处理机制。
- **C++ 支持虚函数**，允许多态性，通过基类指针调用派生类方法。

## 7. 为什么选择做这个项目
在淘宝双11期间，每秒钟可能会有数百万笔交易同时发生。这些交易涉及到多个数据中心的分布式数据库，包括订单信息、库存管理和支付状态等。为了确保所有交易数据的一致性和可靠性，即使在网络分区或节点故障的情况下，使用 Raft 共识算法来管理这些分布式数据库是关键。


# YT tech 2025/1/15
## 自我介绍
面试官您好，我叫易嘉鸣，是华中师范大学计算机26届科学与技术专业的本科生。

在校期间，我对编程和技术充满热情，在各类程序设计竞赛中屡获佳绩，ICPC国际大学生程序设计竞赛全国邀请赛银奖、蓝桥杯大赛人工智能大学组全国一等奖等。

在我的项目经历方面，我独立完成了分布式K/V存储系统的开发，该系统使用了Linux和Golang构建，实现了包括Get()、Put()、Append()在内的核心API，并通过Raft共识算法确保了一致性和容错性。这个项目经过了大量的测试案例验证，证明了其高可用性和稳定性，为大规模应用提供了可靠的技术支持。

除了技术能力外，我还具备良好的沟通协作能力，曾担任学校ACM-ICPC协会副会长，负责组织多项活动和比赛。我相信这些经验能够帮助我在团队环境中更好地发挥作用。


## 怎么排查问题
1. 常用的工具
报错，日志，系统监控工具，pprof性能分析，zipkin分布式追踪系统，对于微服务架构特别有用。
2. 网上的论坛
查阅文档和资源：
查看官方文档、API手册或其他技术资料，确保对所使用的库和技术有正确的理解。
在互联网上搜索类似的错误信息，看看是否有其他人遇到并解决了相同的问题。
3. 同事
4. 记录和总结：
记录下问题的原因、解决方案以及从中学习到的经验教训。
更新项目的FAQ或者知识库，为将来可能出现的类似问题提供参考。

## 最具有挑战的？