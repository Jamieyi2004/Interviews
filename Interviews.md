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
确实，Raft 共识算法已经被广泛应用于多个实际项目和系统中。以下是几个使用 Raft 的具体示例，这些项目展示了 Raft 在不同场景下的应用，包括分布式数据库、服务发现和配置管理等。

### 示例 1：etcd

**简介**：
- **etcd** 是由 CoreOS 开发的一个分布式键值存储系统，主要用于可靠地存储需要被共享和服务发现的数据。
- etcd 使用 Raft 算法来保证数据的一致性和高可用性，是 Kubernetes 集群管理和配置存储的核心组件之一。

**应用场景**：
- **服务发现**：在微服务架构中，etcd 可以用来注册和发现服务实例。
- **配置管理**：应用程序可以将配置信息存储在 etcd 中，并在运行时动态更新配置。
- **分布式锁**：通过 etcd 实现分布式锁机制，确保多个节点之间的协调操作。

**链接**：
- [etcd 官方网站](https://etcd.io/)
- [GitHub 仓库](https://github.com/etcd-io/etcd)

### 示例 2：Consul

**简介**：
- **Consul** 是一个用于服务网络的服务发现与配置工具，提供了健康检查、KV 存储等功能。
- Consul 内部实现了 Raft 协议，以确保集群中所有节点的数据一致性和高可用性。

**应用场景**：
- **服务发现**：Consul 提供了强大的服务注册与发现功能，适用于微服务架构中的服务管理。
- **健康检查**：自动检测服务的健康状态，并根据结果进行流量调度。
- **KV 存储**：用于存储任意的键值对，常用于动态配置管理。

**链接**：
- [Consul 官方网站](https://www.consul.io/)
- [GitHub 仓库](https://github.com/hashicorp/consul)

### 示例 3：TiKV

**简介**：
- **TiKV** 是一个分布式的事务性键值数据库，支持水平扩展和强一致性。
- TiKV 使用 Raft 来实现多副本复制，确保数据的安全性和可靠性。

**应用场景**：
- **分布式数据库**：作为 TiDB 的存储引擎，TiKV 提供了一个高性能、可扩展的分布式存储解决方案。
- **事务处理**：支持 ACID 事务，适用于金融、电商等对数据一致性要求高的场景。

**链接**：
- [TiKV 官方网站](https://tikv.org/)
- [GitHub 仓库](https://github.com/tikv/tikv)

### 示例 4：Rook

**简介**：
- **Rook** 是一个云原生存储编排工具，可以在 Kubernetes 上部署和管理各种存储后端。
- Rook 支持多种存储系统（如 Ceph），并通过 Raft 确保其元数据服务的一致性和高可用性。

**应用场景**：
- **Kubernetes 存储编排**：Rook 可以轻松地为 Kubernetes 集群添加持久化存储能力。
- **Ceph 集群管理**：提供对 Ceph 集群的自动化部署和运维支持。

**链接**：
- [Rook 官方网站](https://rook.io/)
- [GitHub 仓库](https://github.com/rook/rook)

### 结合双11的具体案例

在淘宝双11期间，像 etcd 和 Consul 这样的工具可以帮助管理大规模的服务发现和配置管理，确保各个服务之间的协调运作；而 TiKV 则可以作为高性能的分布式数据库，支持海量交易数据的快速读写。通过 Raft 算法提供的强一致性保障，这些系统能够在高并发环境下保持稳定运行，确保每一个交易都能准确无误地完成。

### 总结

上述示例展示了 Raft 算法在实际项目中的广泛应用，从服务发现到分布式数据库，再到存储编排，Raft 帮助这些系统实现了数据一致性和高可用性。对于像淘宝双11这样的大型电商活动来说，Raft 的应用确保了系统的稳定性和可靠性，为用户提供流畅的购物体验。
当然可以。Raft 共识算法在像淘宝双11这样的高并发、大规模分布式系统中扮演着至关重要的角色，特别是在确保数据一致性和容错性方面。以下是结合 Raft 和双11 的具体应用场景和意义。

### 应用场景：确保交易数据的一致性和可靠性

#### 例子：分布式数据库一致性管理

**背景**：在淘宝双11期间，每秒钟可能会有数百万笔交易同时发生。这些交易涉及到多个数据中心的分布式数据库，包括订单信息、库存管理和支付状态等。为了确保所有交易数据的一致性和可靠性，即使在网络分区或节点故障的情况下，使用 Raft 共识算法来管理这些分布式数据库是关键。

**技术应用：Raft 共识算法**

- **领导者选举（Leader Election）**：Raft 算法通过一个简单的领导者选举机制，确保在一个集群中只有一个领导者负责处理客户端请求。当领导者出现故障时，其他节点会迅速选举出新的领导者，保证系统的持续可用性。
  
- **日志复制（Log Replication）**：所有的写操作首先由领导者接收，并以日志的形式记录下来。然后，领导者将这些日志同步到集群中的其他节点。只有当大多数节点确认接收到日志后，才会应用这些更改。这种方式确保了即使部分节点失效，数据仍然是一致和可靠的。
  
- **安全性（Safety）**：Raft 确保同一任期（term）内不会有两个不同的领导者提交冲突的日志条目。这防止了数据不一致的情况发生，保证了分布式系统的强一致性。

#### 实际意义

- **高可用性和容错性**：通过 Raft 的领导者选举和日志复制机制，即使在网络分区或节点故障的情况下，系统仍然能够保持正常运行，减少了服务中断的风险。
  
- **数据一致性**：Raft 确保了所有节点上的数据最终是一致的，这对于金融交易尤其重要，因为任何数据不一致都可能导致严重的后果，如重复扣款或丢失订单。
  
- **快速恢复**：由于 Raft 的设计简单且易于理解，故障检测和恢复过程更加高效。这使得系统能够在短时间内恢复正常运作，减少了用户等待时间和服务影响范围。

### 结合双11的具体案例

在双11期间，淘宝需要处理海量的交易数据，而这些数据分布在多个数据中心。假设某一个数据中心出现了网络故障或硬件问题：

1. **领导者选举**：Raft 算法会在剩余的健康节点中迅速选出一个新的领导者，继续处理来自用户的交易请求。用户几乎不会感受到任何服务中断。
   
2. **日志复制**：所有新产生的交易数据都会被记录为日志，并由当前的领导者同步到其他健康的节点。即使某个节点暂时无法通信，它也可以在恢复后从其他节点获取最新的日志，确保数据的一致性。
   
3. **数据一致性**：在整个过程中，Raft 确保了所有节点上的交易数据保持一致，避免了因网络分区或其他故障导致的数据不一致问题。

### 总结

通过使用 Raft 共识算法，淘宝可以在双11这样极端负载的环境下，确保交易数据的高度一致性和可靠性。Raft 提供的领导者选举、日志复制和安全性特性，不仅提高了系统的容错能力，还保障了用户体验的连续性和稳定性。这种技术的应用对于大型电商活动的成功至关重要，确保了每一个交易都能准确无误地完成。

# YT tech 2025/1/15
## 怎么排查问题
1. 常用的工具
2. 网上的论坛
3. 同事

## 最具有挑战的？
