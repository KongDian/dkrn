### 前言

- Go 语言被称为 "类 C 语言" 或 "21 世纪的 C",继承了 C 语言的所强调的要点: 程序要编译成高效的机器码, 并自然地与所处的操作系统提供的抽象机制配合.
- Go 语言有 受到 CSP 的启发.

> CSP (Communicating Sequential Process), 通信顺序进程. 在 CSP 中, 程序就是一组无共享状态进程的并行组合, 进程间的通信和同步采用通道完成.

- Rob Pike : 复杂性是以乘积方式增长的.
- 好的变化就既可以实现目的, 又能够不损害 Fred Brooks 所谓软件设计上的 "概念完整性" . 坏的变化就做不到这一点, 而致命的变化则会牺牲 "简单性" 去换取浅薄的 "方便性" . 但是只有通过设计上的简单性, 系统才能在增长过程中保持稳定, 安全和自洽.
- Go 项目有保持极端简单性的行为文化.

- 有垃圾回收, 包系统, 一等公民函数, 词法作用域, 系统调用接口, 默认用 UTF-8 编码的不可变字符串.

- 没有隐式数值类型强制转换, 没有构造或析构函数,没有预算符重载, 没有形参默认值, 没有继承, 没有泛型, 没有异常, 没有宏, 没有函数注解, 没有线程局部存储.

- Go 尤其强调局部性的重要意义.
- Go 不要求显示的初始化或隐式的构造函数. 代码中的内存分配和内存写入大大减少.
- Go 中的聚合类型 (结构体和数组) 都以直接方式持有元素, 需要更少的存储空间以及更少的分配操作和指针间接寻址操作.
- Go 提供边长栈来运行及其轻量极线程或称为 goroutine
- Go 标准库常常被称作 "自带电池的语言".

---

### 第一章 入门

#### 1.1 hello, world

- Go 是编译型语言.

>

     go run : 将一个或多个以 .go 为后缀的源文件进行编译, 链接, 然后生成可执行文件.

go build : 编译为一个可复用的程序.

- Go 原生支持 Unicode, 可以处理所有国家的语言.
- Go 代码使用包组织起来, 包类似于其他语言的库和模块.
- 一个包由一个或多个 .go 源文件组成, 放在一个文件夹中, 该文件夹的名字描述了包的作用.
- 每一个源文件的开始都使用 package 声明, 指明该文件属于哪个一包. 后面跟着它导入的其他包的列表, 然后是存储在文件中的程序声明.
- 包名为 main 的比较特殊, 它用来定义一个独立可执行程序, 而不是库.
- main 包中, 函数 main 也是比较特殊的, 它总是程序开始的地方.
- 用 package 声明后面的 import 来导入所需的包.
- 必须精确的导入所需的包. 在缺失或存在不需要的包的情况下, 编译会失败.
- import 声明必须跟在 package 声明之后.
- import 声明必须跟在 package 声明之后. import 导入声明之后是组成程序的函数, 变量, 常量, 类型 (以 func, var, const, type 开头) 声明.
- 一个函数的声明由 func 关键字, 函数名, 参数列表 (main 函数为空), 返回值列表 (可以为空), 放在大括号内的函数体组成.
- Go 不需要再句尾或者声明后面使用分号结尾, 除非有多个语句或声明出现在同一行. 事实上, 特定符号后面的换行符会被替换成分号, 在什么地方换行会影响 Go 代码的解析.
- Go 对代码格式要求非常严格. gofmt 工具将代码以标准格式重写.

```go
package main
import "fmt"
func main(){
    fmt.Println("Hello, World")
}
```

#### 1.2 命令行参数

- os 包提供一些函数和变量, 以与平台无关的方式和操作系统打交道.
- 命令行参数以 os 包中 Args 名字的变量供程序访问, 在 os 包外面, 使用 os.Args 这个名字.

> 变量 os.Args 是一个字符串 slice. 它是一个动态容量的顺序数组. 通过 s[m:n]来访问一段子区间 (如果 m, n 缺失, 默认分别是 0 或 len(s)), 数组长度用 len(s) 表示. 在 Go 中所有索引使用半开区间, 即包含第一个索引, 不包含最后一个索引.
> os.Agrs 第一个参数是 os.Args[0], 它是命令本身的名字; 另外的元素是程序开始执行时的参数.

```go
//echo1 输出其命令行参数
package main

import (
    "fmt"
    "os"
)

func main() {
    var s, sep string
    for i := 1; i < len(os.Args); i++ {
        s += sep + os.Args[i]
        sep = " "
    }
    fmt.Print(s)
}

```

- 注释以 // 开头
- var 关键字声明了两个 string 类型的变量.
- 变量可以在声明时初始化
- 变量如果没有明确初始化, 将隐式的初始化为该类型的空值.

> 对于数字类型的初始化结果为 0 , 对于字符串是空字符串 "".

- 操作符 += 是一个赋值操作符.
- := 符号用于 ++**短变量声明**++ , 声明一个或多个变量, 并且根据初始化的值给予合适的类型.
- Go 中 i++ 等价于 i += 1 又等价于 i = i + 1; 对应的递减语句 i-- . j = i++ 是不合法的,并且只支持后缀, 所以 i-- 不合法
- for 是 Go 里面**唯一**的循环语句.其中一种形式如下:

```go
for initialization; condition; post {
 }
```

- for 循环的三个组成部分两边不用小括号. 大括号是必需的, 但左大括号和 post (后置) 语句在同一行.
- 可选的 initialization (初始化) 语句在循环开始前执行. 如果存在, 它必须是一个++简单语句++.
- condition (条件) 是一个布尔表达式, 在每次循环执行前推演, 如果结果为真, 循环继续执行.
- post 语句在循环体之后被执行, 如何条件被再次推演. 条件变成假之后循环结束.
- 三部分都是可以被省略的

> 没有 initialization 和 post 语句, 分号可以省略:
>
> for condition {
>
> }
>
> 如果条件部分不存在:
> for {
>
> }
>
> 循环是无限的, 尽管可以通过 break 或 return 等语句进行终止.

- 另外一种形式的 for 循环在字符串或 slice 数据上迭代.

```go
// echo2
package main

import (
    "fmt"
    "os"
)

func main() {
    s, sep := "", ""
    for _, arg := range os.Args[1:] {
        s += sep + arg
        sep = " "
    }
    fmt.Print(s)
}
```

> 每一次迭代, range 产生一对值: 索引和这个索引处元素的值.在这个例子里, 我们不需要索引, 但是语法上 range 循环需要处理, 因此也必须处理索引. Go 不允许存在无用的临时变量, 所以使用了**空标识符**, 它的名字是 \_ 即下划线. 空标识符可以用在任何语法需要变量名但是程序逻辑不需要的地方. 如果存在大量数据需要处理, 这样的代价比较大. 一般使用 strings 包中的 join 函数.

```go
func main() {
    fmt.Println(strings.Join(os.Args[1:]," "))
}
```

#### 1.3 找出重复行

- 第一个版本的 dup 程序输出标准输入中出现次数大于 1 的行, 前面是次数.

```go
// dup1 输出标准输入中出现次数大于 1 的行
package main

import (
    "bufio"
    "fmt"
    "os"
)

func main() {
    counts := make(map[string]int)
    input := bufio.NewScanner(os.Stdin)
    for input.Scan() {
        counts[input.Text()]++
    }
    for line, n := range counts {
        if n > 1 {
            fmt.Printf("%d\t%s\n", n, line)
        }
    }
}

```

- map 存储一个键/值对集合, 并提供常量时间的操作来存储, 获取或测试集合中的某个元素. 键可以是其值能够进行相等比较的任意类型, 值可以是任意类型. 内置函数 make 可以用来新建 map
- counts[input.Text()]++ 等价于 line := input.Text(); counts[line] = counts[line] + 1
- bufio 包, 可以简便高效地处理输入和输出. 扫描器 (Scanner) 类型, 可以读取输入, 以行或者单词为单位断开.
- Printf 函数有超过 10 个转义字符, 被称之为 verb.

| verb       | 描述                         |
| ---------- | ---------------------------- |
| %d         | 十进制整数                   |
| %x, %o, %b | 十六进制, 八进制, 二进制整数 |
| %f, %g, %e | 浮点数                       |
| %t         | 布尔类型: true 或 false      |
| %c         | 字符 (Unicode 码点)          |
| %s         | 字符串                       |
| %q         | 带引号字符串或者字符         |
| %v         | 内置格式的任何值             |
| %T         | 任何值的类型                 |
| %%         | % 号本身 (无操作数)          |

- 字符串字面量可以包含类似转义序列 (escape sequence) 来表示不可见字符.

---

- 下面的 dup 版本可以从标准输入或一个文件列表进行读取,使用 os.Open 函数来逐个打开:

```go
// dup2 输出标准输入中多次出现的行的个数和文本
// 它从 stdin 或指定的文件列表读取
package main

import (
    "bufio"
    "fmt"
    "os"
)

func main() {
    counts := make(map[string]int)
    files := os.Args[1:]
    if len(files) == 0 {
        countLines(os.Stdin, counts)
    } else {
        for _, arg := range files {
            f, err := os.Open(arg)
            if err != nil {
                fmt.Printf(os.Stderr, "dup2: %v\n", err)
                continue
            }
            countLines(f, counts)
            f.Close()
        }
    }
}

func countLines(f *os.File, counts map[string]int) {
    input := bufio.NewScanner(f)
    for input.Scan() {
        counts[input.Text()]++
    }
}

```

- os.Open 返回两个值, 第一个是打开的文件 (\*os.File),第二个是一个内置的 error 类型的值. 如果 err 等于特殊的内置 nil 值, 标准文件成功打开. 如果 err 不是 nil ,说明打开文件错误. 这时 error 的值描述错误原因.
- countLines 的调用出现在声明之前. 函数和其他包级别的实体可以以任意次序声明.
- map 是一个使用 make 创建的数据结构的引用.
- 当 map 被传递给一个函数时, 函数接收到的是这个引用的副本, 所以被调用函数中对于 map 数据结构中的改变对函数调用者使用的 map 引用也是可见的.

---

dup3

```go
// dup3
package main

import (
    "fmt"
    "io/ioutil"
    "os"
    "strings"
)

func main() {
    counts := make(map[string]int)
    for _, filename := range os.Args[1] {
        data, err := ioutil.ReadFile(filename)
        if err != nil {
            fmt.Printf(os.Stderr, "dup3: %v\n", err)
            continue
        }
        for _, line := range strings.Split(string(data), "\n") {
            counts[line]++
        }
    }
    for line, n := range counts {
        if n > 1 {
            fmt.Printf("%d\t%s\n", n, line)
        }
    }
}

```

- ReadFile 函数返回一个可以转化成字符串的字节 slice, 这样可以被 strings.Split 分割.
- 实际上, bufio.Scanner, ioutil.ReadFile 以及 ioutil.WriteFile 使用 \*os.FIle 中的 Read 和 Write 方法.

#### 1.4 GIF 动画

- 下面的程序展示 Go 标准的图像包的使用. 这段代码中有几个新的组成, 包括 const 声明, 结构体以及复合字面量. 还引入了浮点运算.

```Go
// lissajous 产生随机萨如图形的 GIF 动画
package main

import (
    "image"
    "image/color"
    "image/gif"
    "io"
    "log"
    "math"
    "math/rand"
    "net/http"
    "os"
    "time"
)

var palette = []color.Color{color.White, color.Black}

const (
    whiteIndex = 0 // 画板中的第一种颜色
    blackIndex = 1 // 画板中的下一种颜色
)

func main() {
    rand.Seed(time.Now().UTC().UnixNano())
    if len(os.Args) > 1 && os.Args[1] == "web" {
        handler := func(w http.ResponseWriter, r *http.Request) {
            lissajous(w)
        }
        http.HandleFunc("/", handler)
        log.Fatal(http.ListenAndServe("localhost:8000", nil))
        return
    }
    lissajous(os.Stdout)
}

func lissajous(out io.Writer) {
    const (
        cycles  = 5     //完整的 x 震荡器变化的个数
        res     = 0.001 //角度分辨率
        size    = 100   //图像画布包含 [-size...+size]
        nframes = 64    //动画中的帧数
        delay   = 8     // 以 10ms 为单位的帧间延迟
    )
    freq := rand.Float64() * 3.0 //y 震荡器的相对频率
    anim := gif.GIF{LoopCount: nframes}
    phase := 0.0
    for i := 0; i < nframes; i++ {
        rect := image.Rect(0, 0, 2*size+1, 2*size+1)
        img := image.NewPaletted(rect, palette)
        for t := 0.0; t < cycles*2*math.Pi; t += res {
            x := math.Sin(t)
            y := math.Sin(t*freq + phase)
            img.SetColorIndex(size+int(x*size+0.5), size+int(y*size+0.5), blackIndex)
        }
        phase += 0.1
        anim.Delay = append(anim.Delay, delay)
        anim.Image = append(anim.Image, img)
    }
    gif.EncodeAll(out, &anim)
}

```

- const 声明用来给常量命名, 常量是其值在编译期间固定不变的量. const 可以出现在包级别 (在包生命周期内都可见)或在一个函数内 (仅在函数体内可见). 常量必须是数字, 字符串或布尔值.
- 表达式 []color.Color{...} 和 gif.GIF{...} 是复合字面常量, 即用一系列元素的值初始化 Go 的复合类型的紧凑表达式. 第一个是 slice, 第二个是结构体.
- gif.GIF 是一个结构体. 结构体由一组称为**字段**的值组成, 字段通常有不同的数据类型, 它们一起组成单个对象, 作为一个单位被对待. 结构体每个字段可以通过点标记来访问. 结构体字段默认值为对应数据类型的零值.

---

#### 1.5 获取一个 URL

- fetch 展示获取每个 URL 的类容, 然后加以解析地输出.

```go
// fetch 输出从 URL 获取的内容
package main

import (
 "fmt"
 "io/ioutil"
 "net/http"
 "os"
)

func main() {
 for _, url := range os.Args[1:] {
  resp, err := http.Get(url)
  if err != nil {
   fmt.Fprintf(os.Stderr, "fetch: %v\n", err)
  }
  b, err := ioutil.ReadAll(resp.Body)
  resp.Body.Close()
  if err != nil {
   fmt.Fprintf(os.Stderr, "fetch: reading %s: %v\n", url, err)
  }
  fmt.Printf("%s", b)
 }
}
```

- http.Get 函数产生一个 http 请求, 如果没有出错, 返回结果存在响应结构 resp 里面. 其中 resp 的 Body 域包含服务器端响应的一个可读取数据流.
- ioutil.ReadAll 读取整个响应结果并存入 b 中.
- 无论哪种错误情况, os.Exit(1) 会在进程退出时返回状态码 1.

---

#### 1.6 并发获取多个 URL

- Go 最令人感兴趣和新颖的特点就是支持并发编程.
- 下一个程序 fetchall 将并发获取多个 URL 内容.

```go
// fetch 并发获取 URL 并报告它们的时间和大小
package main

import (
    "fmt"
    "io"
    "io/ioutil"
    "net/http"
    "os"
    "time"
)

func main() {
 start := time.Now()
 ch := make(chan string)
 for _, url := range os.Args[1:] {
  go fetch(url, ch) // 启动一个 goroutine
 }
 for range os.Args[1:] {
  fmt.Println(<-ch) //从管道 ch 接收
 }
 fmt.Printf("%.2fs elapsed\n", time.Since(start).Seconds())
}

func fetch(url string, ch chan<- string) {
    start := time.Now()
    resp, err := http.Get(url)
    if err != nil {
        ch <- fmt.Sprint(err) // 发送到管道 ch
        return
    }
    nbytes, err := io.Copy(ioutil.Discard, resp.Body)
    resp.Body.Close()
    if err != nil {
        ch <- fmt.Sprintf("while reading %s: %v", url, err)
        return
 }
    secs := time.Since(start).Seconds()
    ch <- fmt.Sprintf("%.2fs %7d %s", secs, nbytes, url)
}

```

- goroutine 是一个并发执行的函数.
- **管道**是一种允许某一例程向另一例程传递指定类型值的通信机制
- main 函数在一个 goroutine 执行, 然后 go 语句创建额外的 goroutine .
- 当一个 goroutine 试图在一个管道上进行发送或接收操作时, 它会阻塞, 直到另一个 goroutine 试图进行接收或者发送操作时才传递值, 并开始处理两个 goroutine.

---

#### 1.7 一个 web 服务器

- 本节将展示, 一个迷你服务器, 返回访问服务器的 URL 部分.

```go
// server1 是一个迷你回声服务器
package main

import (
    "fmt"
    "log"
    "net/http"
)

func main() {
    http.HandleFunc("/", handler)
    log.Fatal(http.ListenAndServe("localhost:8000", nil))
}

// 处理程序回显请求 URL r 的路径部分
func handler(w http.ResponseWriter, r *http.Request) {
 fmt.Fprintf(w, "URL.Path = %q\n", r.URL.Path)
}
```

- 需要在后台运行服务器, 在 Linux 环境, 启动时需要添加 & 符号.
- 对于不同的请求, 服务器在不同的 goroutine 中运行处理, 可能同时处理多个请求.
- 为了避免竞态 bug, 必须确保最多只有一个 goroutine 在同一时间访问变量, 正是 mu.lock 和 mu.unlock 的作用.

```go
// server2 是一个迷你回声和计数服务器
package main

import (
    "fmt"
    "log"
    "net/http"
    "sync"
)

var mu sync.Mutex
var count int

func main() {
    http.HandleFunc("/", handler)
    http.HandleFunc("/count", counter)
    log.Fatal(http.ListenAndServe("localhost:8000", nil))
}

// 处理程序回显请求 URL r 的路径部分
func handler(w http.ResponseWriter, r *http.Request) {
    mu.Lock()
    count++
    mu.Unlock()
    fmt.Fprintf(w, "URL.Path = %q\n", r.URL.Path)
}
func counter(w http.ResponseWriter, r *http.Request) {
 mu.Lock()
 fmt.Fprintf(w, "Count %d\n", count)
 mu.Unlock()
}

```

- 处理函数可以报告接收到的消息头和表单数据

```go
// server3 处理程序回显 HTTP 请求
package main

import (
    "fmt"
    "log"
    "net/http"
)

func main() {
    http.HandleFunc("/", handler)
    log.Fatal(http.ListenAndServe("localhost:8000", nil))
}

// 处理程序回显请求 URL r 的路径部分
func handler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "%s %s %s \n", r.Method, r.URL, r.Proto)
    for k, v := range r.Header {
        fmt.Fprintf(w, "Header[%q] = %q\n", k, v)
    }
    fmt.Fprintf(w, "Host = %q\n", r.Host)
    fmt.Fprintf(w, "RemoteAddr = %q\n", r.RemoteAddr)
    if err := r.ParseForm(); err != nil {
        log.Print(err)
    }
    for k, v := range r.Form {
        fmt.Fprintf(w, "Form[%q] = %q\n", k, v)
    }
}
```

- Go 允许一个简单的语句 (如一个局部变量声明) 跟在 if 条件前面, 在处理错误的时候特别有用.

```go
    if err := r.ParseForm(); err != nil {
        log.Print(err)
    }
    // 等价于
    err := r.ParseForm()
    if err != nil {
        log.Print(err)
    }
```

- 合并的语句更短而且可以缩小 err 变量的作用域, 这是一个好的实践.

#### 1.8 其他内容

- 控制流

```go
    switch coinflip() {
    case "heads":
        heads++
    case "tails":
        tails++
    default:
        fmt.Println("landed on edge")
    }
```

> coinflip 的调用结果会和每一个条件进行比较. case 语句从上到下进行推演, 所以第一个匹配的语句将被执行. 默认 case 语句可以放在任何地方. case 语句不像 C 语言那样从上到下贯穿执行 (fallthrough 语句可以改写这个行为).

- switch 语句不需要操作数, 它就像一个 case 语句列表, 每一条 case 语句都是一个布尔表达式:

```go
func Signum(x int) int {
    switch {
    case x > 0:
        return +1
    default:
        return 0
    case x < 0:
        return -1
    }
}
```

- 这种形式称为无标签 (tagless) 选择, 它等价于 switch true.
- 与 for 和 if 语句类似, switch 可以包含一个可选的简单语句: 一个短变量声明, 一个递增或赋值语句, 或者一个函数调用, 用来在判断条件前置一个值.
- break 和 continue 可以改变控制流.
- 命名类型: type 声明给已有类型命名. 因为结构体通常很长, 所以它们基本上都是独立命名.

```go
type Point struct {
    X, Y int
}

var p Point
```

- 指针 Go 提供了指针, 它的值是变量的地址. 指针显示可见. 使用 & 操作符可以获取一个变量的地址, 使用 \* 操作符可以获取引用的变量的值, 但不支持算术运算.
- 方法和接口: 一个关联了命名类型的函数称为方法. 接口可以使用相同的方式处理不同具体类型的抽象类型, 它基于这些类型包含的方法, 而不是类型的描述实现.
- 包: Go 自带一个可扩展并且实用的标准库. go doc 工具可以访问相关文档.
- 注释: 单行注释 // . 多行注释 /_..._/. 注释不能嵌套.

---

### 第 2 章 程序结构

_Go 语言中的大程序都是从小的基本组件构建而来: 变量存储值; 简单表达式通过加和减操作合并成大的; 基本类型通过数组和结构体进行聚合; 表达式通过 if 和 for 等控制语句来决定执行顺序; 语句被组织成函数用于隔离和复用; 函数被组织成源文件和包._

#### 2.1 名称

- Go 中函数, 变量, 常量, 类型, 语句标签和包的名称遵循一个简单的规则: 名称开头是一个字母 (Unicode 中的字符) 或下划线, 后面可以跟任意数量的字符,数字和下划线, 并区分大小写.
- Go 有 25 个关键字. 不能被用作名称

> break default func interface select case defer go map struct  
> chan else goto package switch const fallthrough if range type
> continue for import return var

- 预声明常量

> true false iota nil

- 类型

> int int8 int16 int32 int64 uint8 uint16 uint32 uint64 uintptr float32 float64 complex128  
> complex64 bool byte rune string error

- 函数

> make len cap new append copy close delete complex real imag panic recover

这些名称不是预留的, 可以在声明中使用它们. 但是有冲突的风险

- 如果一个实体在函数中声明, 它只在函数局部有效.
- 如果声明在函数外, 它将对包里面所以源文件可见.
- 实体第一个字母的大小写决定其可见性是否跨包.如果名称以大写字母开头, 它是导出的, 对包外是可见和可访问的, 可以被自己包之外的其他程序所引用.
- 名称本身没有长度限制
- 使用 "驼峰式" 风格. 缩写词通常使用相同的大小写

#### 2.2 声明

- 声明给一个程序实体命名, 并且设定其部分或全部属性.
- 有 4 个主要声明: 变量 var, 常量 const, 类型 type 和函数 func.
- Go 程序存储在一个或多个以 .go 为后缀的文件里. 每个每个文件以 package 声明开头, 表明文件属于哪个包. package 后面是 import 声明, 然后是包级别的类型, 变量, 常量, 函数声明, 不区分顺序.
- 函数的声明包含一个名字, 一个参数列表, 一个可选的返回值类型列表, 以及函数体. 如果函数不返回任何类容, 返回值列表可以省略

```go
// ftoc 输出两个华氏摄氏度 - 摄氏温度的转换
package main

import "fmt"

func main() {
    const freezingF, boilingF = 32.0, 212.0
    fmt.Printf("%gF = %gC\n", freezingF, fToC(freezingF)) // 32F = 0C
    fmt.Printf("%gF = %gC\n", boilingF, fToC(boilingF))   // 212F = 100C

}

func fToC(f float64) float64 {
    return (f - 32) * 5 / 9
}

```

#### 2.3 变量

- var 声明创建一个具体类型的变量, 然后给它们附加一个名字, 设置它的初始值.

> var name type = expression

_类型和表达式部分可以省略一个, 但是不能都省略. 如果类型省略, 它的类型将由初始化表达式决定; 如果表达式省略, 其初始值对应于类型的零值, 对于数字是 0, 对于布尔值是 false, 对于字符串是 "", 对于接口和引用类型 (slice, 指针, map, 通道, 函数) 是 nil. 对于数组或结构体这样的复合类型,零值是其所有元素或者成员的零值._

- Go 中不存在未初始化的变量
- 可以声明一个变量列表, 并使用对应的表达式列表对其初始化. 忽略类型允许声明多个不同类型的变量.

```go
var i, j, k int                 //int int int
var b, f, s = true, 2.3, "four" //bool float64 string
```

- 初始值设定可以是字面量或任意的表达试.包级别的初始化在 main 函数执行之前进行, 局部变量的初始化和声明一样在函数执行期间进行.
- 变量可以通过调用返回多个值的函数进行初始化:

> var f, err = os.Open(name) // 返回一个文件和一个错误

##### 2.3.1 短变量声明

- 在函数中, 一种称作*短变量声明*的可选形式可以用来声明和初始化*局部变量*.
- 使用 **name := expression** 的形式, **name** 的类型由 **expression** 的类型决定
- 多个变量可以以短变量声明的方式声明和初始化
- := 表示声明, 而 = 表示赋值.
- 短变量声明也可以用来调用 os.Open 那样返回两个或多个值的函数
- 短变量声明不需要声明所有在左边的变量. 如果一些变量在**同一个**词法块中声明, 那么对于那些变量, 短声明的行为等同于**赋值**, 且外层的声明将被忽略.
- 短变量声明最少声明一个新的变量, 否则, 代码编译将无法通过

---

##### 2.3.2 指针

- **变量**是存储值的地方
- **指针**的值是一个变量的地址
- 不是所有值都有地址, 但是所有的变量都有.
- 使用指针, 可以在无须知道变量名字的情况下, **间接**读取或更新变量的值

> 如果一个变量声明为 var x int, 表达式 &x ( x 的地址) 获取一个指向整型变量的指针, 它的类型是整形指针 ( *int ). 如果值叫作 p, 我们说 p 指向 x, 或者 p 包含 x 的地址. p 指向的变量写作*p. 表达式 *p 获取变量的值. 一个整形, 因为*p 代表一个变量, 所以它可以出现在赋值操作符左边, 用于更新变量的值.

```go
    x := 1
    p := &x         // p 是整形指针, 指向 x
    fmt.Println(*p) // 1
    *p = 2          // 等于 x = 2
    fmt.Println(p)  // 结果 "2"
```

- 每一个聚会类型变量的组成 (结构体的成员或数组中的元素) 都是变量, 所以也有一个地址.
- 变量有时候使用一个**地址**化的值. 代表变量的表达式, 是唯一可以应用**取地址**操作符 & 的表达式.
- 指针类型的零值是 **nil**
- 函数返回的局部变量的地址是非常安全的. 即使在调用后依然存在.
- 每一次调用函数都会返回一个不同的值

```go
var p = f()

func f() *int {
    v := 1
    return &v
}

fmt.Println(f() == f()) // "false"

```

- 因为一个指针包含变量的地址, 所以传递一个指针参数给函数, 能够让函数更新间接传递的变量值.
- 每一个使用变量的地址或者复制一个指针, 我们就创建了新的**别名**或者方式标记同一变量.
- 不仅指针会产生别名, 当复制其他引用类型 (像 slice, map, 通道, 甚至包含这些引用类型的结构体, 数组和接口) 的值的时候, 也会产生别名.
- 指针对于 **flag** 包是关键的, 它使用程序的命令行参数来设置整个程序内某些变量值.

---

##### 2.3.3 new 函数

- 表达式 **new(T)** 创建一个**未命名**的 T 类型变量, 初始化为 T 类型的零值, 并返回其地址 (地址类型为 \*T).
- 使用 new 创建变量和取其地址的普通局部变量没有什么不同, 只是不需要引入 (和声明) 一个虚拟的名字, 通过 new(T) 就可以直接在表达式中使用.
- 每一次调用 new 返回一个具有唯一地址的不同变量, 但是如果两个变量的类型不携带任何信息且是零值, 在当前的实现里, 它们有相同的地址.
- new 是一个预声明函数, 不是一个关键字, 可以被重新定义.

##### 2.3.4 变量的生命周期

- **生命周期**指的是程序执行过程中变量存在的时间段.
- 包级别的变量的生命周期是整个程序的执行时间
- 局部变量有一个动态的生命周期: 每次执行声明语句时创建一个新的实体, 变量一直生存到它变得**不可访问**, 这时它占用的存储空间被回收
- 函数的参数和返回值也是局部变量, 它们在其闭包函数被调用的时候创建.
- 如何判断一个变量是否该被回收

> 每一个包级别的变量, 以及每一个当前执行函数的局部变量, 可以作为追溯该变量的路径的源头, 通过指针和其他方式的引用可以找到变量. 如果变量的路径不存在, 那么变量变得不可访问, 因此它不会影响任何其他计算的过程.  
> 因为变量的生命周期时通过它是否可达来确定的, 所以局部变量可以在包含它的循环一次迭代之外继续存活.

- 编译器可以选择使用堆或栈上的空间来分配变量, 这个选择不是基于 var 或 new 关键字来声明变量.

```go
var global *int

func f() {
    var x int
    x = 1
    global = &x
}

func g() {
    y := new(int)
    *y = 1
}
```

> x 一定使用堆空间, 因为它在 f 函数返回以后还可以从 global 变量访问, 尽管被声明为一个局部变量. 这种情况我们说 x 从 f 中**逃逸**.  
> 当 g 函数返回时, 变量 *y 变得不可访问, 可回收. 因为*y 没有从 g 中逃逸, 所以编译器可以安全地在栈上分配 \*y, 即便使用 **new** 函数创建它.

- 每一次变量逃逸都需要一次额外的内存分配过程.
  _在长生命周期对象中保持短生命周期对象不必要的指针, 特别是在全局变量中, 会阻止垃圾回收短生命周期的对象空间_

#### 2.4 赋值

- 赋值语句用来更新变量所指的值, 它最简单的形式由赋值符 =, 以及符号左边的变量和右边的表达式组成.
- 每一个算数和二进制位操作符有一个对应的赋值操作符.

##### 2.4.1 多重赋值

- 多重赋值, 允许几个变量一次性被赋值. 在实际更新操作之前, 右边所有表达式被推演.

```go
    // 交换两个变量的值
    x, y = y, x
    a[i], a[j] = a[j], a[i]

    //计算两个整数的最大公约数
    func gcd(x, y int) int {
    for y != 0 {
        x, y = y, x%y
    }
    return x
}

// 计算斐波那契数列第 n 个值
func fib(n int) int {
    x, y := 0, 1
    for i := 0; i < n; i++ {
        x, y = y, x+y
    }
    return x
}
```

##### 2.4.2 可赋值性

- 显示赋值: 使用赋值语句
- 隐式赋值: 函数调用隐式地将参数的值赋给对应参数的变量; 一个 return 语句隐式地将 return 操作数赋值给结果变量. 复合类型地字面量表达式.
- 赋值只有在值对于变量类型是**可赋值**的时才是合法.
- **可赋值性**根据不同有不同的规则. 对于前面已经讨论过的类型,规则很 简单: 类型必须精确匹配, **nil** 可以被赋值给任何接口变量或引用类型.
- 常量有更灵活的可赋值性规则来避免显示的转换.
- 两个值使用 == 和 != 进行比较与可赋值性相关: 任何比较中, 第一个操作数相对于第二个操作数的类型必须是可赋值的, 或者可以反过来赋值.

---

#### 2.5 类型声明

- type 声明定义一个新的命名类型, 它和某个已有类型使用同样的**底层类型**.
- 命名类型提供了一种方式来区分底层类型的不同或者不兼容使用, 避免它们在无意间的混用.

> **type name underying-type**

- 类型声明通常出现在包级别

```go
// 包 tempconv 进行摄氏温度和华氏温度的转换计算
package tempconv

type Celsius float64
type Fahrenheit float64

const (
    AbsoluteZeroC Celsius = -273.15
    FreezingC     Celsius = 0
    BoilingC      Celsius = 100
)

func CToF(c Celsius) Fahrenheit {
    return Fahrenheit(c*9/5 + 32)
}

func FToC(f Fahrenheit) Celsius {
    return Celsius((f - 32) * 5 / 9)
}

```

> 包定义了两个类型,Celsius (摄氏温度),Fahrenheit (华氏温度). 即使它们使用了相同的底层数据类型 float64, 它们也是不同类型不能使用算数表达式进行比较和合并.  
> 从 float64 转换为 **Celsius(t)** 或 **Fahrenheit(t)** 需要显示类型转换. **Celsius(t)** 和 **Fahrenheit(t)** 是类型转换, 而不是函数调用. 它们不会改变值和表达方式, 但改变了显示意义.

- 对于每一个类型 T, 都有一个对应的类型转换操作 T(x) 将值 x 转换为类型 T.
- 如果两个类型具有相同的底层类型或二者都是指向相同底层类型变量的未命名指针类型, 则二者可以相互转换.
- 类型转换不改变值的表达方式, 仅改变类型
- 如果 x 对于 类型 T 是赋值的, 类型转换也是允许的, 但通常是不必要的.
- 命名类型的底层类型决定了它的结构和表达方式, 以及它支持的内部操作集合, 这些内部操作与直接使用底层类型的情况相同.
- 通过 == 和 < 之类的比较操作符, 命名类型的值可以与其相同类型的值或者底层类型相同的未命名类型的值相比较. 但是不同命名类型的值不能直接比较.

```go
    var c Celsius
    var f Fahrenheit
    fmt.Println(c == 0)          // "true"
    fmt.Println(f >= 0)          // "true"
    fmt.Printf(c == f)           // 编译错误: 类型不匹配
    fmt.Println(c == Celsius(f)) // "true"
```

#### 2.6 包和文件

- 在 Go 语言中包的作用与其他语言中的库或模块类似, 用于支持模块化, 封装, 编译隔离和重用.
- 每一个包的源码保存在一个或多个以 .go 结尾的文件中, 它所在目录名的尾部就是包导入的路径
- 每一个包给它的声明提供独立的**命名空间**
- 包通过控制变量在包外面的可见性或者导出情况来隐藏信息. 导出的标志符以大写字母开头.

##### 2.6.1 导入

- Go 程序里, 每一个包通过称之为**导入路径 (import path)** 的唯一字符串来标识.
- 每一个包还有一个包名, 它以短名字的形式 (且不必是唯一的) 出现在包的声明中. 按约定包名匹配导入路径的最后一段.
- 导入声明可以给导入的包绑定一个短名字, 用来在整个文件中引用包的内容, 默认这个短名就是包名. 导入声明可以设定一个可选的名字来避免冲突.
- 如果导入一个没有被引用的包, 就会触发一个错误.

---

#### 2.7 作用域

- 声明将名字和程序实体关联起来.
- 声明的作用域是指用到声明时所声明名字的源代码.
- 声明的作用域是声明在程序文本中出现的区域,它是一个编译时属性.
- 变量的生命周期是变量在程序执行期间可能被程序其他部分引用的起止时间, 它是一个运行时属性.
- 语法块是由大括号围起来的一个语句序列. 在语法块内部声明的变量对块外部都不可见.
- 其他没有被显示包含在大括号中的声明代码, 统称为**词法块**.
- 包含了全部源代码的词法块, 叫做**全局块**
- 每一个包, 每一个文件, 每一个 for, if 和 switch 语句, 已经 switch 和 select 语句中的每一个条件, 都写在一个词法块里
- 显示写在大括号语法代码块也是一个词法块.
- 一个声明的词法块决定声明的作用域大小.
- 像 int, len 和 true 等内置类型, 函数或者常量在全局块中声明并且对整个程序可见.
- 在包级别 (就是在任何函数外) 的声明, 可以被同一个包里的任何文件引用.
- 导入的包是文件级别的, 它们可以在同一个文件内引用.
- 局部声明, 仅可在同一函数中或者函数的一部分被引用
- 控制流标签的作用域是整个外层函数 (enclosing function).
- 一个程序可以包含多个同名声明, 前提是它们在不同的词法块中
- 当编译器遇到一个名字引用时, 将从最内层的封闭词法块到全局块寻找其声明. 如果内存和外层都存在一个声明, 内层的将首先被找到. 这种情况下, 内层声明将**覆盖**外部声明, 使它不可访问.
- 不是所有词法块都对应于显示大括号包围的语句序列, 有一些词法块是隐式的.
- 在包级别, 声明的顺序和它们的作用域没有关系.
- 短变量声明依赖一个明确的作用域.

---

### 第 3 章 基本数据

_计算机底层操作全是位, 而实际操作则是基于大小固定的单元中的数值, 称为字 (word)_  
**Go 的数据类型分为四大类:**

- 基础数据类型 (basic type)
- 聚合类型 (aggregate type)
- 引用类型 (reference type)
- 接口类型 (Interface type)

#### 3.1 整数

- Go 同时具备有符号数和无符号数

> 有符号数: int8, int16, int32, int64; 对应无无符号数: uint8, uint16, uint32, uint64

- int 和 uint

> 在特定平台上, 其大小与原生的有符号数\ 无符号整数相同, 或等于该平台上的运算效率最高的值. 即使在同一个硬件平台上, 不同的编译器可能选用不同的大小.

- rune 类型是 int32 类型的同义词, 常常用于指明一个值是 Unicode 码点 (code point).
- byte 类型是 uint8 类型的同义词, 强调一个值是原始数据类型数据, 而非一个值.
- 无符号整数 uintptr, 其大小并不明确, 但可以完整存放指针. uintptr 类型仅仅用于底层编程.
- int, uint 和 uintptr 与别于大小明确的相似类型. 如要类型转换 必须显示转换.
- 有符号整数以补码表示. 保留最高位做符号位.无符号整数由全部位构成其非负值
- Go 的二元操作符涵盖了算术, 逻辑和比较等运算, 优先级的降序排列如下:

> \* / % << >> & &^
> \+ - | ^  
> == != < <= > >=  
> &&  
> ||

- 二元运算符分 五大优先级. 满足左结合律
- 算术运算符 +, -, \*, / 可应用于整数, 浮点数和复数.
- 取模运算符 % 仅能用于整数. Go, 取模余数的正负号总是与被除数一致.
- 除法运算 / 的行为取决于操作数是否都是整形, 整数相除, 商会舍弃小数部分,
- 全部基本类型的值都是可以比较的.
- Go 具备下列位运算符, 前四个对操作数的运算逐位独立进行, 不涉及算术进位或正负号

> & 位运算 AND  
> | 位运算 OR  
> ^ 位运算 XOR
> &^ 位清空 (AND NOT)  
> \>\> 左移  
> << 右移

_如果作为二元运算符, 运算符 ^ 表示按位 "异或" (XOR); 若作为一元前缀运算符, 则它表示按位取反或按位取补, 运算结果是操作数逐位取反._  
_运算符 &^ 是按位清除 (AND NOT): 表达式 z=x&^y 中, 若 y 的某位是 1, 则 z 的对应位等于 0; 否则, 它就等于 x 的对应位._  
_移运算 x<<n x="""""x""""">>n 中, 操作数 n 决定位移量, 而且 n 必须为无符号型; 操作数 x 可以是有符号型也可以是无符号型. 左移 以 0 补右边空位, 但有符合数的右移操作是按符号位的值填补空位._

- 通常, 将某种类型的值转换成另一种, 需要显式转换.
- 对于算术逻辑 (不含位移) 的二元操作符, 其操作数类型必须相同
- 八进制数以 0 开头; 十六进制数,以 0x 开头.

---

#### 3.2 浮点数

_Go 具有两种大小的浮点数 float32 和 float64. 其算术特性遵从 IEEE 754 标准._

- 浮点数可通过 %g, %e (有指数) 或 %f (无指数) 输出.
- 正无穷大: +Inf
- 负无穷大: -Inf
- NaN: Not a Number

---

#### 3.3 复数

- Go 具备两种大小的复数 complex64 和 complex128, 二者分别由 float32 和 float64 构成.
- 内置的 complex 函数根据给定的实部和虚部创建复数, 而内置的 real 函数和 imag 函数则分别提取复数的实部和虚部.
- 如果在浮点数或十进制数后面紧紧接着写字母 i, 它就变成了一个虚数, 表示一个实部为 0 的复数
- 可以用 == 或者 != 判断复数是否相等. 若两个复数的实部和虚部都相等, 则它们相等.

---

#### 3.4 布尔值

- bool 型的值或布尔值 (boolean)只有两种: 真 (true) 和 假 (false).
- 一元操作符 ! 表示逻辑取反.
- 布尔值可以由运算符 && (AND) 以及 || (OR)组合运算, 这可能引起短路行为: 如果操作符左边的操作数已经能确定总体结果, 则右边的操作数不会计算在内.
- 布尔值无法隐式转换成数值, 反之亦然.

---

#### 3.5 字符串

- 字符串是不可变的字节序列.
- 内置的 len 函数返回字符串的字节数 (并非文字符号的数目). 下标访问操作 s[i] 则取得第 i 个字符, 其中 0 <=i < len(s)
- 字符串第 i 个字节不一定就是 i 个字符, 因为非 ASCII 字符的 UTF-8 码点需要两个或者更多字节.
- 子串生成操作 s[i:j] 产生一个新字符串, 内容取自原字符串的字节, 下标从 i (含边界值) 开始, 直到 j (不含边界值). 结果大小是 j-i 个字节. 操作数 i 与 j 的默认值是 0
- 加号 (+) 运算符连接两个字符串生成一个新的字符串.
- 字符串可以通过比较运算符做比较, 比较运算按字节进行, 结果服从本身的字典序.
- 字符串的值无法改变: 字符串值本身所包含的字节序列永远不可变.
- 字符串内部数据不允许修改

##### 3.5.1 字符串字面量

字符串的值可以直接写成字符串字面量 (string literal), 形式上就是带双引号的字节序列

- Go 的源文件总是按 UTF-8 编码
- 在带双引号的字符串字面量中, 转义序列以反斜杠 (\\)开始.

| 转义符 | 含义          |
| ------ | ------------- |
| \a     | "警告" 或响铃 |
| \b     | 退格符        |
| \f     | 换页符        |
| \n     | 换行符        |
| \r     | 回车符        |
| \t     | 制表符        |
| \v     | 垂直制表符    |
| \\'    | 单引号        |
| \\"    | 双引号        |
| \\\\   | 反斜杠        |

- 源码中的字符串也可以包含十六进制或八进制的任意字节.
- 十六转义字符写成 \xhh, h 是十六进制数字,且必须是两位.
- 八进制的转义字符写成 \000 的形式, 必须使用三位八进制数字 (0~7), 且不能超过 \377.
- 原生字符串字面量的书写形式是\`...\`,使用反引号而不是双引号. 原生字符串字面量内, 转义序列不起作用, 实质内容与字面量写法严格一致.
- 源码中, 原生的字符串字面量可以展开多行. 唯一的特殊处理是回车符会被删除, 使得同一字符串在所有平台上的值都相同.

##### 3.5.2 Unicode

- Unicode, 囊括了世界上所有文书体系的全部字符, 还有重音符和其他变音符, 控制码, 以及许多特有文字, 对它们各自赋予一个叫 Unicode 码点的标准数字. 在 Go 的术语中, 这些字符记号被称为文字符号 (rune).
- rune 类型作为 int32 类型的别名
- 每个 Unicode 码点的编码长度相同, 都是 32 位.

##### 3.5.3 UTF-8

_UTF-8 以字节为单位对 Unicode 码点作变长编码. UTF-8 是现行的一种 Unicode 标准._

- 每个文字符号用 1 ~ 4 个字节表示, 一个文字符号编码的首字节的最高位表明后面还有多少字节. 若最高位为 0, 则表示它是 7 位的 ASCII 码,
- UTF-8 兼容 ASCII, 且自动同步.
- UTF-8 是前缀编码,
- 文字符号的字典字节序列与 Unicode 码点序一致.
- UTF-8 本身不会嵌入 NULL 字节 (0 值), 便于某些程序语言用 NULL 标记字符串结尾.
- Go 语言中, 字符串字面量的转义让我们能用码点值来指明 Unicode 字符: \uhhhh 表示 16 位码点值, \Uhhhhhhhh 表示 32 位码点值. 其中,每一个 h 代表一个 十六进制数字.
- 每次 UTF-8 解码器读入一个不合理的字符, 无论是显示调用 **utf8.DecodeRuneInString**,还是在 range 循环内隐式读取, 都会产生一个专门的 Unicode 字符 '\uFFFD' 替换它, 其输出通常是一个黑色六角形或类似钻石的形状.
- 当 []rune 转换作用于 UTF-8 编码的字符串时, 返回该字符串的 Unicode 码点序列.
- 若将一个整数值转换成字符串, 其值按文字符号类型解读, 并且产生代表该文字符号值的 UTF-8 码.
- 如果文字符号值非法, 将被 '\uFFFD' 取代.

##### 3.5.4 字符串和字节 slice

- 4 个标准包对字符串的重要操作:

> **bytes**, **strings**, **strconv** 和 **unicode**.

- strings 包含多个函数, 用于收索, 替换, 比较, 修整, 切分与连接字符串.
- bytes 包也有类似的函数, 用于操作字节 slice.
- strconv 包具备的函数, 主要用于转换布尔值, 整数, 浮点数与之对应的字符串之间的相互转换.
- unicode 包有判别文字符号特性的函数.

##### 3.5.5 字符串和数字的相互转换

除了字符串, 文字符号和字节之间的转换, 我们常常需要相互转换数值及字符串表示形式. 这由 strconv 包函数完成.

#### 3.6 常量

常量是一种表单式, 其可以保证在编译阶段就计算出表达式的值, 并不需要等到运行时, 从而使编译器知晓其值. 所有常量本质上都属于基本类型: 布尔型, 字符串或数字

- 关键字 const

```go
const pi = 3.1415926
const (
e = 2.71828182845904523536028747135266249775724709369995957496696763
pi = 3.14159265358979323846264338327950288419716939937510582097494459
)
```

- 对于常量操作数, 所有数学运算, 逻辑运算和比较运算的结果依然是常量.
- 常量的类型转换结果和某些内置函数的返回值, 例如 len, cap, real, imag, complex 和 unsafe.Sizeof 同样是常量.
- 常量声明可以同时指定类型和值, 如果没有指定类型, 则类型根据右边表达式推断.
- 如果同时声明一组常量, 除第一项之外, 其他项在等号右侧的表达式可以省略, 这意味着会复用前一项的表达式及其类型.

```go
const (
a = 1
b
c = 2
d
)
fmt.Println(a, b, c, d) // "1 1 2 2"
```

##### 3.6.1 常量生成器 iota

_常量的声明可以使用常量生成器 iota, 它创建一系列相关值, 而不是逐个值显示写出. 常量声明中, iota 从 0 开始取值, 逐项加 1 下例取自 time 包, 它定义了 weekday 的具名类型, 并声明每周 7 天为该类型的常量, 从 Sunday 开始, 其值为 0. 这种类型通常称为 枚举行 (enumeration, 或缩写成 enum)._

```go
type Weekday int
const (
Sunday Weekday = iota
Monday
Tuesday
Wednesday
Thursday
Friday
Saturday
)
```

##### 3.6.2 无类型常量

_Go 的常量自有特别之处: 常量可以是任何基本数据类型, 也包括具名的基本类型, 但是许多常量并不属于某一具体类型. 编译器将这些从属类型待定的常量表示成某些值, 这些值比基本数据类型的数字精度更高, 且算术精度高于原生机器精度. 可以认为它们的精度至少达到 256 位._

- 从属类型待定的常量共有 6 种, 分别是 无类型布尔型, 无类型整数, 无类型文字符号, 无类型浮点数, 无类型复数, 无类型字符串
- 只有常量才可以是无类型的.
- 不论隐式或是显示, 常量从一种类型转换成另一种, 都要求目标类型能够表示原值. 实数和复数允许舍入取整.
- 变量声明 (包括短变量声明) 中, 假如没有显示指定类型, 无类型常量会隐式转换成该变量的默认类型.
- 不对称性: 无类型整数可以转换成 int, 其大小不确定, 但无类型浮点数和无类型复数将被转换成大小明确的 float64 和 complex128.
- 要将变量转换成不同的类型, 我们必须将无类型常量显示转换为期望类型, 或在声明变量时指明类型.

---

### 第 4 章 复合数据类型

_复合数据类型是由基本数据类型以各种方式组合而成._

#### 4.1 数组

- 数组是具有固定长度且拥有零个或者多个相同数据类型元素的序列.

```go
    var a [3]int             // array of 3 integers
    fmt.Println(a[0])        // print the first element
    fmt.Println(a[len(a)-1]) // print the last element, a[2]
    // Print the indices and elements.
    for i, v := range a {
        fmt.Printf("%d %d\n", i, v)
    }
    // Print the elements only.
    for _, v := range a {
        fmt.Printf("%d\n", v)
    }
```

- 默认情况下, 一个新数组中的元素初始值为元素类型的零值.
- 可以使用**数组字面量**根据一组值来初始化一个数组

```go
  var q [3]int = [3]int{1, 2, 3}
  var r [3]int = [3]int{1, 2}
  fmt.Println(r[2]) // "0"
```

- 在数组字面量中, 如果省略号 "..." 出现在数组长度位置, 那么数组长度由初始化数组的元素个数决定.

```go
q := [...]int{1, 2, 3}
fmt.Printf("%T\n", q) // "[3]int"
```

- 数组的长度是数组类型的一部分, 所以 [3]int 和 [4]int 是两种不同的数组类型. 数组的长度必须是常量表达式.

```go
q := [3]int{1, 2, 3}
q = [4]int{1, 2, 3, 4} // compile error: cannot assign [4]int to [3]int
```

- 数组, slice, map 和结构体的字面语法都是相似的: 可以给出一组值, 这一组值具有索引和索引对应的值:

```go
type Currency int
const (
USD Currency = iota
EUR
GBP
RMB
)
```

- 没有指定索引位置的元素默认被数组元素类型的零值

```go
r := [...]int{99: -1}
```

- 如果一个数组元素的元素类型是可比较的, 那么这个数组也是可比较的, 这样我们可以直接使用 == 操作符来比较两个数组, 比较结果是两边元素的值是否完全相等. 使用 != 比较两个数组是否不同

```go
a := [2]int{1, 2}
b := [...]int{1, 2}
c := [2]int{1, 3}
fmt.Println(a == b, a == c, b == c) // "true false false"
d := [3]int{1, 2}
fmt.Println(a == d) // compile error: cannot compare [2]int == [3]int
```

- Go 把数组和其他的类型都看成值传递.
- 可以显示的传递一个数组的指针给函数. 这样函数内部对数组的改变, 对原数组是可见的.

#### 4.2 slice

_slice 表示一个拥有相同类型元素的可变长度的序列._

- slice 可以用来访问数组的部分或者全部元素, 而这个数组称为 **slice 的底层数组**
- slice 又 三个属性: 指针, 长度和容量.

> 指针指向数组的第一个可以从 slice 中访问的元素, 这个元素不一定是数组的第一个元素.  
> 长度是指 slice 中元素个数, 它不能超过 slice 的容量.  
> 容量的大小通常是从 slice 的起始元素到底层数组的最后一个元素间元素的个数.  
> Go 的内置函数 len 和 cap 用来返回 slice 的长度和容量.

- 一个底层数组可以对应多个 slice, 这些 slice 可以引用数组的任何位置, 彼此之间的元素可以被重叠.
- slice 操作符 s[i : j] (其中 0 <= i <= j<=cap(s)) 创建了一个新的 slice, 这个新的 slice 引用了序列 s 中从 i 到 j-1 索引位置的所有元素, 这里的 s 既可以是数组或者指向数组的指针, 也可以是 slice. 新 slice 的元素个数是 j-1 个. 如果省略了 i, 那么新的 slice 的起始位置就是 0, 如果省略了 j, 那么 slice 的结束位置就是 len(s)-1, 即 j = len(s).
- 如果 slice 的引用超过被引用对象的容量, 即 cap(s), 那么会导致程序宕机; 如果 slice 的引用超出了被引用对象的长度, 即 len(s), 那么最终 slice 会比原 slice 长.
- 字符串 (string) 字串操作和对字节 slice ([]byte) 做 slice 操作这两者的相似性.

> 她们都写作 x[m:n], 并且都返回原始字节的一个子序列, 同时它们的底层引用方式也是相同的.  
> 区别在于: 如果 x 是字符串, 那么 x[m:n] 返回的是一个字符串, 如果 x 是字节 slice, 那么返回的结果是字节 slice.  
> 因为 slice 包含了指向数组元素的指针, 所以将一个 slice 传递给函数的时候, 可以在函数内部修改底层数组的元素. 换言之, 创建一个 slice 等于为数组创建了一个别名.

```go
import "fmt"

func main() {
    a := [...]int{0, 1, 2, 3, 4, 5}
    reverse(a[:])
    fmt.Println(a) // "[5 4 3 2 1 0]"
}

// reverse reverses a slice of ints in place.
func reverse(s []int) {
    for i, j := 0, len(s)-1; i < j; i, j = i+1, j-1 {
        s[i], s[j] = s[j], s[i]
    }
}
```

- 注意初始化 slice s 的表达式和初始化数组 a 的表达式的区别. slice 没有指定长度.
- 和数组一样, slice 可以按照指定顺序指定元素, 也可以通过索引来指定元素, 或者两者结合.
- slice 无法作比较. 不能用 == 来测试两个 slice. 标准库里面(仅)提供了高度优化的函数 bytes.Equal 来比较两个字节 slice([]byte).

> slice 不可用 == 操作符比较的原因有两个. 首先, 和数组元素不同, slice 的元素是非直接的, 有可能 slice 可以包含它自身. 虽然有办法处理这种特殊情况, 但是没有一种方法是简单, 高效, 直观的.  
> 其次, 因为 slice 的元素不是直接的, 所以如果底层数组元素改变, 同一个 slice 在不同的时间会拥有不同的元素. 由于散列表 (例如 Go 的 map 类型) 仅对元素的键做浅拷贝, 这就要求散列表里面键在整个生命周期内必须保持不变. 因为 slice 需要深度比较, 所以不能用 slice 作为 map 的键. 对于引用类型, 如指针和通道, 操作符 == 检查的是引用相等性, 即它们是否指向了相同元素. 如果有一个相似的 slice 相等性比较功能, 它或许会比较有用, 也能解决 slice 作为 map 键的问题, 但是如果操作符 == 对 slice 和数组的行为不一致, 会带来困扰. 所以最安全的方式就是不允许直接比较 slice.

- slice 唯一允许的比较操作是和 nil 做比较.
- slice 类型的零值是 nil. 值为 nil 的 slice 没有对应的底层数组.
- 值为 nil 的 slice 长度和容量都是零, 但是也有非 nil 的 slice 长度和容量是零, 例如[]int{} 或 make[[]int,3](3:)
- 对于任何类型, 如果它们的值可以是 nil, 那么这个类型的 nil 值可以使用一种转换表达式, 例如 []int(nil).
- 如果要检查一个 slice 是否为空, 应使用 len(s) == 0, 而不是 s== nil, 因为 s != nil 的情况下, slice 也可能为空.
- 内置函数 make 可以创建一个具有指定元素类型, 长度和容量的 slice. 其中容量参数可以省略, 这种情况下, slice 的长度和容量相等.

```go
make([]T, len)
make([]T, len, cap) // same as make([]T, cap)[:len]
```

> make 创建了一个无名数组并返回给了一个 slice. 这个数组仅可以通过这个 slice 来访问.

##### 4.2.1 append 函数

_内置函数 append 用来将元素追加到 slice 的后面._

```go
    var runes []rune
    for _, r := range "Hello, BF " {
        runes = append(runes, r)
    }
    fmt.Printf("%q\n", runes) // "['H' 'e' 'l' 'l' 'o' ',' ' ' ' B ' ' F ']"
```

- append 函数对理解 slice 的工作原理很重要.

```go
func appendInt(x []int, y int) []int {
    var z []int
    zlen := len(x) + 1
    if zlen <= cap(x) {
        // There is room to grow. Extend the slice.
        z = x[:zlen]
    } else {
        // There is insufficient space. Allocate a new array.
        // Grow by doubling, for amortized linear complexity.
        zcap := zlen
        if zcap < 2*len(x) {
            zcap = 2 * len(x)
        }
        z = make([]int, zlen, zcap)
        copy(z, x) // a built-in function; see text
    }
    z[len(x)] = y
    return z
}
```

> 每一次 appendInt 调用都必须检查 slice 是否仍有足够容量来存储数组中的新元素. 如果 slice 容量足够, 那么它会定义一个新的 slice (仍引用原始底层数组). 然后将新元素 y 复制到新的位置, 并返回这个新的 slice. 输入参数 slice x 和函数返回值 slice z 拥有相同的底层数组.  
> 如果 slice 的容量不够容纳增长元素, appendInt 函数必须创建一个拥有足够容量的新的底层数组来存储新的元素, 然后将元素从 slice x 复制到这个数组, 再将新元素 y 追加到数组后面. 返回值 slice z 将和输入参数 slice x 引用不同的底层函数.  
> copy 函数用来为两个拥有相同元素类型的 slice 复制元素. 第一个参数是目标 slice, 第二个参数是源 slice, copy 函数将源 slice 中的元素复制到目标 slice 中. 不同的 slice 可能对应相同的底层数组, 甚至可能存在元素重叠. copy 函数有返回值, 它返回实际上复制的元素个数, 这个值是两个 slice 长度的较小值.  
> 每次扩容时, 通过扩展一倍的容量来减少内存分配次数, 这样也可以保证追加一个元素所消耗的是固定时间.  
> 内置的 append 函数使用比这里 appendInt 更复杂的增长策略.

- 通常情况下我们不清楚哪一次 append 调用会导致新的一次内存分配, 所以我们不能假设原始 slice 和调用 append 后的 slice 指向同一个底层数组, 也无法保证它们指向的是不同的底层数组. 同样, 我们也无法假设旧 slice 上对元素的操作会或者不会影响新的 slice 元素. 所以, 通常我们将 append 的调用结果再次赋值给传入 append 函数的 slice.

```go
runes = append(runes, r)
```

- 不仅仅是在调用 append 函数的情况下需要更新 slice 变量. 另外, 对于任何函数, 只要有可能改变 slice 的长度或者容量, 或者是使用 slice 指向不同的底层数组, 都需要更新 slice 变量. 虽然底层数组的元素是间接引用的, 但是 slice 的指针, 长度和容量不是. 要更新一个 slice 的指针, 长度或容量必须使用如上所示的赋值. 从这个角度来看, slice 并不是纯引用类型,而更像下面这种聚合类型

```go
type IntSlice struct {
ptr *int
len, cap int
}
```

- 内置的 append 函数可以同时给 slice 添加多个元素, 甚至添加另一个 slice 里的所有元素.

```go
var x []int
x = append(x, 1)
x = append(x, 2, 3)
x = append(x, 4, 5, 6)
x = append(x, x...) // append the slice x
fmt.Println(x) // "[1 2 3 4 5 6 1 2 3 4 5 6]"
```

- 函数声明中的 "..." 表示该函数可以接受**可变长度参数列表**.

##### 4.2.2 slice 就地修改

_下面的 nonempty 可以从给定的字符串列表中除去空字符串并返回一个新的 slice._

```go
// Nonempty is an example of an in-place slice algorithm.
package main

import "fmt"

func main() {
    data := []string{"one", "", "three"}
    fmt.Printf("%q\n", nonempty(data))
    fmt.Printf("%q\n", data)
}
// nonempty returns a slice holding only the non-empty strings.
// The underlying array is modified during the call.
func nonempty(strings []string) []string {
    i := 0
    for _, s := range strings {
        if s != "" {
            strings[i] = s
            i++
        }
    }
    return strings[:i]
}
```

_输入的 slice 和输出的 slice 拥有相同的底层数组, 避免在函数内部重新分配一个新的数组, 这种情况下, 底层数组元素只有部分被修改. 因此我们通常写成 data = nonempty(data). 还可以通过 append 函数来写:_

```go
func nonempty2(strings []string) []string {
    out := strings[:0] // zero-length slice of original
    for _, s := range strings {
        if s != "" {
            out = append(out, s)
        }
    }
    return out
}
```

- 重用底层数组的结果是每一个输入值的 slice 最多只有一个输出的结果 slice.
- slice 实现栈

```go
stack = append(stack, v) // push v
top := stack[len(stack)-1] // top of stack
stack = stack[:len(stack)-1] // pop
```

- 从 slice 中间删除一个元素, 并保留剩余元素的顺序, 可以使用函数 copy 来将高位索引的元素向前移动来覆盖被移除元素所在的位置:

```go
func remove(slice []int, i int) []int {
    copy(slice[i:], slice[i+1:])
    return slice[:len(slice)-1]
}

func main() {
s := []int{5, 6, 7, 8, 9}
fmt.Println(remove(s, 2)) // "[5 6 8 9]"
}
```

_如果不需要维持 slice 中剩余元素的顺序, 可以简单地将 slice 的最后一个元素赋值给被移除元素所在的索引位置:_

```go
func remove(slice []int, i int) []int {
slice[i] = slice[len(slice)-1]
return slice[:len(slice)-1]
}
func main() {
s := []int{5, 6, 7, 8, 9}
fmt.Println(remove(s, 2)) // "[5 6 9 8]
}
```

#### 4.3 map

_散列表是一个拥有键值对元素的无序集合. 在集合中, 键是唯一的, 键对应的值可以通过键来获取, 更新或移除. 无论这个散列表有多大, 这些操作基本是通过常量时间的键比较就可以完成._

- 在 Go 语言中, map 是散列表的引用, map 的类型是 map[K]V, 其中 k 和 v 是字典的键和值对应的数据类型, 但是键的类型和值的类型不一定相同. 键的类型 k , 必须是可以通过 == 来进行比较的数据类型, 所以 map 可以检测某一个键是否已经存在. 虽然浮点型是可以比较的, 但是比较浮点型的相等性不是一个好主意.
- 内置函数 make 可以用来创建一个 map:

```go
ages := make(map[string]int) // mapping from strings to ints
```

- 也可以使用 map 的字面量来新建一个带初始化键值对元素的字典:

```go
ages := map[string]int{
"alice": 31,
"charlie": 34,
}
```

等价于

```go
ages := make(map[string]int)
ages["alice"] = 31
ages["charlie"] = 34
```

_因此, 新的空 map 的另一种表达式是 : map[string]int{}_  
_map 元素访问也可以通过下标的方式:_

```go
ages["alice"] = 32
fmt.Println(ages["alice"]) // "32"
```

- 可以使用内置函数 delete 来从字典中根据键移除一个元素:

```go
delete(ages, "alice") // remove element ages["alice"]
```

_即使键不在 map 中, 上面的操作也是安全的. map 使用给定的键来查找元素, 如果对应的元素不存在, 就返回值类型的零值._

- 快捷复制方式 (如 x+=y he x++) 对 map 中的元素同样适用

```go
ages["bob"] += 1
//or
ages["bob"]++
```

- map 不是一个变量, 不可以获取它的地址, 比如这样是不对的:

```go
_ = &ages["bob"] // compile error: cannot take address of map element
```

我们无法获取 map 元素的地址的一个原因是 map 的增长可能导致已有元素被重新散列到新的存储位置, 这样就可能导致获取的地址无效

- 可以使用 for 循环 (结合 range 关键字) 来遍历 map 中所有的键和值.

```go
for name, age := range ages {
fmt.Printf("%s\t%d\n", name, age)
}
```

- map 中元素的迭代顺序是不固定的, 不同实现方法会使用不同的散列算法, 得到不同的元素顺序. 如果需要按照某种顺序来遍历 map 中的元素, 我们必须显示地进行排序

```go
import "sort"
var names []string
for name := range ages {
names = append(names, name)
}
sort.Strings(names)
for _, name := range names {
fmt.Printf("%s\t%d\n", name, ages[name])
}
```

- map 类型的零值是 nil, 也就是说, 没有引用任何散列表.

```go
var ages map[string]int
fmt.Println(ages == nil) // "true"
fmt.Println(len(ages) == 0) // "true"
```

- 大多数 map 操作都可以安全在 map 的零值 nil 上执行, 包括查找元素, 删除元素, 获取 map 元素个数 (len), 执行 range 循环, 因为这和空 map 的行为一致. 但是向零值 map 中设置元素将会导致错误:

```go
ages["carol"] = 21 // panic: assignment to entry in nil map
```

- 设置元素之前, 必须初始化 map
- 通过下标访问 map 中的元素总是有值.

> 如果键在 map 中, 你将获得对应的值;  
> 如果键不在 map 中, 将获得 map 值类型的零值. 如果需要知道一个元素是否在 map 中. 可以这样做:

```go
age, ok := ages["bob"]
if !ok { /* "bob" is not a key in this map; age == 0. */ }
```

_通常这两条语句合并成一条语句, 如下所示:_

```go
if age, ok := ages["bob"]; !ok { /* ... */ }
```

通过这种下标形式访问 map 中的元素输出两个值, 第二个值是一个布尔值, 用来报告该元素是否存在. 这个布尔值一般叫做 ok

> map 不可比较, 唯一合法的比较就是和 nil 做比较.
> _判断两个 map 是否拥有相同的键和值, 必须写一个循环:_

```go
func equal(x, y map[string]int) bool {
if len(x) != len(y) {
return false
}
for k, xv := range x {
        if yv, ok := y[k]; !ok || yv != xv {
            return false
        }
    }
return true
}
```

- Go 没有集合类型, 但是既然 map 的键都是唯一的, 就可以通过 map 来实现这个功能

```go
func main() {
    seen := make(map[string]bool) //a set of strings
    input := bufio.NewScanner(os.Stdin)
    for input.Scan() {
        line := input.Text()
        if !seen[line] {
            seen[line] = true
            fmt.Println(line)
        }
    }
    if err := input.Err(); err != nil {
        fmt.Fprintf(os.Stderr, "dedup:%v\n", err)
        os.Exit(1)
    }
}
```

_有时候, 我们需要一个 map 并且需求它的键是 slice, 但是因为 map 的键必须是可以比较的. 通常可以分为两个来做. 首先, 定义一个帮助函数 k 将每一个键都映射到字符串, 当且仅当 x 和 y 相等的时候, 我们才认为 k(x) == k(y). 然后, 就可以创建一个 map , map 的键是字符串类型, 在每个键元素被访问时, 调用这个帮助函数._

```go
var m = make(map[string]int)
func k(list []string) string { return fmt.Sprintf("%q", list) }
func Add(list []string) { m[k(list)]++ }
func Count(list []string) int { return m[k(list)] }
```

_同样的方法适用于任何不可直接比较的键类型. 或有时候, 不不想让键通过 == 来比较相等性, 而是自定义一种比较方法.下面的例子, 用于统计输入中 Unicode 代码点出现次数的程序._

```go
// Charcount computes counts of Unicode characters.
package main
import (
    "bufio"
    "fmt"
    "io"
    "os"
    "unicode"
    "unicode/utf8"
)
func main() {
    counts := make(map[rune]int) // counts of Unicode characters
    var utflen [utf8.UTFMax + 1]int // count of lengths of UTF-8 encodings
    invalid := 0 // count of invalid UTF-8 characters
    in := bufio.NewReader(os.Stdin)
    for {
        r, n, err := in.ReadRune() // returns rune, nbytes, error
        if err == io.EOF {
            break
        }
        if err != nil {
            fmt.Fprintf(os.Stderr, "charcount: %v\n", err)
            os.Exit(1)
        }
        if r == unicode.ReplacementChar && n == 1 {
            invalid++
            continue
        }
        counts[r]++
        utflen[n]++
    }
    fmt.Printf("rune\tcount\n")
    for c, n := range counts {
        fmt.Printf("%q\t%d\n", c, n)
    }
    fmt.Print("\nlen\tcount\n")
    for i, n := range utflen {
        if i > 0 {
            fmt.Printf("%d\t%d\n", i, n)
        } }
    if invalid > 0 {
        fmt.Printf("\n%d invalid UTF-8 characters\n", invalid)
    }
}

```

_函数 ReadRune 解码 UTF-8 编码, 并返回三个值: 解码的字符, UTF-8 编码中字节的长度和错误值. 这里唯一可能出现的错误时文件结束 (EOF). 如果输入的不是合法的 UTF-8 字符, 那么返回的字符是 code.ReplacementChar 并且长度是 1._

- map 的值类型本身可以是复合数据类型. 下面, graph 建立了一个从字符串到字符串集合的映射.

```go
var graph = make(map[string]map[string]bool)
func addEdge(from, to string) {
edges := graph[from]
if edges == nil {
edges = make(map[string]bool)
graph[from] = edges
}
edges[to] = true
}
func hasEdge(from, to string) bool {
return graph[from][to]
}
```

_函数 addEdge 演示了一种符合语言习惯的延迟初始化 map 的方法, 当 map 中的每一个键第一次出现的时候初始化. 函数 hasEdge 演示了在 map 中值不存在的情况下, 也可以直接使用, 即使 from 和 to 都不存在, graph[from][to] 也始终可以给出一个有意义的值._

---

#### 4.4 结构体

_结构体是将零个或者多个任意类型的命名变量组合在一起的聚何数据类型. 每个变量都叫做结构体成员._
_下面的语句定义了一个叫 Employee 的结构体和一个结构体变量 dilbert:_

```go
type Employee struct {
ID int
Name string
Address string
DoB time.Time
Position string
Salary int
ManagerID int
}
var dilbert Employee
```

_dilbert 的每一个成员都是通过点号方式来访问, 就像 dilbert.Name 和 dilbert.DoB. 由于 dilbert 是一个变量, 它的所有成员都是变量, 因此可以给结构体的成员赋值:_

```go
dilbert.Salary -= 5000 // demoted, for writing too few lines of code

```

_或者获取成员变量的地址, 然后通过指针来访问它:_

```go
position := &dilbert.Position
*position = "Senior " + *position // promoted, for outsourcing to Elbonia
```

_点号同样可以用在结构体指针上:_

```go
var employeeOfTheMonth *Employee = &dilbert
employeeOfTheMonth.Position += " (proactive team player)"
```

_后面一条句子等价于:_

```go
(*employeeOfTheMonth).Position += " (proactive team player)"
```

_函数 EmployeeID 通过给定的参数 ID 来返回一个指向 Employee 结构体的指针. 可以用点号来访问它的成员变量:_

```go
func EmployeeByID(id int) *Employee { /* ... */ }
fmt.Println(EmployeeByID(dilbert.ManagerID).Position) // "Pointy-haired boss"
id := dilbert.ID
EmployeeByID(id).Salary = 0 // fired for... no real reason
```

_最后一条语句更新了函数 EmployeeID 返回的指针指向的结构体 Employee. 如果函数 EmployeeID 的返回值类型变成了 Employee 而不是 \*Employee, 那么代码将无法通过编译, 因为赋值表达式的左侧无法识别出一个变量._
_结构体的成员变量通常一行写一个, 变量的名称在类型前面, 但是相同类型的连续成员变量可以写在一行上, 如这里的 Name 和 Address:_

```go
type Employee struct {
ID int
Name, Address string
DoB time.Time
Position string
Salary int
ManagerID int
}
```

- 成员变量的顺序不同, 那么就是不是同一个结构体.
- 如果一个结构体的成员变量名称首字母是大写的, 那么这个变量是可导出的. 一个结构体可以同时包含可导出和不可导出的成员变量.
- 因为结构体类型一个成员变量占据一行,所以通常它的定义一般比较长. 虽然可以每次需要它时写出整个结构体定义, 但是通常我们会定义命名结构体类型.
- 命名结构体类型 S 不能定义一个拥有相同结构体类型 S 的成员变量, 也就是说一个聚合类型不可以包含它自己本身 (同样的限制对数组也适用). 但是 S 中可以定义一个 S 的指针类型, 即 \*S, 这样我们就可以创建一些递归数据结构, 比如链表和树. 下面的代码给出了一个利用二叉树来实现插入排序的例子.

```go

type tree struct {
    value       int
    left, right *tree
}

// Sort sorts values in place.
func Sort(values []int) {
    var root *tree
    for _, v := range values {
        root = add(root, v)
    }
    appendValues(values[:0], root)
}

// appendValues appends the elements of t to values in order
// and returns the resulting slice.
func appendValues(values []int, t *tree) []int {
    if t != nil {
        values = appendValues(values, t.left)
        values = append(values, t.value)
        values = appendValues(values, t.right)
    }
    return values
}
func add(t *tree, value int) *tree {
    if t == nil {
        // Equivalent to return &tree{value: value}.
        t = new(tree)
        t.value = value
        return t
    }
    if value < t.value {
        t.left = add(t.left, value)
    } else {
        t.right = add(t.right, value)
    }
    return t
}

```

- 结构体的零值由结构体成员的零值组成.
- 没有任何成员变量的结构体称为空结构体, 写作: strut{}. 它没有长度, 也不携带任何信息.

##### 4.4.1 结构体字面量

- 结构体类型的值可以通过结构体字面量来设置, 即通过设置结构体的成员变量来设置.

```go
type Point struct{ X, Y int }
p := Point{1, 2}
```

_如上形式, 要求按照正确顺序, 为每个成员变量指定一个值.一般使用通过指定部分或者全部成员变量的名称和值来初始化结构体变量:_

```go
anim := gif.GIF{LoopCount: nframes}
```

_如果在这种初始化方式中某个成员变量没有指定, 那么它们的值就是该成员变量类型值, 因为指定了名字, 所以它们的顺序无所谓_
_两种初始化方式不能混用. 也不能通过第一种初始化方式来绕过不可导出变量无法在其他报中使用的规则._

```go
package p
type T struct{ a, b int } // a and b are not exported
package q
import "p"
var _ = p.T{a: 1, b: 2} // compile error: can't reference a, b
var _ = p.T{1, 2} // compile error: can't reference a, b
```

_虽然上面的最后一行代码没有显示地提到不可导出变量, 但是它们被隐式地引用了, 所以也是不允许地._

- 结构体类型地值可以作为参数传递给函数或者作为函数地返回值.

```go
func Scale(p Point, factor int) Point {
return Point{p.X * factor, p.Y * factor}
}
fmt.Println(Scale(Point{1, 2}, 5)) // "{5 10}"
```

- 处于效率考虑, 大型结构体通常使用结构体指针地方式直接传递给函数或者从函数中返回.

```go
func Bonus(e *Employee, percent int) int {
return e.Salary * percent / 100
}
```

- 由于通常结构体都是通过指针地方式使用, 因为可以使用一种简单地方式来创建, 初始化一个 struct 类型地变量并获取它地地址:

```go
pp := &Point{1, 2}
```

_等价于:_

```go
pp := new(Point)
*pp = Point{1, 2}
```

&Point{1, 2} 可以直接用在一个表达式中, 例如函数调用

##### 4.4.3 结构体比较

- 如果一个结构体地所有成员变量是可以比较地, 那么这个结构体就是可比较地.
  > 两个结构体地比较可以使用 == 或者 !=. 其中 == 操作符按照顺序比较两个结构体变量地成员变量, 所以下面地两个输出语句是等价地:

```go
type Point struct{ X, Y int }
p := Point{1, 2}
q := Point{2, 1}
fmt.Println(p.X == q.X && p.Y == q.Y) // "false"
fmt.Println(p == q) // "false"
```

_和其他可比较地类型一样, 可比较地结构体类型都可以作为 map 地键类型._

```go
type address struct {
hostname string
port int
}
hits := make(map[address]int)
hits[address{"golang.org", 443}]++
```

##### 4.4.3 结构体嵌套和匿名成员

_结构体嵌套机制, 这个机制可以让我们将一个命名结构体当作另一个结构体类型地匿名成员使用; 使用简单地表达式 (比如 x.f) 就可以代表连续地成员 (如 x.d.e.f)._

```go
type Circle struct {
X, Y, Radius int
}

type Wheel struct {
X, Y, Radius, Spokes int
}
```

_Circle 类型定义了圆心地坐标 X 和 Y , 另外还有一个半径 Radius. Wheel 类型拥有 Circle 类型的所有属性, 另外还有一个 Spokes 属性, 即车轮中条幅的数量. 创建一个 wheel 类型的对象:_

```go
var w Wheel
w.X = 8
w.Y = 8
w.Radius = 5
w.Spokes = 20
```

重构相同部分

```go
type Point struct {
X, Y int
}
type Circle struct {
Center Point
Radius int
}
type Wheel struct {
Circle Circle
Spokes int
}
```

_访问 wheel 成员变量变得复杂:_

```go
var w Wheel
w.Circle.Center.X = 8
w.Circle.Center.Y = 8
w.Circle.Radius = 5
w.Spokes = 20
```

_Go 允许我们定义不带名称的结构体成员, 只需要指定类型即可; 这种结构体成员称做匿名成员. 这个结构体成员的类型必须是一个命名类型或者指向命名类型的指针._

```go
type Circle struct {
Point
Radius int
}
type Wheel struct {
Circle
Spokes int
}
```

_我们能直接访问我们需要的变量而不指定一大串中间变量:_

```go
var w Wheel
w.X = 8 // equivalent to w.Circle.Point.X = 8
w.Y = 8 // equivalent to w.Circle.Point.Y = 8
w.Radius = 5 // equivalent to w.Circle.Radius = 5
w.Spokes = 20
```

_上面注释里面的方式也是正确的, 但是使用 "匿名成员" 的说法或许不合适. 上面结构体成员的名字, 就是对应类型的名字, 只是这些名字在点号访问变量时是可选的. 当我们访问最终需要的变量是可以省略中间所有的匿名成员._
_结构体字面量并没有什么快捷方式来初始化结构体, 所以下面的语句是无法通过编译的:_

```go
w = Wheel{8, 8, 5, 20} // compile error: unknown fields
w = Wheel{X: 8, Y: 8, Radius: 5, Spokes: 20} // compile error: unknown fields
```

_结构体字面量必须遵循形状类型的定义, 所以一下两种方式是等价的:_

```go
w = Wheel{Circle{Point{8, 8}, 5}, 20}
w = Wheel{
Circle: Circle{
Point: Point{X: 8, Y: 8},
Radius: 5,
},
Spokes: 20, // NOTE: trailing comma necessary here (and at Radius)
}
fmt.Printf("%#v\n", w)
// Output:
// Wheel{Circle:Circle{Point:Point{X:8, Y:8}, Radius:5}, Spokes:20}
w.X = 42
fmt.Printf("%#v\n", w)
// Output:
// Wheel{Circle:Circle{Point:Point{X:42, Y:8}, Radius:5}, Spokes:20}
```

- 匿名成员 拥有隐式的名字, 所以不能在结构体里面定义两个相同类型的匿名成员, 否则会引起冲突. 因此, 它们的可导性也是由它们的类型决定的.
- 匿名成员不一定是结构体, 任何命名类型或者指向命名类型的指针都可以.
- 以快捷方式访问匿名成员的内部变量同样适用于访问匿名成员的内部方法.
- 在 Go 中, 组合是面向对象编程方式的核心.

#### 4.5 JSON

- JSON 是 JavaScript 值的 Unicode 编码, 这些值包括字符串, 数字, 布尔值, 数组和对象.
- Go 的数据结构转换为 JSON 称为 marshal. marshal 是通过 json.Marshal 来实现的:

```go
type Movie struct {
Title string
Year int `json:"released"`
Color bool `json:"color,omitempty"`
Actors []string
}
var movies = []Movie{
{Title: "Casablanca", Year: 1942, Color: false,
Actors: []string{"Humphrey Bogart", "Ingrid Bergman"}},
{Title: "Cool Hand Luke", Year: 1967, Color: true,
Actors: []string{"Paul Newman"}},
{Title: "Bullitt", Year: 1968, Color: true,
Actors: []string{"Steve McQueen", "Jacqueline Bisset"}},
// ...
}

data, err := json.MarshalIndent(movies, "", " ")
if err != nil {
log.Fatalf("JSON marshaling failed: %s", err)
}
fmt.Printf("%s\n", data)
```

> Marshal 生成了一个字节 slice, 其中包含一个不带任何多余空白字符串的很长的字符串.

```go
[{"Title":"Casablanca","released":1942,"Actors":["Humphrey Bogart","Ingr
id Bergman"]},{"Title":"Cool Hand Luke","released":1967,"color":true,"Ac
tors":["Paul Newman"]},{"Title":"Bullitt","released":1968,"color":true,"
Actors":["Steve McQueen","Jacqueline Bisset"]}]
```

- 通过 json.MarshalIndent 可以输出整齐格式化过的结果. 整个函数有两个参数, 一个是定义每行输出的前缀字符串, 另外一个是定义缩进的字符串.

```go
data, err := json.MarshalIndent(movies, "", " ")
if err != nil {
log.Fatalf("JSON marshaling failed: %s", err)
}
fmt.Printf("%s\n", data)
```

> 上面代码的输出结果:

```go
 [
 {
"Title": "Casablanca",
"released": 1942,
"Actors": [
"Humphrey Bogart",
"Ingrid Bergman"
]
},
{
"Title": "Cool Hand Luke",
"released": 1967,
"color": true,
"Actors": [
"Paul Newman"
]
},
{
"Title": "Bullitt",
"released": 1968,
"color": true,
"Actors": [
"Steve McQueen",
"Jacqueline Bisset"
] } ]
```

marshal 使用 Go 结构体成员名称作为 JSON 对象里面字段的名称 (通过反射方式). 只有可导出的成员可以转换为 JSON 字段. 可以通过成员标签定义 (field tag) 自定义 JSON 字段名称

```go
Year int `json:"released"`
Color bool `json:"color,omitempty"`
```

> 成员标签定义可以是任意字符串, 但是按照习惯, 是由一串由空格分开的标签键值对 key:"value" 组成; 因为标签的值使用双引号括起来, 所以一般标签都是原生字符串字面量. Color 的标签还有一个额外的选项, omitempty, 它表示如果这个成员变量的值是零值或者为空, 这不输出这个成员到 JSON 中.

- marshal 的逆操作将 JSON 字符串解码为 Go 数据结构, 这个过程叫 unmarshal, 这个是由 json.Unmarshal 实现的.

```go
var titles []struct{ Title string }
if err := json.Unmarshal(data, &titles); err != nil {
log.Fatalf("JSON unmarshaling failed: %s", err)
}
fmt.Println(titles) // "[{Casablanca} {Cool Hand Luke} {Bullitt}]"
```

> 通过查询 Github 提供的 issue 跟踪接口来演示, 首先定义结构体如下:

```go
// Package github provides a Go API for the GitHub issue tracker.
// See https://developer.github.com/v3/search/#search-issues.
package github
import "time"
const IssuesURL = "https://api.github.com/search/issues"
type IssuesSearchResult struct {
TotalCount int `json:"total_count"`
Items []*Issue
}
type Issue struct {
Number int
HTMLURL string `json:"html_url"`
Title string
State string
User *User
CreatedAt time.Time `json:"created_at"`
Body string // in Markdown format
}
type User struct {
Login string
HTMLURL string `json:"html_url"`
}
```

_即使对应的 JSON 字段的名称不是首字母大写, 结构体的成员名称也必须首字母大写. 由于在 unmarshal 阶段, JSON 字段的名称关联到 Go 结构体成员的名称是忽略大小写的, 因此这里只需要在 JSON 中有下划线而 Go 里面没有下划线的使用一下成员标签定义. 函数 SearchIssues 发送 HTTP 请求将回复解析为 JSON._

```go
package github
import (
"encoding/json"
"fmt"
"net/http"
"net/url"
"strings"
)
// SearchIssues queries the GitHub issue tracker.
func SearchIssues(terms []string) (*IssuesSearchResult, error) {
q := url.QueryEscape(strings.Join(terms, " "))
resp, err := http.Get(IssuesURL + "&#63;q=" + q)
if err != nil {
return nil, err
}
// We must close resp.Body on all execution paths.
// (Chapter 5 presents 'defer', which makes this simpler.)
if resp.StatusCode != http.StatusOK {
resp.Body.Close()
return nil, fmt.Errorf("search query failed: %s", resp.Status)
}
var result IssuesSearchResult
if err := json.NewDecoder(resp.Body).Decode(&result); err != nil {
resp.Body.Close()
return nil, err
}
resp.Body.Close()
return &result, nil
}
```

- 使用流式解码器 (即 json.Decoder), 可以利用它来依次从字节流里解码出多个 JSON 实体.

```go
// Issues prints a table of GitHub issues matching the search terms.
package main
import (
"fmt"
"log"
"os"
"gopl.io/ch4/github"
)
func main() {
result, err := github.SearchIssues(os.Args[1:])
if err != nil {
log.Fatal(err)
}
fmt.Printf("%d issues:\n", result.TotalCount)
for _, item := range result.Items {
fmt.Printf("#%-5d %9.9s %.55s\n",
item.Number, item.User.Login, item.Title)
}
}
```

#### 文本和 HTML 模板

_通过 text/template 包和 html/template 包, 将程序标量值代入到文本或者 HTML 模板中._
_模板是一个字符串或者文件, 它包含一个或者多个两边用双大括号包围的单元 (形如: {{...}}), 这称为操作. 大多数字符串是直接输出的, 但是操作可以引发其他行为. 每个操作在模板语言里都对应一个表达式, 提供的简单但强大的功能包括: 输出值, 选择结构体成员, 调用函数和方法, 描述控制逻辑 (比如 if-else 语句和 range 循环), 实例化其他模板等._

```go
const templ = `{{.TotalCount}} issues:
{{range .Items}}----------------------------------------
Number: {{.Number}}
User: {{.User.Login}}
Title: {{.Title | printf "%.64s"}}
Age: {{.CreatedAt | daysAgo}} days
{{end}}`

func daysAgo(t time.Time) int {
return int(time.Since(t).Hours() / 24)
}
```

> 在操作里用点号 (.) 表示当前值, 在这个例子中即是 github.IssuesSearchResult. 操作 {{.TotalCount}} 代表 TotalCount 成员的值, 直接输出. {{range.Items}} 和 {{end}} 操作创建一个循环, 这个时候点号 (.) 表示 Items 里面连续的元素.
> _在操作中, 符号 | 会将前一个操作的结果当做下一个操作的输入, 和 UNIX 的 shell 管道类似._

- 通过模板输出结果需要两个步骤:
  > 首先, 需要解析模板并转换为内部的表示方法, 然后在指定的输入上面执行. 解析模板只需要执行一次. 下面的代码创建并解析了上面定义的文本模板 templ. 注意方法的链式调用: template.New 创建并返回一个新的模板, Funcs 添加 daysAgo 到模板内部可以访问的函数列表中, 然后返回这个模板对象; 最后调用 Parse 方法.

```go
report, err := template.New("report").
Funcs(template.FuncMap{"daysAgo": daysAgo}).
Parse(templ)
if err != nil {
log.Fatal(err)
}
```

> 由于模板通常在编译期间就固定下来, 因此无法解析模板将是程序中的一个严重 bug. 帮助函数 template.Must 提供了一种便捷的错误处理方式, 它接受一个模板和错误作为参数, 检查错误是否为 nil (如果不是 nil, 则宕机), 然后返回这模板.  
> 一旦创建了模板, 添加了内部可以调用的函数 daysAgo, 然后解析, 再检查, 就可以使用 github.IssuesSearchResult 作为数据源, 使用 os.Stdout 作为输出目标执行这个模板:

```go
var report = template.Must(template.New("issuelist").
Funcs(template.FuncMap{"daysAgo": daysAgo}).
Parse(templ))
func main() {
result, err := github.SearchIssues(os.Args[1:])
if err != nil {
log.Fatal(err)
}
if err := report.Execute(os.Stdout, result); err != nil {
log.Fatal(err)
}
}
```

_程序输出如下:_

```go
 go build gopl.io/ch4/issuesreport
 ./issuesreport repo:golang/go is:open json decoder
13 issues:
----------------------------------------
Number: 5680
User: eaigner
Title: encoding/json: set key converter on en/decoder
Age: 750 days
----------------------------------------
Number: 6050
User: gopherbot
Title: encoding/json: provide tokenizer
Age: 695 days
----------------------------------------
...
```

- html/template 包.
  > 使用和 text/template 包里面一样的 API 和表达式语句, 并且额外地出现在 HTML, JavaScript, CSS 和 URL 中地字符串进行自动转义. 避免生成地 HTML 引发安全问题, 比如 **注入攻击**.  
  > _我们可以通过使用命名地字符串类型 template.HTML 类型而不是字符串类型避免模板自动转义受信任地 HTML 数据. 同样地命名类型适用于受信任地 JavaScript, CSS 和 URL._

```go
func main() {
const templ = `<p>A: {{.A}}</p><p>B: {{.B}}</p>`
t := template.Must(template.New("escape").Parse(templ))
var data struct {
A string // untrusted plain text
B template.HTML // trusted HTML
}
data.A = "<b>Hello!</b>"
data.B = "<b>Hello!</b>"
if err := t.Execute(os.Stdout, data); err != nil {
log.Fatal(err)
}
}
```

---

### 第 5 章 函数

_函数包含连续的执行语句, 可以在代码中通过调用函数来执行它们._
_函数将一个复杂的工作切分成多个更小模块, 使多人协作变得更加容易._
_函数对它的使用者隐藏了实现细节._

#### 5.1 函数声明

- 每个函数声明都包含一个名字, 一个形参列表, 一个可选的返回列表以及函数体:

```go
func name ( parameter-list ) ( result-list ) {
body
}
```

> 形参列表指定了一组变量的参数名和参数类型, 这些局部变量都由调用者提供的实参传递而来. 返回列表指定了函数返回值的类型. 当函数返回一个未命名的返回值或者没有返回值的时候, 返回列表的圆括号可以省略. 如果一个函数既省略返回列表也没有返回值, 那么设计这个函数的目的是调用函数之后所带来的附加效果.

```go
func hypot(x, y float64) float64 {
return math.Sqrt(x*x + y*y)
}
fmt.Println(hypot(3, 4)) // "5"
```

> x 和 y 是函数声明中的形参, 3 和 4 是调用函数时的实参, 并且返回一个类型为 float64 的值.

- 返回值也可以像形参一样命名.
  > 这个时候, 每一个命名的返回值会声明为一个局部变量, 并根据变量类型初始化为相应的 0 值.
- 当函数存在返回列表时, 必须显式地以 return 语句结束.
- 如果几个形参或者返回值地类型相同, 那么类型只需要写一次.

```go
func f(i, j, k int, s, t string) { /* ... */ }
func f(i int, j int, k int, s string, t string) { /* ... */ }
```

_下面使用 4 种方式声明一个带有两个形参和一个返回值地函数, 所有变量都是 int 类型. 空白标志符用来强调这个形参在函数中未使用._

```go
func add(x int, y int) int { return x + y }
func sub(x, y int) (z int) { z = x - y; return }
func first(x int, _ int) int { return x }
func zero(int, int) int { return 0 }
fmt.Printf("%T\n", add) // "func(int, int) int"
fmt.Printf("%T\n", sub) // "func(int, int) int"
fmt.Printf("%T\n", first) // "func(int, int) int"
fmt.Printf("%T\n", zero) // "func(int, int) int"
```

- 函数地类型称作**参数签名**.  
  _当两个函数拥有相同地形参列表和返回列表时, 认为这个两个函数地类型或者签名时相同地. 而形参和返回值地名字不会影响到函数类型, 采用简写同样也不会影响到函数类型._  
  _每一次调用函数都需要提供实参来对应函数地每一个形参, 包括参数地调用顺序也必须一致. Go 语言没有默认参数值地概念也不能指定实参名, 所以形参和返回值地命名不会对调用方有任何影响._  
  _形参变量都是函数地局部变量, 初始值由调用者提供地实参传递. 函数形参以及命名返回值同属于外层作用域地局部变量._  
  _实参是按值传递地, 所以函数接收地是每个实参地副本; 改变函数地形参变量并不会影响到调用者提供地实参. 然而, 如果提供地实参包含引用类型, 比如指针, slice, map, 函数或者通道, 那么当函数使用形参变量时就有可能会间接地修改实参变量_  
  _如果函数声明没有函数体, 那说明这个函数使用除了 Go 以外地语言实现. 这样地声明定义了该函数地签名._

```go
package math
func Sin(x float64) float64 // implemented in assembly language
```

#### 5.2 递归

_函数可以递归调用, 这意味着函数可以直接或者间接地调用自己._  
_递归地深度会受限于固定长度地栈大小, 当金星深度递归调用时必须谨防栈溢出. Go 语言地实现了可变长度地栈, 栈地大小会随着使用而增长, 可达到 1 GB 左右地上限._

#### 5.3 多返回值

- 一个函数能够返回不止一个结果.

```go
func main() {
for _, url := range os.Args[1:] {
links, err := findLinks(url)
if err != nil {
fmt.Fprintf(os.Stderr, "findlinks2: %v\n", err)
continue
}
for _, link := range links {
fmt.Println(link)
}
}
}
// findLinks performs an HTTP GET request for url, parses the
// response as HTML, and extracts and returns the links.
func findLinks(url string) ([]string, error) {
resp, err := http.Get(url)
if err != nil {
return nil, err
}
if resp.StatusCode != http.StatusOK {
resp.Body.Close()
return nil, fmt.Errorf("getting %s: %s", url, resp.Status)
}
doc, err := html.Parse(resp.Body)
resp.Body.Close()
if err != nil {
return nil, fmt.Errorf("parsing %s as HTML: %v", url, err)
}
return visit(nil, doc), nil
}
```

_Go 语言的垃圾回收机制将回收未使用的内存, 但是不能指望它会释放未使用的操作系统资源. 比如打开的文件以及网络连接. 必须显式的关闭它们._  
_返回一个多值结果可以时调用另一个多值返回的函数._

```go
func findLinksLog(url string) ([]string, error) {
log.Printf("findLinks %s", url)
return findLinks(url)
}
```

_一个多值调用可以作为单独的实参传递给拥有多个形参的函数中. 下面两个输出语句的效果时一致的._

```go
log.Println(findLinks(url))
links, err := findLinks(url)
log.Println(links, err)
```

_良好的名称可以使返回值更加有意义._

```go
func Size(rect image.Rectangle) (width, height int)
func Split(path string) (dir, file string)
func HourMinSec(t time.Time) (hour, minute, second int)
```

_但不必始终为每个返回值都单独命名._  
_一个函数如果有命名返回值, 可以省略 return 语句的操作数, 这称为裸返回._

```go
// CountWordsAndImages does an HTTP GET request for the HTML
// document url and returns the number of words and images in it.
func CountWordsAndImages(url string) (words, images int, err error) {
    resp, err := http.Get(url)
    if err != nil {
        return
    }
    doc, err := html.Parse(resp.Body)
    resp.Body.Close()
    if err != nil {
        err = fmt.Errorf("parsing HTML: %s", err)
        return
    }
    words, images = countWordsAndImages(doc)
    return
}
func countWordsAndImages(n *html.Node) (words, images int) { /* ... */ }
```

> 裸返回是将每个命名返回结果按照顺序返回的快捷方式, 所以在上面的函数中, 每个 return 语句等价于:

```go
return words, images, err
```

_错误处理是包的 API 设计者应用程序用户接口的只要一部分, 发生错误只是许多预料行为中的一种而已. 这就是 Go 语言处理错误的方法._  
_如果当函数调用发生错误时返回一个附加的结果作为错误值, 习惯上将错误值最为最后一个结果返回._  
_更多的时候, 尤其对于 I/O 操作, 错误原因可能多种多样, 而调用者则需要一些详细信息. 这种情况下, 错误的结果往往是 error._  
_error 是内置的接口类型._  
_一般当一个函数返回一个非空错误时, 它其他的结果都是未定义的而且应该被忽略. 然而, 有一些函数在调用出错的情况下返回部分有用的结果. 正确的行为通常是在调用者处理错误前先处理这些不完整的返回结果._  
_Go 语言通过使用普通的值而非异常来报告错误._
_Go 语言的异常只针对程序 bug 导致的预料之外的错误, 而不能作为常规的错误处理方法出现在程序中._  
_这样做的原因是异常会陷入带有错误消息的控制流去处理它, 通常会导致预期外的结果: 错误会以难以理解的栈跟踪信息报告给最终用户, 这些信息大多都是关于程序结构方面的而不是简单明了的错误信息._  
_相比之下, Go 程序使用通常的控制流机制 (比如 if 和 return 语句) 应对错误. 这种方式在错误处理逻辑方面要求更加小心谨慎, 当这恰恰是设计的要点._

##### 5.4.1 错误处理策略

_当一个函数调用返回一个错误时, 调用者应当负责检查错误并采取合适的处理应对._

- 策略一: 最常见的情形时将错误传递下去

```go
resp, err := http.Get(url)
if err != nil {
return nil, err
}
```

> 一般地, f(x) 调用只负责报告函数行为 f 和参数值 x, 因为它们和错误的上下文相关. 调用者负责添加进一步的信息, 但是 f(x) 本身并不会.

- 策略二: 对于不固定或者不可预测的错误, 在短暂间隔后对操作进行重试, 超出一定的重试次数和限定的时间后再报错退出.

```go
// WaitForServer attempts to contact the server of a URL.
// It tries for one minute using exponential back-off.
// It reports an error if all attempts fail.
func WaitForServer(url string) error {
const timeout = 1 * time.Minute
deadline := time.Now().Add(timeout)
for tries := 0; time.Now().Before(deadline); tries++ {
_, err := http.Head(url)
if err == nil {
return nil // success
}
log.Printf("server not responding (%s); retrying...", err)
time.Sleep(time.Second << uint(tries)) // exponential back-off
}
return fmt.Errorf("server %s failed to respond after %s", url, timeout)
}
```

- 策略三: 如果依旧不能顺利进行下去, 调用者能够输出错误然后优雅的停止程序, 但一般这样的处理应该留给主程序部分. 通常库函数应当将错误传递给调用者, 除非这个错误表示一个内部一致性错误, 这意味着库内部存在 bug.

-

```go
// (In function main.)
if err := WaitForServer(url); err != nil {
fmt.Fprintf(os.Stderr, "Site is down: %v\n", err)
os.Exit(1)
}
```

> 一个更加方便地方法是通过调用 log.Fatalf 实现同样地效果. 它会默认将时间和日期作为前缀添加到错误消息前.

```go
if err := WaitForServer(url); err != nil {
log.Fatalf("Site is down: %v\n", err)
}
```

> 可以自定义命令地名称作为 log 包地前缀, 并且将日期和时间略去.

```go
log.SetPrefix("wait: ")
log.SetFlags(0)
```

- 策略四: 在一些错误情况下, 只记录下错误信息后程序继续运行. 同样, 可以选择使用 log 包来增加日志地常用前缀:

```go
if err := Ping(); err != nil {
log.Printf("ping failed: %v; networking disabled", err)
}
```

_并且直接输出到标准错误流:_

```go
if err := Ping(); err != nil {
fmt.Fprintf(os.Stderr, "ping failed: %v; networking disabled\n", err)
}
```

(所有 log 函数都会为缺少换行符地日志补充一个换行符.)

- 策略五: 在某些罕见地情况下我们可以直接安全地忽略掉整个日志:

```go
dir, err := ioutil.TempDir("", "scratch")
if err != nil {
return fmt.Errorf("failed to create temp dir: %v", err)
}
// ...use temp dir...
os.RemoveAll(dir) // ignore errors; TMPDIR is cleaned periodically
```

> 调用 os.RemoveAll 可能会失败, 但是程序忽略了这个错误, 原因是操作系统会周期性地清理临时目录.  
> _Go 语言地错误处理有特定地规律. 进行错误检查之后, 检测到失败地情况往往都在成功之前. 如果检测到失败导致函数返回, 成功地逻辑一般不会放在 else 块中而是在外层地作用域中. 函数会有一种通常地形式, 就是在开头有一串地检查来返回错误, 之后跟着实际地函数体一直到最后._

##### 5.4.2 文件结束标志

_io 包保证任何由文件结束引起地读取错误, 始终都将会得到一个与众不同地错误: io.EOF, 它地定义如下:_

```go
package io
import "errors"
// EOF is the error returned by Read when no more input is available.
var EOF = errors.New("EOF")
```

_调用者可以使用一个简单地比较操作来检测这种情况:_

```go
in := bufio.NewReader(os.Stdin)
for {
r, _, err := in.ReadRune()
if err == io.EOF {
break // finished reading
}
if err != nil {
return fmt.Errorf("read failed: %v", err)
}
// ...use r...
}
```

_io.EOF 有一条固定地错误信息 "EOF"._

#### 5.5 函数变量

_函数在 Go 语言中是头等重要地值: 函数变量也有类型, 而且它们可以赋值给变量或者传递或者从其他函数中返回. 函数变量可以像其他函数一样调用._

```go
func square(n int) int { return n * n }
func negative(n int) int { return -n }
func product(m, n int) int { return m * n }
f := square
fmt.Println(f(3)) // "9"
f = negative
fmt.Println(f(3)) // "-3"
fmt.Printf("%T\n", f) // "func(int) int"
f = product // compile error: can't assign f(int, int) int to f(int) int
```

_函数类型地零值是 nil (空值), 调用一个空的函数变量将导致宕机._

```go
var f func(int) int
f(3) // panic: call of nil function
```

_函数变量可以和空值相比较:_

```go
var f func(int) int
if f != nil {
f(3)
}
```

_但它们本身不可比较, 所以不可以相互进行比较或者作为键值出现在 map 中._  
_函数变量使得函数不仅将数据进行参数化, 还将函数地行为当作参数进行传递._

```go
func add1(r rune) rune { return r + 1 }
fmt.Println(strings.Map(add1, "HAL-9000")) // "IBM.:111"
fmt.Println(strings.Map(add1, "VMS")) // "WNT"
fmt.Println(strings.Map(add1, "Admix")) // "Benjy"
```

#### 5.6 匿名函数

_匿名函数只能在包级别地作用域进行声明, 但我们能够使用函数字面量在任何表达式内指定函数变量. 函数字面量就像函数声明, 但在 func 关键字后面没有函数名称. 它是一个表达式, 它地值称作匿名函数._  
_函数字面量在我们需要使用地时候才定义._

```go
strings.Map(func(r rune) rune { return r + 1 }, "HAL-9000")
```

_以这种方式定义地函数能够获取到整个词法环境, 因此里层函数可以使用外层函数地变量:_

```go
// squares returns a function that returns
// the next square number each time it is called.
func squares() func() int {
var x int
return func() int {
x++
return x * x
}
}
func main() {
f := squares()
fmt.Println(f()) // "1"
fmt.Println(f()) // "4"
fmt.Println(f()) // "9"
fmt.Println(f()) // "16"
}
```

> 函数 squares 返回了另一个函数, 类型是 func() int . 调用 squares 创建了一个局部变量 x 而且返回了一个匿名函数, 每次调用 squares 都会递增 x 地值然后返回 x 的平方. 第二次调用 squares 函数将创建第二个变量 x, 然后返回一个递增 x 值的新匿名函数.  
> _实例演示了函数变量不仅是一段代码还可以拥有状态. 里层的匿名函数可以获取和更新外层 squares 函数的局部变量. 这些隐藏的变量引用就是我们把函数归类为引用类型而且函数变量无法进行比较的原因. 函数变量类似于使用闭包方法实现的变量, Go 程序员通常把函数变量称为闭包._  
> _这个例子里变量的生命周期不是由它的作用域所决定: 变量 x 在 main 函数中返回 squares 函数后依旧存在 (虽然 x 在这个时候是隐藏在函数变量 f 中的)._  
> _在下面的示例中, 考虑学习计算机科学技术课程的顺序, 需要计算出学习每一门课程的先决条件. 先决课程在下面的 prereqs 表中已经给出, 其中给出了每一门课程必须提前完成的课程列表关系._

```go
var prereqs = map[string][]string{
"algorithms": {"data structures"},
"calculus": {"linear algebra"},
"compilers": {
"data structures",
"formal languages",
"computer organization",
},
"data structures": {"discrete math"},
"databases": {"data structures"},
"discrete math": {"intro to programming"},
"formal languages": {"discrete math"},
"networks": {"operating systems"},
"operating systems": {"data structures", "computer organization"},
"programming languages": {"data structures", "computer organization"},
}
```

_这样的问题是我们熟知的拓扑排序. 先决条件的内容构成一张有向图, 每一个节点代表一门课程, 每一条边代表一门课程所依赖另一门课程的关系. 图是无环的: 没有节点可以通过图上的路径回到它自己. 我们可以使用深度优先的搜索算法计算得到合法的学习路径:_

```go
package main

import (
    "fmt"
    "sort"
)

func main() {
    for i, course := range topoSort(prereqs) {
        fmt.Printf("%d:\t%s\n", i+1, course)
    }
}
func topoSort(m map[string][]string) []string {
    var order []string
    seen := make(map[string]bool)
    var visitAll func(items []string)
    visitAll = func(items []string) {
        for _, item := range items {
            if !seen[item] {
                seen[item] = true
                visitAll(m[item])
                order = append(order, item)
            }
        }
    }
    var keys []string
    for key := range m {
        keys = append(keys, key)
    }
    sort.Strings(keys)
    visitAll(keys)
    return order
}
```

#### 警告: 捕获迭代变量

_假设一个程序必须创建一系列的目录之后又会删除它们. 可以使用一个包含函数变量的 slice 进行清理操作._

```go
var rmdirs []func()
for _, d := range tempDirs() {
dir := d // NOTE: necessary!
os.MkdirAll(dir, 0755) // creates parent directories too
rmdirs = append(rmdirs, func() {
os.RemoveAll(dir)
})
}
// ...do some work...
for _, rmdir := range rmdirs {
rmdir() // clean up
}
```

_为什么循环体内将循环变量赋给一个新的局部变量 dir, 而不是在下面这个略有错误的变体中直接使用循环变量 dir._

```go
var rmdirs []func()
for _, dir := range tempDirs() {
os.MkdirAll(dir, 0755)
rmdirs = append(rmdirs, func() {
os.RemoveAll(dir) // NOTE: incorrect!
})
}
```

_这个原因是循环变量的作用域的规则限制. 在上面的程序中, dir 在 for 循环引进的一个块作用域进行声明. 在循环体里创建的所有函数变量共享相同的变量: 同一个可访问的存储位置, 而不是固定的值. dir 变量的值在不断地迭代中更新, 因此当调用清理函数时, dir 变量已经被每一次的 for 循环更新多次. 因此, dir 变量的实际取值是最后一次迭代时的值并且所有的 os.RemoveAll 调用最终都试图删除同一个目录._  
_我们通常引入一个内部变量来解决这个问题, 就像 dir 变量时一个和外部变量同名的变量, 只不过时一个副本:_

```go
for _, dir := range tempDirs() {
dir := dir // declares inner dir, initialized to outer dir
// ...
}
```

_这样的隐患不仅仅存在于使用 range 的 for 循环里. 在下面的循环中也面临由于无意间捕获的索引变量 i 而导致的同样问题._

```go
var rmdirs []func()
dirs := tempDirs()
for i := 0; i < len(dirs); i++ {
os.MkdirAll(dirs[i], 0755) // OK
rmdirs = append(rmdirs, func() {
os.RemoveAll(dirs[i]) // NOTE: incorrect!
})
}
```

#### 5.7 变长函数

_变长函数被调用时候可以有可变的参数个数._  
_在参数列表最后的类型名称之前使用省略号 "..." 表示声明一个变长函数, 调用这个函数的时候可以传递该类型任意数目的参数._

```go
func sum(vals ...int) int {
total := 0
for _, val := range vals {
total += val
}
return total
}
```

> 上面这个 sum 函数返回零个或者多个 int 参数. 在函数体内, vals 是一个 int 类型的 slice. 调用 sum 的时候任何数量的参数都将提供给 vals 参数.

```go
fmt.Println(sum()) // "0"
fmt.Println(sum(3)) // "3"
fmt.Println(sum(1, 2, 3, 4)) // "10"
```

_调用者显式地申请一个数组, 将实参复制给这个数组, 并把一个数组 slice 传递给函数. 上面地最后一个调用和下面的调用的作用是一样的, 它展示了当实参已经存在于一个 slice 中的时候如何调用一个变长函数: 在最后一个参数后面放一个省略号._

```go
values := []int{1, 2, 3, 4}
fmt.Println(sum(values...)) // "10"
```

_尽管 ...int 参数就像函数体内的 slice, 但是变长函数的类型和一个带有普通 slice 参数的函数的类型不同._

```go
func f(...int) {}
func g([]int) {}
fmt.Printf("%T\n", f) // "func(...int)"
fmt.Printf("%T\n", g) // "func([]int)"
```

_变长函数通常用于格式化字符串._

```go
func errorf(linenum int, format string, args ...interface{}) {
fmt.Fprintf(os.Stderr, "Line %d: ", linenum)
fmt.Fprintf(os.Stderr, format, args...)
fmt.Fprintln(os.Stderr)
}
linenum, name := 12, "count"
errorf(linenum, "undefined: %s", name) // "Line 12: undefined: count"
```

_interface{} 类型意味着这个函数最后一个参数可以接受任何值._

#### 5.8 延迟函数调用

_下面的程序获取一个 HTML 文档然后输出它的标题. title 函数检测从服务器端回的 Content-Type 头部, 如果文档不是 HTML 则返回错误._

```go
func title(url string) error {
resp, err := http.Get(url)
if err != nil {
return err
}
// Check Content-Type is HTML (e.g., "text/html; charset=utf-8").
ct := resp.Header.Get("Content-Type")
if ct != "text/html" && !strings.HasPrefix(ct, "text/html;") {
resp.Body.Close()
return fmt.Errorf("%s has type %s, not text/html", url, ct)
}
doc, err := html.Parse(resp.Body)
resp.Body.Close()
if err != nil {
return fmt.Errorf("parsing %s as HTML: %v", url, err)
}
visitNode := func(n *html.Node) {
if n.Type == html.ElementNode && n.Data == "title" &&
n.FirstChild != nil {
fmt.Println(n.FirstChild.Data)
}
}
```

> 观察重复的 resp.Body.CLose() 调用, 它保证 title 函数在任何执行路径下都会关闭网络连接. 通常我们使用 defer 机制让工作变得简单.  
> _语法上, 一个 defer 语句就是一个普通的函数或者方法调用, 在调用之前加上关键字 defer. 函数和参数表达式会在语句执行时求值, 但是无论是正常情况下, 执行 return 语句或函数执行完毕, 还是在不正常情况下, 比如发生宕机, 实际的调用推迟到包含 defer 语句的函数结束后才执行. defer 语句没有限制使用次数; 执行的时候可以调用 defer 语句顺序的倒序进行._  
> _defer 语句经常使用于成对的操作, 比如打开和关闭, 连接和断开, 枷锁和解锁, 即使是在复杂的控制流, 资源在任何情况下都能够正确释放. 正确的使用 defer 语句的地方是在成功获取资源后._

```go
func title(url string) error {
resp, err := http.Get(url)
if err != nil {
return err
}
defer resp.Body.Close()
ct := resp.Header.Get("Content-Type")
if ct != "text/html" && !strings.HasPrefix(ct, "text/html;") {
return fmt.Errorf("%s has type %s, not text/html", url, ct)
}
doc, err := html.Parse(resp.Body)
if err != nil {
return fmt.Errorf("parsing %s as HTML: %v", url, err)
}
// ...print doc's title element...
return nil
}
```

_同样的方法可以使用在其他资源 (包括网络连接) 上, 比如关闭一个打开的文件:_

```go
func ReadFile(filename string) ([]byte, error) {
f, err := os.Open(filename)
if err != nil {
return nil, err
}
defer f.Close()
return ReadAll(f)
}
```

_或者解锁一个互斥锁:_

```go
var mu sync.Mutex
var m = make(map[string]int)

func lookup(key string) int {
mu.Lock()
defer mu.Unlock()
return m[key]
}
```

_defer 语句也可以用来调试一个复杂的函数, 即在函数的 '入口' 和 '出口' 处设置调试行为. 但别忘记 defer 语句末尾的圆括号, 或者入口的操作会在函数推出时执行而出口的操作永远不会调用._

```go
func bigSlowOperation() {
defer trace("bigSlowOperation")() // don't forget the extra parentheses
// ...lots of work...
time.Sleep(10 * time.Second) // simulate slow operation by sleeping
}
func trace(msg string) func() {
start := time.Now()
log.Printf("enter %s", msg)
return func() { log.Printf("exit %s (%s)", msg, time.Since(start)) }
}
```

_调用结果:_

```go
 go build gopl.io/ch5/trace
 ./trace
2015/11/18 09:53:26 enter bigSlowOperation
2015/11/18 09:53:36 exit bigSlowOperation (10.000589217s)
```

_延迟执行的函数在 return 语句执行之后执行, 并且可以更新函数的结果变量. 因为匿名函数可以得到其外层函数作用域内的变量 (包括命名结果), 所以延迟执行的匿名函数可以观察到函数的返回结果._  
_考虑下面的函数 double_

```go
func double(x int) int {
    return x + x
}
```

_通过命名结果变量和增加 defer 语句, 我们能够在每次调用函数的时候输出它的参数和结果:_

```go
func double(x int) (result int) {
    defer func() { fmt.Printf("double(%d) = %d\n", x, result) }()
    return x + x
}
_ = double(4)
// Output:
// "double(4) = 8"
```

_延迟执行的匿名函数能够改变外层函数返回给调用者的结果:_

```go
func triple(x int) (result int) {
defer func() { result += x }()
return double(x)
}
fmt.Println(triple(4)) // "12"
```

_因为延迟函数不到最后一刻是不会执行的. 要注意在循环里 defer 语句的使用. 下面的这段代码就可能会用尽所有的文件描述符, 这是因为处理完成后却没有文件关闭._

```go
for _, filename := range filenames {
f, err := os.Open(filename)
if err != nil {
return err
}
defer f.Close() // NOTE: risky; could run out of file descriptors
// ...process f...
}
```

_一种解决方式是将循环体 (包括 defer 语句) 放到另外一个函数里, 每次循环迭代都会调用文件关闭函数._

```go
for _, filename := range filenames {
if err := doFile(filename); err != nil {
return err
}
}
func doFile(filename string) error {
f, err := os.Open(filename)
if err != nil {
return err
}
defer f.Close()
// ...process f...
}
```

#### 5.9 宕机

_当 Go 语言运行时检测到错误时, 它就会发生宕机._  
_一个典型的宕机发生时, 正常的程序执行会终止, goroutine 中的所有延迟函数会执行, 然后程序异常退出并留下一条日志信息. 日志信息包括宕机的值, 这往往代表某种错误消息, 每一个 goroutine 都会在宕机的时候显示一个函数调用的栈跟踪信息._  
_并不是所有宕机都是在运行时发生的, 可以直接调用内置的宕机函数; 内置的宕机函数可以接受任何值作为参数. 如果碰到 '不可能发生' 的情况, 宕机是最好的处理方式, 比如语句执行到逻辑上不可能到达的地方时:_

```go
switch s := suit(drawCard()); s {
case "Spades": // ...
case "Hearts": // ...
case "Diamonds": // ...
case "Clubs": // ...
default:
panic(fmt.Sprintf("invalid suit %q", s)) // Joker&#63;
}
```

_当宕机发生时, 所有的延迟函数以倒序执行, 从栈最上面的函数开始一直返回到 main 函数:_

```go
func main() {
f(3)
}
func f(x int) {
fmt.Printf("f(%d)\n", x+0/x) // panics if x == 0
defer fmt.Printf("defer %d\n", x)
f(x - 1)
}
```

_运行时, 程序会输出下面的内容到标准输出._

```go
f(3)
f(2)
f(1)
defer 1
defer 2
defer 3
```

_runtime 包提供了转储栈的方法用来诊断错误._

```go
func main() {
defer printStack()
f(3)
}
func printStack() {
var buf [4096]byte
n := runtime.Stack(buf[:], false)
os.Stdout.Write(buf[:n])
}
```

_Go 语言的宕机机制让延迟执行的函数在栈清理之前调用._

#### 5.10 恢复

_退出程序通常是正确处理宕机的方式, 但是也有例外. 在一定情况下是可以进行恢复的, 至少有时候可以在退出之前清理当前混乱的情况._  
_如果内置的 recover 函数在延迟函数的内部调用, 而且这个包含 defer 语句的函数发生宕机, recover 会终止当前宕机状态并返回宕机的值. 函数不会从之前宕机的地方继续运行而是正常返回. 如果 recover 在其他任何情况下运行则它没有任何效果且返回 nil._

```go
func Parse(input string) (s *Syntax, err error) {
defer func() {
if p := recover(); p != nil {
err = fmt.Errorf("internal error: %v", p)
}
}()
// ...parser...
}
```

> Parse 函数中的延迟函数会从宕机状态恢复, 并使用宕机值组成一条错误信息; 理想的写法是使用 runtime.Stack 将整个调用栈包含进来. 延迟函数则将错误值赋给 err 结果变量, 从而返回给调用者.  
> _对于宕机采用无差别的恢复措施是不可靠的, 因为宕机后包内变量的状态往往没有清晰的定义和解释._  
> _从同一个包内发生的宕机进行恢复有助于简化处理复杂和未知的错误, 但一般的原则是, 不应该尝试恢复从另一个包内发生的宕机. 公共的 API 应该直接报告错误. 同样, 也不应该恢复一个宕机, 而这段代码却不是由你来维护的._  
> _最安全的做法是要选择性地使用 recover. 可以通过使用一个明确地, 非导出类型作为宕机值, 之后检测 recover 地返回值是否是这个类型. 如果是这类型, 可以像普通地 error 那样处理宕机; 如果不是, 使用相同一个参数调用 panic 以继续触发宕机._

```go
// soleTitle returns the text of the first non-empty title element
// in doc, and an error if there was not exactly one.
func soleTitle(doc *html.Node) (title string, err error) {
type bailout struct{}
defer func() {
switch p := recover(); p {
case nil:
// no panic
case bailout{}:
// "expected" panic
err = fmt.Errorf("multiple title elements")
default:
panic(p) // unexpected panic; carry on panicking
}
}()
// Bail out of recursion if we find more than one non-empty title.
forEachNode(doc, func(n *html.Node) {
if n.Type == html.ElementNode && n.Data == "title" &&
n.FirstChild != nil {
if title != "" {
panic(bailout{}) // multiple title elements
}
title = n.FirstChild.Data
}
}, nil)
if title == "" {
return "", fmt.Errorf("no title element")
}
return title, nil
}
```

_有些情况下是没有恢复动作地. 比如, 内存耗尽使得 Go 运行时发生严重错误而直接终止进程._

### 第 6 章 方法

_对象就是一个简单的一个值或者变量, 并且拥有方法, 而方法是某种特定的函数. 面向对象编程就是使用方法来描述每个数据结构的属性和操作, 于是, 使用者不需要了解对象本身的实现._

#### 6.1 方法声明

_方法声明和普通函数的声明类似, 只是在函数名字前面多了一个参数. 这个参数把这个方法绑定到这个参数对应的类型上._

```go
type Point struct{ X, Y float64 }

// traditional function
func Distance(p, q Point) float64 {
    return math.Hypot(q.X-p.X, q.Y-p.Y)
}

// same thing, but as a method of the Point type
func (p Point) Distance(q Point) float64 {
    return math.Hypot(q.X-p.X, q.Y-p.Y)
}

```

_附加的参数 p 称为方法的接收者, 它源自早先的面向对象语言, 用来描述主调方法就像向对象发送消息_  
_Go 语言中, 接收者不使用特殊名 (比如 this 或者 self); 而是我们自己选择接收者名字, 就像其他的参数变量一样. 由于接收者会频繁地使用, 因此最好能够选择简短且在整个方法中名称始终保持一致地名字. 最常用地方法就是取类型名称地首字母._  
_调用方法地时候, 接收者在方法名地前面. 这样就和声明保持一致._

```go
p := Point{1, 2}
q := Point{4, 6}
fmt.Println(Distance(p, q)) // "5", function call
fmt.Println(p.Distance(q)) // "5", method call
```

_上面两个 Distance 函数声明没有冲突. 第一个声明一个包级别的函数 (称为 geometry.Distance). 第二个声明一个类型 Point 的方法, 因此它的名字是 Point.Distance._  
_表达式 p.Distance 称作选择子 (selector), 因为它为接收者 p 选择合适的 Distance 方法. 选择子也用于选择结构体类型中的某些字段值, 就像 p.X 中的字段值. 由于方法和字段来自同一个命名空间, 因此 Point 结构体类型中如果声明一个叫作 X 的方法会与字段 x 冲突, 编译器会报错._  
_因为每一个类型有它自己的命名空间, 所以我们能够在其他不同的类型中使用名字 Distance 作为方法名. 定义一个 Path 类型表示一条线段, 同样使用 Distance 作为方法名._

```go
// A Path is a journey connecting the points with straight lines.
type Path []Point

// Distance returns the distance traveled along the path.
func (path Path) Distance() float64 {
    sum := 0.0
    for i := range path {
        if i > 0 {
            sum += path[i-1].Distance(path[i])
        }
    }
    return sum
}
```

_Path 是一个命名的 slice 类型, 而非 Point 这样的结构体类型, 我们依旧可以给它定义方法. Go 可以将方法绑定到任何类型上. 可以方便地为简单地类型 (如数字, 字符串, slice, map, 甚至函数等) 定义附加地行为. 同一个包下的任何类型都可以声明方法, 只要它的类型既不是指针类型也不是接口类型._  
_这两个 Distance 方法拥有不同的类型, 它们彼此无关, 尽管 Path.Distance 在内部使用 Point.Distance 来计算线段相邻点之间的距离._  
_类型拥有的所有方法名都必须是唯一的, 但不同的类型可以使用相同的方法名. 也没有必要使用附加字段来修饰方法名以示区别. 在包的外部进行调用时候, 方法能够使用更加简短的名字且省略包的名字:_

```go
import "gopl.io/ch6/geometry"
perim := geometry.Path{{1, 1}, {5, 1}, {5, 4}, {1, 1}}
fmt.Println(geometry.PathDistance(perim)) // "12", standalone function
fmt.Println(perim.Distance()) // "12", method of geometry.Path
```

#### 6.2 指针接收者的方法

_由于主调函数会复制每一个实参变量, 如果需要更新一个变量, 或者如果一个实参太大而我们希望避免复制整个实参, 因此我们必须使用指针来传递变量地址. 这也同样适用于更新接收者:_

```go
func (p *Point) ScaleBy(factor float64) {
p.X *= factor
p.Y *= factor
}
```

> 这个方法的名字是 (_Point).ScaleBy. 圆括号是必需的; 没有圆括号, 表达式会被解析成_(Point.ScaleBy).  
> _在实际的程序中, 习惯上遵循如果 Point 的任何一个方法使用指针接收者, 那么所有的 Point 方法都应该使用指针接收者, 即使有些方法并不一定需要._  
> 命名类型 (Point) 与指向它们的指针 (\*Point) 是唯一可以出现在接收者声明处的类型. 而且, 为防止混淆, 不允许本身是指针的类型进行方法声明:

```go
type P *int
func (P) f() { /* ... */ } // compile error: invalid receiver type
```

*通过提供 \\*Point 能够调用(\*Point).ScaleBy 方法, 比如:\*

```go
r := &Point{1, 2}
r.ScaleBy(2)
fmt.Println(*r) // "{2, 4}"
```

_或者:_

```go
p := Point{1, 2}
pptr := &p
pptr.ScaleBy(2)
fmt.Println(p) // "{2, 4}
```

_或者:_

```go
p := Point{1, 2}
(&p).ScaleBy(2)
fmt.Println(p) // "{2, 4}"
```

_如果接收者 p 是 Point 类型的变量, 但方法要求一个 \*Point 接收者, 可以简写:_

```go
p.ScaleBy(2)
```

_实际上编译器会对变量进行 &p 的隐式转换. 只有变量才允许这么做. 不能对一个不能取地址的 Point 接收者参数调用 \*Point 方法, 因为无法获取临时变量的地址._

```go
Point{1, 2}.ScaleBy(2) // compile error: can't take address of Point literal
```

_如果实参接收者是 \*Point 类型, 以 Point.Distance 地方式调用 Point 类型地方法是合法的, 因为可以从地址中获取 Point 的值; 只要解引用指向接收者的指针即可. 编译器自动插入一个隐式的 \* 操作符. 下面两个函数调用效果是一样的:_

```go
pptr.Distance(q)
(*pptr).Distance(q)
```

_在合法的方法调用表达式中, 只有符合下面三种形式的语句才能够成立._

> 实际接收者和形参接收者是同一个类型, 比如都是 T 类型或是 \*T 类型:

```go
Point{1, 2}.Distance(q) // Point
pptr.ScaleBy(2) // *Point
```

> 或者实参接收者是 T 类型的变量而形参接收者是 \*T 类型. 编译器会隐式地获取变量地地址.

```go
p.ScaleBy(2) // implicit (&p)
```

> 或者实参接收者是 \*T 类型而形参接收者是 T 类型. 编译器会隐式地解引用接收者, 获得实际地取值.

```go
pptr.Distance(q) // implicit (*pptr)
```

*如果所有类型 T 方法的接收者是 T 自己 (而非*T), 那么复制它的实例是安全的; 调用方法的时候必须进行一次复制. 但是任何方法的接收者是指针的情况下, 应该避免复制 T 的实例, 因为这么做可能会破环内部原本的数据.\*

##### nil 是一个合法的接收者

_就像一些函数允许 nil 指针作为实参, 方法接收者也是一样, 尤其是当 nil 是类型中有意义的零值时._  
_当定义一个类型允许 nil 作为接收者时, 应当在文档注释中显示地表明._

#### 6.3 通过结构体内嵌组成类型

考虑 ColoredPoint 类型

```go
import "image/color"
type Point struct{ X, Y float64 }
type ColoredPoint struct {
Point
Color color.RGBA
}
```

> ColoredPoint 内嵌了一个 Point 类型. 它包含 Point 类型所有字段以及其他更多的自有字段. 如有需要, 可以直接使用 ColoredPoint 内所有字段而不需要 Point 类型:

```go
var cp ColoredPoint
cp.X = 1
fmt.Println(cp.Point.X) // "1"
cp.Point.Y = 2
fmt.Println(cp.Y) // "2"
```

> 同理, 这也适用于 Point 类型的方法. 我们能够通过类型为 ColoredPoint 的接收者调用内嵌类型 Point 的 方法, 即使在 ColoredPoint 类型没有声明过这个方法的情况:

```go
red := color.RGBA{255, 0, 0, 255}
blue := color.RGBA{0, 0, 255, 255}
var p = ColoredPoint{Point{1, 1}, red}
var q = ColoredPoint{Point{5, 4}, blue}
fmt.Println(p.Distance(q.Point)) // "5"
p.ScaleBy(2)
q.ScaleBy(2)
fmt.Println(p.Distance(q.Point)) // "10"
```

> Point 的方法都被纳入到 ColoredPoint 类型中. 以这种方式, 内嵌允许构成复杂的类型, 该类型由许多字段构成, 每个字段提供一些方法.  
> 熟悉基于类的面向对象编程语言的读者可能认为 Point 类型就是 ColoredPoint 类型的基类, 而 ColoredPoint 则作为子类或者派生类, 或将这两个之间的关系翻译为 ColoredPoint 就是 Point 的其中一个表现. 但这是一个**误解**. 注意上面调用 Distance 的地方. Distance 有一个形参 Point, q 不是 Point, 因此虽然 q 有一个内嵌 Point 字段, 但是必须显示地使用它. 尝试直接传递 q 作为参数会报错:

```go
p.Distance(q) // compile error: cannot use q (ColoredPoint) as Point
```

> ColoredPoint 并不是 Point, 但是它包含一个 Point, 并且它有另外两个地方法 Distance 和 ScaleBy 来自 Point. 如果考虑具体实现, 实际上, 内嵌地字段会告诉编译器生成额外地包装方法来调用 Point 声明的方法, 这相当于以下代码:

```go
func (p ColoredPoint) Distance(q Point) float64 {
return p.Point.Distance(q)
}
func (p *ColoredPoint) ScaleBy(factor float64) {
p.Point.ScaleBy(factor)
}
```

_匿名字段类型可以是指向命名类型的指针, 这个时候, 字段和方法间接地来自于所指向的对象. 这可以让我们共享通用的结构以及使对象之间的关系更加动态, 多样化._

```go
type ColoredPoint struct {
*Point
Color color.RGBA
}
p := ColoredPoint{&Point{1, 1}, red}
q := ColoredPoint{&Point{5, 4}, blue}
fmt.Println(p.Distance(*q.Point)) // "5"
q.Point = p.Point // p and q now share the same Point
p.ScaleBy(2)
fmt.Println(*p.Point, *q.Point) // "{2 2} {2 2}"
```

_结构体类型可以拥有多个匿名字段._

```go
type ColoredPoint struct {
Point
color.RGBA
}
```

> 这个类型的值可以拥有 Point 所有的方法和 RGBA 所有的方法, 以及任何其他直接在 ColoredPoint 类型中声明的方法. 当编译器处理选择子 (比如: p.ScaleBy) 的时候, 首先, 它先查找到直接声明的方法 ScaleBy, 之后再从来自 ColoredPoint 的内嵌字段的方法中进行查找, 再之后从 Point 和 RGBA 中内嵌字段的方法中进行查找, 以此类推. 当同一个查找级别中有相同方法名时, 编译器会报告选择子不明确的错误.  
> 方法只能在命名类型 (比如 Point) 和它们的指针 (\*Point) 中声明, 但内嵌帮助我们能够在未命名的结构体中声明方法.  
> 下面的例子展示了简单的缓存实现, 其中使用了两个包级别的变量: 互斥锁和 map, 互斥锁会保护 map 的数据.

```go
var (
mu sync.Mutex // guards mapping
mapping = make(map[string]string)
)
func Lookup(key string) string {
mu.Lock()
v := mapping[key]
mu.Unlock()
return v
}
```

> 下面这个版本的功能和上面完全相同, 但是将两个相关变量放到了一个包级别的变量 cache 中:

```go
var cache = struct {
sync.Mutex
mapping map[string]string
} {
mapping: make(map[string]string),
}
func Lookup(key string) string {
cache.Lock()
v := cache.mapping[key]
cache.Unlock()
return v
}
```

#### 6.4 方法变量与表达式

_通常我们都是在相同的表达式里使用和调用方法, 就像在 p.Distance() 中, 但是把两个操作分开也是可以的. 选择子 p.Distance 可以赋予一个方法变量, 它是一个函数, 把方法 (Point.Distance) 绑定到一个接收者 p 上. 函数只需要提供实参而不需要提供接收者就能够调用._

```go
p := Point{1, 2}
q := Point{4, 6}
distanceFromP := p.Distance // method value
fmt.Println(distanceFromP(q)) // "5"
var origin Point // {0, 0}
fmt.Println(distanceFromP(origin)) // "2.23606797749979", ; 5
scaleP := p.ScaleBy // method value
scaleP(2) // p becomes (2, 4)
scaleP(3) // then (6, 12)
scaleP(10) // then (60, 120)
```

_如果包内的 API 调用一个函数值, 并且使用者希望这个函数的行为是调用一个特定接收者的方法, 方法变量就非常有用._

```go
type Rocket struct { /* ... */ }
func (r *Rocket) Launch() { /* ... */ }
r := new(Rocket)
time.AfterFunc(10 * time.Second, func() { r.Launch() })
```

> 如果使用方法变量则可以更简洁:

```go
time.AfterFunc(10 * time.Second, r.Launch)
```

> 于方法变量相关的是方法表达式. 和调用一个普通的函数不同, 在调用方法的时候必须提供接收者, 并且按照选择子的语法进行调用. 而方法表达式写成 T.f 或者 (\*T).f, 其中 T 是类型, 是一种函数变量, 把原来方法的接收者替换成函数的第一个形参, 因此它可以像平常函数一样调用.

```go
p := Point{1, 2}
q := Point{4, 6}
distance := Point.Distance // method expression
fmt.Println(distance(p, q)) // "5"
fmt.Printf("%T\n", distance) // "func(Point, Point) float64"
scale := (*Point).ScaleBy
scale(&p, 2)
fmt.Println(p) // "{2 4}"
fmt.Printf("%T\n", scale) // "func(*Point, float64)"
```

> 如果需要用一个值来代表多个方法中的一个, 而方法都属于同一个类型, 方法变量可以帮助你调用这个值对应的方法来处理不同的接收者.

```go
type Point struct{ X, Y float64 }
func (p Point) Add(q Point) Point { return Point{p.X + q.X, p.Y + q.Y} }
func (p Point) Sub(q Point) Point { return Point{p.X - q.X, p.Y - q.Y} }
type Path []Point
func (path Path) TranslateBy(offset Point, add bool) {
var op func(p, q Point) Point
if add {
op = Point.Add
} else {
op = Point.Sub
}
for i := range path {
// Call either path[i].Add(offset) or path[i].Sub(offset).
path[i] = op(path[i], offset)
}
}
```

#### 6.5 示例: 位向量

_在数据流分析领域, 集合元素都是小的非负整形, 集合拥有许多元素, 而且集合元素操作多数是求并集和交集, 位向量是个理想的数据结构._  
_位向量使用一个无符号整型值的 slice, 每一位代表集合中的一个元素. 如果设置第 i 位的元素, 则认为集合包含 i._

```go
// An IntSet is a set of small non-negative integers.
// Its zero value represents the empty set.
type IntSet struct {
    words []uint64
}

// Has reports whether the set contains the non-negative value x.
func (s *IntSet) Has(x int) bool {
    word, bit := x/64, uint(x%64)
    return word < len(s.words) && s.words[word]&(1<<bit) add="""""add""""" adds="""""adds""""" the="""""the""""" non-negative="""""non-negative""""" value="""""value""""" x="""""x""""" the="""""the""""" set="""""set""""" func="""""func""""" s="""""s""""" intset="""""intset""""" add="""""add""""" int="""""int""""" word="""""word""""" bit="""""bit""""" x="""""x""""" uint="""""uint""""" for="""""for""""" word="""""word""""">= len(s.words) {
        s.words = append(s.words, 0)
    }
    s.words[word] |= 1 << bit
}

// UnionWith sets s to the union of s and t.
func (s *IntSet) UnionWith(t *IntSet) {
    for i, tword := range t.words {
        if i < len(s.words) {
            s.words[i] |= tword
        } else {
            s.words = append(s.words, tword)
        }
    }
}
```

> 由于每一个字拥有 64 位, 因此为了定位 x 位的位置, 我们使用商数 x/64 作为字的索引, 而 x%64 记作该字内位的索引. UnionWith 操作使用按位 "或" 操作符 | 来计算一次 64 个元素求并集的结果.

#### 6.6 封装

_如果变量或者方法是不能通过对象访问到的, 这称作封装的变量或者方法. 封装 (有时称作数据隐藏) 是面向对象编程中重要的一方面._  
_Go 语言只有一种方式控制命名的可见性: 定义的时候, 首字母大写的标识符是可以从包中导出的, 而首字母没有大写的则不导出. 同样的机制也同样作用于结构体内的字段和类型中的方法. 结论是, 要封装一个对象, 必须使用结构体._  
_Go 语言中封装单元是包而不是类型. 无论是在函数内的代码还是方法内的代码, 结构体内的字段对于同一个包中的所有代码都是可见的._  
_封装提供了三个优点:_

> 第一, 因为使用方不能直接修改对象的变量, 所以不需要更多的语句用来检查变量的值.  
> 第二, 隐藏实现细节可以防止使用方依赖的属性发生改变, 使得设计者可以更加灵活地改变 API 地实现而不破环兼容性.  
> 第三, 就是可以防止使用者肆意改变对象内部地变量. 因为对象地变量只能被同一个包地函数修改, 所以包地作者能够保证所有地函数都可以维护对象内部地资源.

### 第 7 章 接口

_接口类型是对其他类型行为地概况与抽象. 通过使用接口, 可以写出更加灵活通用地函数, 这些函数不用绑定在一个特定地类型实现上._  
_Go 语言地接口地独特之处在于它是隐式实现. 对于一个具体类型, 无须声明它实现了那些接口, 只要提供接口所必需地方法即可. 这种设计无需改变已有类型地实现, 就可以为这些类型创建新地接口._

#### 7.1 接口即约定

_具体类型指定了它所含数据地精确布局, 还是暴露基于这个精确布局地内部称作._  
_Go 语言中存在一种类型称为接口类型. 接口时一种抽象类型, 它并没有曝露所含数据地布局或者内部结构, 当然也没有那些数据的基本称作, 它所提供的仅仅是一些方法而已._

```go
package io
// Writer is the interface that wraps the basic Write method.
type Writer interface {
// Write writes len(p) bytes from p to the underlying data stream.
// It returns the number of bytes written from p (0 <= n <= len(p))
// and any error encountered that caused the write to stop early.
// Write must return a non-nil error if it returns n < len(p).
// Write must not modify the slice data, even temporarily.
//
// Implementations must not retain p.
Write(p []byte) (n int, err error)
}
```

_io.Writer 接口定义了和调用者之间的约定. 一方面, 这个约定要求调用者提供具体类型(比如 \*os.File 或者\*bytes.Buffer) 包含一个与其签名和行为一致的 Write 方法. 另一方面, 这个约定保证了 Fprint 能使用任何满足 io.Writer 接口的参数._  
_这种可以把一种类型替换为满足同一接口的另一种类型的特性称为可取代性 (substitutability), 这也是面向对象语言的典型特征._

#### 7.2 接口类型

_一个接口类型定义了一套方法, 如果一个具体类型要实现该接口, 那么必须实现该接口类型定义中的所有方法._  
_可以通过组合已有接口得到新的接口:_

```go
package io

type Reader interface {
    Read(p []byte) (n int, err error)
}
type Closer interface {
    Close() error
}
type ReadWriter interface {
    Reader
    Writer
}
type ReadWriteCloser interface {
    Reader
    Writer
    Closer
}
```

> 如上的语法称为嵌入式接口, 与嵌入式结构体类似, 让我们可以直接使用一个接口, 而不用逐一写出这个接口所包含的方法. 如下所示, 尽管不够简洁, 但是可以不用嵌入式来声明 io.ReadWriter:

```go
type ReadWriter interface {
Read(p []byte) (n int, err error)
Write(p []byte) (n int, err error)
}
```

> 或者混合使用两种方式:

```go
type ReadWriter interface {
Read(p []byte) (n int, err error)
Writer
}
```

#### 7.3 实现接口

_如果一个类型实现了一个接口要求的所有方法, 那么这类型实现了这个接口._  
_Go 程序员通常说一个具体类型 '是一个' (is-a) 特定的接口类型, 代表着该类型实现了该接口._  
_接口的赋值规则很简单, 仅当一个表达式实现一个接口时, 这个表达式才可以赋值给该接口._

```go
var w io.Writer
w = os.Stdout // OK: *os.File has Write method
w = new(bytes.Buffer) // OK: *bytes.Buffer has Write method
w = time.Second // compile error: time.Duration lacks Write method
var rwc io.ReadWriteCloser
rwc = os.Stdout // OK: *os.File has Read, Write, Close methods
rwc = new(bytes.Buffer) // compile error: *bytes.Buffer lacks Close method
```

_当右侧表达式也是一个接口时, 该规则也有效:_

```go
w = rwc // OK: io.ReadWriteCloser has Write method
rwc = w // compile error: io.Writer lacks Close method
```

*对每一个具体类型 T, 部分方法的接收者就是 T, 而该方法的接收者则是*T 指针. 同时我们对类型 T 的变量直接调用 *T 的方法也可以是合法的, 只要改变量是可变的, 编译器隐式地完成了取地址地操作. 但这仅仅是一个语法糖, 类型 T 地方法没有对应地指针*T 多, 所以实现地接口也可能比对应地指针少.\*  
_接口也封装了所对应地类型和数据, 只有通过接口暴露的方法才可以调用, 类型的其他方法则无法通过接口来调用:_

```go
os.Stdout.Write([]byte("hello")) // OK: *os.File has Write method
os.Stdout.Close() // OK: *os.File has Close method
var w io.Writer
w = os.Stdout
w.Write([]byte("hello")) // OK: io.Writer has Write method
w.Close() // compile error: io.Writer lacks Close method
```

_空接口类型 interface{}, 不包含任何方法. 正因为空接口类型对其实现类型没有任何要求, 所以我们可以把任何值赋值给空接口类型._

```go
var any interface{}
any = true
any = 12.34
any = "hello"
any = map[string]int{"one": 1}
any = new(bytes.Buffer)
```

_非空接口类型通常由一个指针类型来实现, 特别是当接口类型的一个或多个方法会修改接收者的情形. 一个指向结构体的指针才是最常见的方法接收者._  
_指针类型不是实现接口的唯一类型, 也可以由 Go 的其他引用类型来实现._  
_一个具体类型可以实现很多不相关的接口._  
_从具体类型出发, 提取其共性而得出每一种分组方式都可以表示为一种接口类型. 与基于类的语言 (它们显式地声明一个类型实现所有接口) 不同的是, 在 Go 语言里我们可以在需要地时才定义新的抽象和分组, 并且不用修改原有类型地定义._

#### 7.4 使用 flag.Value 来解析参数

_我们将使用一个标准接口 flag.Value 来帮助我们定义命令行标志._

```go
var period = flag.Duration("period", 1*time.Second, "sleep period")
func main() {
flag.Parse()
fmt.Printf("Sleeping for %v...", *period)
time.Sleep(*period)
fmt.Println()
}
```

> 在程序进入睡眠前输出了睡眠时长.默认睡眠时间是 1 s, 但是可以用 -period 命令行标志来控制. flag.Duration 函数创建了一个 time.Duration 类型地标志变量, 并且允许用户用一种友好地方式来指定时长  
> 支持自定义类型只须要定义一个满足 flag.Value 接口地类型, 其定义如下所示:

```go
// Value is the interface to the value stored in a flag.
type Value interface {
String() string
Set(string) error
}
```

> String 方法用于格式化标志对应地值, 可用于输出命令行帮助信息. 由于有了该方法, 因此每个 flag.Value 其实也是 fmt.Stringer. Set 方法解析了传入地字符串参数并更新标志值. 可以认为 Set 方法是 String 方法地逆操作.

```go
// *celsiusFlag satisfies the flag.Value interface.
type celsiusFlag struct{ Celsius }
func (f *celsiusFlag) Set(s string) error {
var unit string
var value float64
fmt.Sscanf(s, "%f%s", &value, &unit) // no error check needed
switch unit {
case "C", "°C":
f.Celsius = Celsius(value)
return nil
case "F", "°F":
f.Celsius = FToC(Fahrenheit(value))
return nil
}
return fmt.Errorf("invalid temperature %q", s)
}

// CelsiusFlag defines a Celsius flag with the specified name,
// default value, and usage, and returns the address of the flag variable.
// The flag argument must have a quantity and a unit, e.g., "100C".
func CelsiusFlag(name string, value Celsius, usage string) *Celsius {
f := celsiusFlag{value}
flag.CommandLine.Var(&f, name, usage)
return &f.Celsius
}

var temp = tempconv.CelsiusFlag("temp", 20.0, "the temperature")
func main() {
flag.Parse()
fmt.Println(*temp)
}
```

#### 7.5 接口值

_一个接口类型的值 (简称接口值) 其实有两部分: 一个具体类型和该类型的一个值. 二者称为接口的动态类型和动态值._  
_Go 是静态语言, 类型仅仅是一个编译时的概念, 所以类型不是一个值. 用类型描述符来提供每个类型的具体信息, 比如它的名字和方法. 对于一个接口值, 类型部分就用对应的类型描述符来表述._

> 下面四个语句中, 变量 w 有三个不同的值 (最初和最后是同一个值):

```go
var w io.Writer
w=os.Stdout
w=new(bytes.Buffer)
w=nil
```

_第一个语句声明了 w:_

```go
var w io.Writer
```

> 在 Go 语言中, 变量总是初始化为一个特定的值, 接口也不例外. 接口的零值就是把它的动态类型和值都设置为 nil.  
> 一个接口值是否是 nil 取决于它的动态类型, 所以现在这是一个 nil 接口值. 可以用 w== nil 或者 w != nil 来检测一个接口值是否是 nil. 调用一个 nil 接口的任何方法都会导致崩溃:

```go
w.Write([]byte("hello")) // panic: nil pointer dereference
```

> 第二个语句把一个 \*os.File 类型的值赋给了 w:

```go
w = os.Stdout
```

> 这次赋值把一个具体类型隐式转换为一个接口类型, 它与对应的显式转换 io.Writer(os.Stdout) 等价. 不管这种类型的转换是隐式的还是显式的, 它都可以转换操作数类型和值. 接口值的动态类型会设置为指针类型 *os.File 的类型描述符, 它的动态值会设置为 os.Stdout 的副本, 即一个指向代表进程的标准输出的 os.File 类型的指针.  
> 调用该接口值的 Write 方法, 会实际调用 (*os.File).Write 方法, 即输出 "hello"

```go
w.Write([]byte("hello")) // "hello"
```

> 一般来讲, 在编译时我们无法知道一个接口值的动态类型会是什么, 所以通过接口来做调用必然需要使用动态分发. 编译器必须生成一段代码来从类型描述符拿到名为 Write 的方法地址, 再间接调用该方法地址. 调用的接收者就是接口值的动态值, 即 os.Stdout, 所以实际效果与直接调用等价:

```go
os.Stdout.Write([]byte("hello")) // "hello"
```

> 第三个语句把一个 \*bytes.Buffer 类型的值赋给了接口值:

```go
w=new(bytes.Buffer)
```

> 动态类型现在是 \*bytes.Buffer, 动态值现在则是一个指向新分配缓冲区的指针, 调用 Write 方法的机制也跟第二个语句一致:

```go
w.Write([]byte("hello")) // writes "hello" to the bytes.Buffer
```

> 这次, 类型描述符是 *bytes.Buffer, 所以调用的是 (*bytes.Buffer).Write 方法, 方法的接收者是缓冲区的地址. 调用该方法会追加 "hello" 到缓冲区.  
> 最后, 第四个语句把 nil 赋给了接口值:

```go
w = nil
```

> 这个语句把动态类型和动态值都设置为了 nil, 把 w 恢复到了它刚声明时的状态.  
> 一个接口值可以指向任意大的动态值. 从理论上来讲, 无论动态值有多大, 它永远在接口值内部 (当然这只是一个理论模型; 实际得到实现很不同).  
> 接口值可以用 == 和 != 操作符来做比较. 如果两个接口值都是 nil 或者二者的动态类型完全一致且二者动态值相等 (使用动态类型的 == 操作符来做比较), 那么两个接口值相等. 因为接口值是可以比较的, 所以它们可以作为 map 的键, 也可以作为 switch 语句的操作数.  
> 需要注意的是, 在比较两个接口值时, 如果两个接口值的动态类型一致, 但对应的动态值是不可比较的 (比如 slice), 那么这个比较会以崩溃的方式失败:

```go
var x interface{} = []int{1, 2, 3}
fmt.Println(x == x) // panic: comparing uncomparable type []int
```

> 从这一点来看, 接口类型是非平凡的. 其他类型要么是可以安全比较的 (比如基础类型和指针), 要么是完全不可比较的 (比如 slice, map 和函数), 但当比较接口值或者包含接口值的聚合类型时, 我们必须小心崩溃的可能性. 当把接口作为 map 的键或者 switch 语句的操作数时, 也存在类似的风险. 仅在能确认接口值包含的动态值是可以比较时, 才比较接口值.  
> 当处理错误或者调试时, 能拿到接口值的动态类型是很有帮助的. 可以使用 fmt 包的 %T 来实现这个需求:

```go
var w io.Writer
fmt.Printf("%T\n", w) // "<nil>"
w=os.Stdout
fmt.Printf("%T\n", w) // "*os.File"
w=new(bytes.Buffer)
fmt.Printf("%T\n", w) // "*bytes.Buffer"
```

> 内部实现中, fmt 用反射来拿到接口动态类型的名字.

##### 注意: 含有空指针的非空接口

_空的接口值 (其中不包含任何信息) 与仅仅动态值为 nil 的接口值是不一样的._  
_考虑如下程序, 当 debug 设置为 true 时, 主函数收集函数 f 的输出到一个缓冲区中:_

```go
const debug = true

func main() {
    var buf *bytes.Buffer
    if debug {
        buf = new(bytes.Buffer) // enable collection of output
    }
    f(buf) // NOTE: subtly incorrect!
    if debug {
        // ...use buf...
    }
}

// If out is non-nil, output will be written to it.
func f(out io.Writer) {
    // ...do something...
    if out != nil {
        out.Write([]byte("done!\n"))
    }
}
```

> 当设置 debug 为 false 时, 会导致程序调用 out.Write 时崩溃:

```go
if out != nil {
out.Write([]byte("done!\n")) // panic: nil pointer dereference
}
```

> 当 main 函数调用 f 时, 它把一个类型为 *bytes.Buffer 的空指针赋给了 out 参数, 所以 out 的动态值确实为空. 但它的动态类型是*bytes.Buffer, 这表示 out 是一个包含空指针的非空接口, 所以防御性检查 out != nil 仍然是 true. 解决方案是把 main 函数中的 buf 类型修改为 io.Writer, 从而避免在最开始就把一个功能不完整的值赋值给一个接口.

```go
var buf io.Writer
if debug {
buf = new(bytes.Buffer) // enable collection of output
}
f(buf) // OK
```

#### 7.6 使用 sort.Interface 来排序

_在很多语言中, 排序算法跟序列数据类型绑定, 排序算法则跟序列元素类型绑定. 与之相反的是, Go 语言的 sort.Sort 函数对序列和其中元素的布局无任何要求, 它使用 sort.Interface 接口来指定通用排序算法和每个具体的序列类型之间的协议 (contract). 这个接口的实现确定了序列的具体布局 (经常是一个 slice), 以及元素期望的排序方式._  
_一个原地排序算法需要知道三个信息: 序列长度, 比较两个元素的含义以及如何交换两个元素, 所以 sort.Interface 接口就有三个方法:_

```go
package sort
type Interface interface {
Len() int
Less(i, j int) bool // i, j are indices of sequence elements
Swap(i, j int)
}
```

> 要对序列排序, 需要先确定一个实现了如上三个方法的类型, 接着把 sort.Sort 函数应用到上面这类方法的实列上. 考虑字符串 slice. 定义的新类型 StringSlice 以及它的 Len, Less, Swap 三个方法如下所示:

```go
type StringSlice []string
func (p StringSlice) Len() int { return len(p) }
func (p StringSlice) Less(i, j int) bool { return p[i] < p[j] }
func (p StringSlice) Swap(i, j int) { p[i], p[j] = p[j], p[i] }
```

> 现在就可以对一个字符串 slice 进行排序, 只须简单地把一个 slice 转换为 StringSlice 类型即可, 如下所示:

```go
sort.Sort(StringSlice(names))
```

> 类型转换生成一个新的 slice, 与原始的 names 有相同长度, 容量和底层数组, 不同的是额外增加了三个用于排序的方法.  
> _sort 包专门提供了对于 []int, []string, []float64 自然排序的函数和相关类型. 对于其他类型需要自己写._

#### 7.7 http.Handler 接口

_本章将进一步讨论 http.Handler 接口._

```go
package http
type Handler interface {
ServeHTTP(w ResponseWriter, r *Request)
}
func ListenAndServe(address string, h Handler) error
```

> ListenAndServe 需要一个服务地址, 以及一个 Handler 接口的实例.  
> _假设一个电子商务网站, 使用一个数据库来存储商品和价格的映射._

```go
import (
    "fmt"
    "net/http"
)

type dollars float32

func (d dollars) String() string { return fmt.Sprintf("%.2f", d) }

type database map[string]dollars

func (db database) ServeHTTP(w http.ResponseWriter, req *http.Request) {
    switch req.URL.Path {
    case "/list":
        for item, price := range db {
            fmt.Fprintf(w, "%s: %s\n", item, price)
        }
    case "/price":
        item := req.URL.Query().Get("item")
        price, ok := db[item]
        if !ok {
            w.WriteHeader(http.StatusNotFound) // 404
            fmt.Fprintf(w, "no such item: %q\n", item)
            return
        }
        fmt.Fprintf(w, "%s\n", price)
    default:
        w.WriteHeader(http.StatusNotFound) // 404
        fmt.Fprintf(w, "no such page: %s\n", req.URL)
    }
}
```

> 处理函数基于 URL 的路径部分 (req.URL.Path) 如果处理函数不能识别路径部分来执行哪部分逻辑. 如果处理函数不能识别这个路径, 那么通过调用 w.WriteHeader(http.StatusNotFound) 来返回一个 HTTP 错误. 注意, 这个调用必须在往 w 中写入内容前完成 (http.ResponseWriter 也是一个接口, 它扩充了 io.Writer, 加了发送 HTTP 响应头的方法). 也可以使用 http.Error 这个工具函数来达到相同目的.

```go
msg := fmt.Sprintf("no such page: %s\n", req.URL)
http.Error(w, msg, http.StatusNotFound) // 404
```

> net/http 包提供了一个请求多工转发器 ServeMux, 用来简化 URL 和处理程序之间的关联. 一个 ServeMux 把多个 http.Handler 组合成单个 http.Handler.  
> 下面的代码中, 创建了一个 ServeMux, 用于将 /list, /price 这样的 URL 和对应的处理程序关联起来, 这些处理程序也已经拆分到不同的方法中. 最后作为主程序在 ListenAndServe 调用中使用这个 ServerMux:

```go
func main() {
    db := database{"shoes": 50, "socks": 5}
    http.HandleFunc("/list", db.list)
    http.HandleFunc("/price", db.price)
    log.Fatal(http.ListenAndServe("localhost:8000", nil))
}
```

#### 7.8 error 接口

_error 只是一个接口类型, 含有一个返回错误信息的方法:_

```go
type error interface {
Error() string
}
```

_构造 error 最简单的方法是调用 errors.New, 它会返回一个包含指定的错误信息的新 error 实例. 完整的 error 包只有如下 4 行代码:_

```go
package errors
func New(text string) error { return &errorString{text} }
type errorString struct { text string }
func (e *errorString) Error() string { return e.text }
```

> 底层的 errorString 类型是一个结构, 而没有直接用字符串, 主要是为了避免将来的布局变更. 满足 error 接口的是 \*errorString 指针, 而不是原始的 errorString, 主要是为了让每次 New 分配的 error 实例都互不相等.

```go
fmt.Println(errors.New("EOF") == errors.New("EOF")) // "false"
```

> 直接调用 errors.New 比较罕见, 因为有一个更易用的封装函数 fmt.Errorf, 它额外提供了字符串格式化功能.

```go
package fmt
import "errors"
func Errorf(format string, args ...interface{}) error {
return errors.New(Sprintf(format, args...))
}
```

> syscall 包提供了 Go 语言的底层系统调用 API. 在很多平台上, 它也定义了一个满足 error 接口的数字类型 Errno. 在 UNIX 平台上, Errno 的 Error 方法会从一个字符串表格中查询错误消息, 如下所示:

```go
package syscall
type Errno uintptr // operating system error code
var errors = [...]string{
    1: "operation not permitted",   // EPERM
    2: "no such file or directory", // ENOENT
    3: "no such process",           // ESRCH
    // ...
}

func (e Errno) Error() string {
    if 0 <= int(e) && int(e) < len(errors) {
        return errors[e]
    }
    return fmt.Sprintf("errno %d", e)
}
```

> 如下语句创建一个接口值, 其中包含值为 2 的 Errno, 这个值代表 POSIX ENOENT 状态:

```go
var err error = syscall.Errno(2)
fmt.Println(err.Error()) // "no such file or directory"
fmt.Println(err) // "no such file or directory"
```

#### 7.9 示例: 表达式求值器

_下面 5 种具体类型代表特定类型的表达式. Var 代表变量应用. literal 代表浮点数常量. unary 和 binary 类型代表一个或者两个操作数的操作符表达式, 而操作数则可以任意的 Expr. call 代表函数调用, 这里限制它的 fn 字段只能是 pow, sin 和 sqrt._

```go
// An Expr is an arithmetic expression.
type Expr interface{}

// A Var identifies a variable, e.g., x.
type Var string

// A literal is a numeric constant, e.g., 3.141.
type literal float64

// A unary represents a unary operator expression, e.g., -x.
type unary struct {
    op rune // one of '+', '-'
    x  Expr
}

// A binary represents a binary operator expression, e.g., x+y.
type binary struct {
    op   rune // one of '+', '-', '*', '/'
    x, y Expr
}

// A call represents a function call expression, e.g., sin(x).
type call struct {
    fn   string // one of "pow", "sin", "sqrt"
    args []Expr
}
```

_要对包含变量的表达式求值, 需要一个上下文 (environment) 来把变量映射到数值:_

```go
type Env map[Var]float64
```

_我们还需要为每种类型的表达式定义一个 Eval 方法来返回表达式的一个给定上下文的值. 既然每个表达式都需要提供这个方法, 那么可以把它加到 Expr 接口中. 这个包只导出了类型 Expr, Env 和 Var. 客户端可以在不接触其他表达式类型的情况下使用这个求值器._

> 下面是具体的 Eval 方法. Var 的 Eval 方法从上下文中查询结果, 如果变量不存在则返回 0. literal 的 Eval 方法则直接返回本身的值.

```go
func (v Var) Eval(env Env) float64 {
return env[v]
}
func (l literal) Eval(_ Env) float64 {
return float64(l)
}
```

> unary 和 binary 的 Eval 方法首先对它们的操作数递归求值, 然后应用 op 操作. 我们不把除以 0 或者无穷大当做错误. 最后, call 方法先对 pow, sin 或者 sqrt 函数的参数求值, 再调用 math 包中对应函数.

```go
func (u unary) Eval(env Env) float64 {
switch u.op {
case '+':
return +u.x.Eval(env)
case '-':
return -u.x.Eval(env)
}
panic(fmt.Sprintf("unsupported unary operator: %q", u.op))
}
func (b binary) Eval(env Env) float64 {
switch b.op {
case '+':
return b.x.Eval(env) + b.y.Eval(env)
case '-':
return b.x.Eval(env) - b.y.Eval(env)
case '*':
return b.x.Eval(env) * b.y.Eval(env)
case '/':
return b.x.Eval(env) / b.y.Eval(env)
}
panic(fmt.Sprintf("unsupported binary operator: %q", b.op))
}
func (c call) Eval(env Env) float64 {
switch c.fn {
case "pow":
return math.Pow(c.args[0].Eval(env), c.args[1].Eval(env))
case "sin":
return math.Sin(c.args[0].Eval(env))
case "sqrt":
return math.Sqrt(c.args[0].Eval(env))
}
panic(fmt.Sprintf("unsupported function call: %s", c.fn))
}
```

> 我们给 Expr 方法加上另外一个方法. Check 方法用于在表达式语法树上检查静态错误.

```go
type Expr interface {
Eval(env Env) float64
// Check reports errors in this Expr and adds its Vars to the set.
Check(vars map[Var]bool) error
}
```

> 具体的 Check 如下所示. literal 和 Var 的求值不可能出错, 所以 Check 方法返回 nil. unary 和 binary 的方法首先检查操作符是否合法, 再递归检查操作数. 类似地, call 地方法首先检查函数是否是已知地, 然后检查参数个数是否正确, 最后递归检查每个参数.

```go
func (v Var) Check(vars map[Var]bool) error {
vars[v] = true
return nil
}
func (literal) Check(vars map[Var]bool) error {
return nil
}
func (u unary) Check(vars map[Var]bool) error {
if !strings.ContainsRune("+-", u.op) {
return fmt.Errorf("unexpected unary op %q", u.op)
}
return u.x.Check(vars)
}
func (b binary) Check(vars map[Var]bool) error {
if !strings.ContainsRune("+-*/", b.op) {
return fmt.Errorf("unexpected binary op %q", b.op)
}
if err := b.x.Check(vars); err != nil {
return err
}
return b.y.Check(vars)
}
func (c call) Check(vars map[Var]bool) error {
arity, ok := numParams[c.fn]
if !ok {
return fmt.Errorf("unknown function %q", c.fn)
}
if len(c.args) != arity {
return fmt.Errorf("call to %s has %d args, want %d",
c.fn, len(c.args), arity)
}
for _, arg := range c.args {
if err := arg.Check(vars); err != nil {
return err
}
}
return nil
}
var numParams = map[string]int{"pow": 2, "sin": 1, "sqrt": 1}
```

> Check 地输入参数是一个 Var 集合, 它收集在表达式中发现地变量名. 要让表达式能成功求值, 上下文必须包含所有地这些变量. 从逻辑上来讲, 这个集合应当是 Check 的输出结果而不是输入参数, 但因为这个方法是递归调用, 在这种情况下使用参数更加方便. 调用方在最初调用时需要提供一个空的集合.  
> 可以构建一个 Web 应用, 在运行时从客户端接收一个表达式, 并绘制函数曲面图.

```go
func parseAndCheck(s string) (eval.Expr, error) {
    if s == "" {
        return nil, fmt.Errorf("empty expression")
    }
    expr, err := eval.Parse(s)
    if err != nil {
        return nil, err
    }
    vars := make(map[eval.Var]bool)
    if err := expr.Check(vars); err != nil {
        return nil, err
    }
    for v := range vars {
        if v != "x" && v != "y" && v != "r" {
            return nil, fmt.Errorf("undefined variable: %s", v)
        }
    }
    return expr, nil
}
```

> 要构造这个 Web 应用, 仅需要增加下面的 plot 函数, 其函数签名与 http.HandlerFunc 类似:

```go
func plot(w http.ResponseWriter, r *http.Request) {
r.ParseForm()
expr, err := parseAndCheck(r.Form.Get("expr"))
if err != nil {
http.Error(w, "bad expr: "+err.Error(), http.StatusBadRequest)
return
}
w.Header().Set("Content-Type", "image/svg+xml")
surface(w, func(x, y float64) float64 {
r := math.Hypot(x, y) // distance from (0,0)
return expr.Eval(eval.Env{"x": x, "y": y, "r": r})
})
}
```

#### 7.10 类型断言

_类型断言是一个作用在接口值上的操作, 写出来类似于 x.(T), 其中 x 是一个接口类型表达式, 而 T 是一个类型 (称为断言类型). 类型断言会检查作为操作数的动态类型是否满足指定的断言类型._

> 这儿有两个可能. 首先, 如果断言类型 T 是一个具体类型, 那么类型断言会检查 x 的动态类型是否就是 T. 如果检查成功, 类型断言的结果就是 x 的动态值, 类型当然就是 T. 换句话说, 类型断言就是用来从它的操作数中把具体类型的值取出来的操作. 如果检查失败, 那么操作崩溃. 比如:

```go
var w io.Writer
w=os.Stdout
f := w.(*os.File) // success: f == os.Stdout
c := w.(*bytes.Buffer) // panic: interface holds *os.File, not *bytes.Buffer
```

> 其次, 如果断言类型 T 是一个接口类型, 那么类型断言检查 x 的动态类型是否满足 T. 如果检查成功, 动态值并没有提取出来, 结果仍然是一个接口值, 接口值的类型和值部分也没有变更, 只是结果类型为接口类型 T. 换句话说, 类型断言就是一个接口值表达式, 从一个接口类型变为拥有另外一套方法的接口类型, 但保留了接口值中的动态类型和动态值部分.

```go
var w io.Writer
w=os.Stdout
rw := w.(io.ReadWriter) // success: *os.File has both Read and Write
w=new(ByteCounter)
rw = w.(io.ReadWriter) // panic: *ByteCounter has no Read method
```

> 无论哪种类型作为断言类型, 如果操作数是一个空接口值, 类型断言都失败. 很少需要从一个接口类型向一个要求更加宽松的类型断言. 该宽松类型的接口方法比原类型的少, 而且是其子集. 因为除了在操作 nil 之外的情况下, 在其他情况下这种操作与赋值一致.  
> 如下类型断言代码中, w 和 rw 都持有 os.Stdout, 于是所有对应的动态类型都是 \*os.File, 但 w 作为 io.Writer 仅暴露了文件的 Write 方法, 而 rw 还暴露了它的 Read 方法.

```go
w=rw // io.ReadWriter is assignable to io.Writer
w=rw.(io.Writer) // fails only if rw == nil
```

> 我们经常无法确定一个接口值的动态类型, 这时候需要检测它是否是某一个特定类型. 如果类型断言出现在需要两个结果的赋值表达式 (比如如下代码) 中, 那么断言不会在失败时崩溃, 而是会多返回一个布尔型的返回值来表示断言是否成功.

```go
var w io.Writer = os.Stdout
f, ok := w.(*os.File) // success: ok, f == os.Stdout
b, ok := w.(*bytes.Buffer) // failure: !ok, b == nil
```

> 按照惯例, 一般把第二个返回值赋给一个名为 ok 的变量. 如果操作失败, ok 为 false, 而第一个返回值为断言类型的零值, 在这个例子中就是 \*bytes.Buffer 的空指针.  
> ok 返回值通常马上就用来决定下一步做什么. 下面 if 表达式的扩展形式就可以写出以下代码:

```go
if f, ok := w.(*os.File); ok {
// ...use f...
}
```

> 但类型断言操作数是一个变量时, 有时候会看到返回值的名字与操作数变量名一致, 原有的值就被新的返回值覆盖了, 比如:

```go
if w, ok := w.(*os.File); ok {
// ...use w...
}
```

#### 7.11 使用类型断言来识别错误

_考虑一下 os 包中的文件操作返回的错误集合, I/O 会因为很多原因失败, 但有三类原因通常必须单独处理: 文件已存储 (创建操作), 文件没找到 (读取操作) 以及权限不足. os 包提供了三个帮助函数用来对错误进行分类:_

```go
package os
func IsExist(err error) bool
func IsNotExist(err error) bool
func IsPermission(err error) bool
```

> 一般用专门的类型来表示结构化的错误值. os 包定义了一个 PathError 类型来表示在一个文件路径相关操作上发生错误 (比如 Open 或者 Delete), 一个类似的 LinkError 用来表达在与两个文件路径相关的操作上发生错误 (比如 Symlink 和 Rename). 下面是 os.PathError 的定义:

```go
package os
// PathError records an error and the operation and file path that caused it.
type PathError struct {
Op string
Path string
Err error
}
func (e *PathError) Error() string {
return e.Op + " " + e.Path + ": " + e.Err.Error()
}
```

> 很多客户忽略了 PathError, 改用一种统一的方法来处理所有错误, 即调用 Error 方法. PathError 的 Error 方法只是拼接了所有的字段. 而 PathError 的结构则保留了错误还有的底层信息. 对于那些需要区分错误的客户端, 可以使用断言来检查错误的特定类型, 这些类型包含的细节远远多于一个简单的字符串.

```go
import (
"errors"
"syscall"
)
var ErrNotExist = errors.New("file does not exist")
// IsNotExist returns a boolean indicating whether the error is known to
// report that a file or directory does not exist. It is satisfied by
// ErrNotExist as well as some syscall errors.
func IsNotExist(err error) bool {
if pe, ok := err.(*PathError); ok {
err = pe.Err
}
return err == syscall.ENOENT || err == ErrNotExist
}
_, err := os.Open("/no/such/file")
fmt.Println(os.IsNotExist(err)) // "true"
```

##### 7.12 通过接口类型断言来查询特性

_下面这段代码的逻辑类似于 net/http 包中的 Web 服务器向客户端响应诸如 "Content-type:text/html" 这样的 HTTP 头字段. io.Writer w 代表 HTTP 响应, 写入的字节最终会发到某人的 Web 浏览器上._

```go
func writeHeader(w io.Writer, contentType string) error {
if _, err := w.Write([]byte("Content-Type: ")); err != nil {
return err
}
if _, err := w.Write([]byte(contentType)); err != nil {
return err
}
// ...
}
```

> 因为 write 方法需要一个字节 slice, 而我们想写入的是一个字符串, 所以 []byte(...) 转换就是必需的. 这种转换需要进行内存分配和内存复制, 但复制后内存又会马上抛弃. 性能分析表明这内存分配导致性能下降.  
> 从 io.Writer 接口我们仅仅知道 w 中具体类型的一个信息, 那就是可写入字节 slice. 但如果深入 net/http 包查看, 可以看到 w 对应的动态类型还支持一个能高效写入字符串的 writeString 方法, 这个方法避免了临时内存分配和复制.  
> 我们无法假定任意一个 io.Writer w 也有 WriteString 方法. 但可以定义一个新的接口, 这个接口只包含 WriteString 方法, 然后所有类型断言来判断 w 的动态类型是否满足这个新接口.

```go
// writeString writes s to w.
// If w has a WriteString method, it is invoked instead of w.Write.
func writeString(w io.Writer, s string) (n int, err error) {
type stringWriter interface {
WriteString(string) (n int, err error)
}
if sw, ok := w.(stringWriter); ok {
return sw.WriteString(s) // avoid a copy
}
return w.Write([]byte(s)) // allocate temporary copy
}
func writeHeader(w io.Writer, contentType string) error {
if _, err := writeString(w, "Content-Type: "); err != nil {
return err
}
if _, err := writeString(w, contentType); err != nil {
return err
}
// ...
}
```

> 一个具体类型是否满足 stringWriter 接口仅仅由它拥有的方法来决定, 而不是这个类型与一个接口类型之间的一个声明关系. writeString 函数使用类型断言来判定一个更普适接口类型的值是否满足更专用的接口类型, 如果满足, 则可以使用后者所定义的方法.

```go
package fmt
func formatOneValue(x interface{}) string {
if err, ok := x.(error); ok {
return err.Error()
}
if str, ok := x.(Stringer); ok {
return str.String()
}
// ...all other types...
}
```

> 如果 x 满足这两种接口中的一个， 就直接确定格式化方法。 如果不满足， 默认处理部分大致会使用反射处理所有其他类型。

#### 7.13 分支类型

_接口有两种不同风格。_

> 第一种风格， 接口上各种方法突出了满足这个接口的具体类型之间的相似性， 但隐藏了各个具体类型的布局和各自特有的功能。 这种风格强调了方法， 而不是具体类型。  
> 第二种风格则充分利用了接口值能够容纳各种具体类型的能力， 它把接口作为这些类型的联合 （union）来使用。 类型断言用来在运行时区分这些类型并分别处理。 在这种风格中，强调的是满足这个接口的具体类型， 而不是整个接口的方法， 也不注重信息隐藏。 我们把这种风格的接口使用方式称为可识别联合 (discriminated union)。  
> 这两种风格分别对应子类型多态 (subtype polymorphism) 和特设多态 (ad hoc polymorphism)。

_与其他语言一样, Go 语言的数据库 SQL 　查询　 API 　也允许我们干净的分离查询中的不变部分和可变部分．　一个示例客户端如下所示：_

```go
import "database/sql"
func listTracks(db sql.DB, artist string, minYear, maxYear int) {
result, err := db.Exec(
"SELECT * FROM tracks WHERE artist = &#63; AND &#63; <= year AND year <= &#63;",
artist, minYear, maxYear)
// ...
}
```

> Exec 方法把查询字符串中的每一个 "&#63;" 都替换为相应参数的值对应的 SQL 字面量, 这些参数可能是布尔型, 数字, 字符串或者 nil. 通过这种方式来构造请求可以帮助避免 SQL 注入式攻击, 攻击者可以通过在输入数据中加入不恰当的引号来控制你的查询.

```go
func sqlQuote(x interface{}) string {
if x == nil {
return "NULL"
} else if _, ok := x.(int); ok {
return fmt.Sprintf("%d", x)
} else if _, ok := x.(uint); ok {
return fmt.Sprintf("%d", x)
} else if b, ok := x.(bool); ok {
if b {
return "TRUE"
}
return "FALSE"
} else if s, ok := x.(string); ok {
return sqlQuoteString(s) // (not shown)
} else {
panic(fmt.Sprintf("unexpected type %T: %v", x, x))
} }
```

> 一个相似的类型分支 (type switch) 语句则可以用来简化类型断言 if-else 语句.  
> 类型分支的简单形式与普通分支语句类似, 两个的差别是操作数改为 x.(type), 每个分支是一个或多个类型. 类型分支的分支判定基于接口值的动态类型, 其中 nil 分支需要 x == nil, 而 default 分支则在其他分支都没有满足时才运行.

```go
switch x.(type) {
case nil: // ...
case int, uint: // ...
case bool: // ...
case string: // ...
default: // ...
}
```

> 与普通 switch 语句类似， 分支是按顺序来判定的， 当一个分支符合时, 对应的代码会执行. default 分支的位置无关紧要. 类型分支不允许使用 fallthrough.
> 类型分支语句有一种扩展形式, 它用来把每个分支中提取出来的原始值绑定到一个新的变量:

```go
switch x := x.(type) { /* ... */ }
```

> 与 switch 语句类似, 类型分支也隐式创建了一个词法块, 所以声明一个新变量叫 x 并不与外部块中的变量 x 冲突. 每个分支也会隐式创建各自的词法块.

```go
func sqlQuote(x interface{}) string {
switch x := x.(type) {
case nil:
return "NULL"
case int, uint:
return fmt.Sprintf("%d", x) // x has type interface{} here.
case bool:
if x {
return "TRUE"
}
return "FALSE"
case string:
return sqlQuoteString(x) // (not shown)
default:
panic(fmt.Sprintf("unexpected type %T: %v", x, x))
} }
```

#### 7.15 示例: 基于标记的 XML 解析

*encoding/xml 为解析 API 提供了一个基于标记的底层 XML. 在这些 API 中, 解析器读入输入文本, 然后输出一个标记流. 标记流中主要包含四种类型: StartElement, EndElement, CharData 和 Comment, 这四种类型都是 encoding/xml 包中的一个具体类型. 每次调用 (*xml.Decoder).Token 都会返回一个标记.\*

```go
package xml
type Name struct {
Local string // e.g., "Title" or "id"
}
type Attr struct { // e.g., name="value"
Name Name
Value string
}
// A Token includes StartElement, EndElement, CharData,
// and Comment, plus a few esoteric types (not shown).
type Token interface{}
type StartElement struct { // e.g., <name>
Name Name
Attr []Attr
}
type EndElement struct { Name Name } // e.g., </name>
type CharData []byte // e.g., <p>CharData</p>
type Comment []byte // e.g.,
type Decoder struct{ /* ... */ }
func NewDecoder(io.Reader) *Decoder
func (*Decoder) Token() (Token, error) // returns next Token in sequence
```

> Token 的接口没有任何方法, 这也是一个可识别联合的典型示例. 一个传统的接口 (比如 io.Reader) 的目标时隐藏具体类型的细节, 这样可以轻松创建满足接口的新类型, 对于每一种实现, 使用方的处理方式都是一样的. 可识别联合类型接口正好与之相反, 它的实现类型是固定的而不是随意增加的, 实现类型是暴露的而不是隐藏的. 可识别联合类型很少有方法, 操作它的函数经常会使用类型 switch, 然后对每种类型应用不同的逻辑.  
> 下面的 xmlselect 程序提取并输出 XML 文档中特定元素下的文本.

```go
// Xmlselect prints the text of selected elements of an XML document.
package main
import (
"encoding/xml"
"fmt"
"io"
"os"
"strings"
)
func main() {
    dec := xml.NewDecoder(os.Stdin)
    var stack []string // stack of element names
    for {
        tok, err := dec.Token()
        if err == io.EOF {
            break
        } else if err != nil {
            fmt.Fprintf(os.Stderr, "xmlselect: %v\n", err)
            os.Exit(1)
        }
        switch tok := tok.(type) {
        case xml.StartElement:
            stack = append(stack, tok.Name.Local) // push
        case xml.EndElement:
            stack = stack[:len(stack)-1] // pop
        case xml.CharData:
            if containsAll(stack, os.Args[1:]) {
                fmt.Printf("%s: %s\n", strings.Join(stack, " "), tok)
            }
        }
    }
}

// containsAll reports whether x contains the elements of y, in order.
func containsAll(x, y []string) bool {
    for len(y) <= len(x) {
        if len(y) == 0 {
            return true
        }
        if x[0] == y[0] {
            y = y[1:]
        }
        x = x[1:]
    }
    return false
}

```

#### 7.15 一些建议

_当设计一个新包时, 一个新手 Go 程序员会首先创建一系列接口, 然后定义满足这些接口的具体类型. 这种方式会产生很多接口, 当这些接口只有一个单独的实现. 不要这样做. 这些接口是不必要的抽象, 还有运行时的成本. 可以用导出机制来限制一个类型的哪些方法或结构体的哪些字段是对包外可见的. 仅在有两个或者多个具体类型需要按统一方式处理时才需要接口._  
_这个规则也有特例, 如果接口和类型实现出于依赖的原因不能放在同一个包里面, 那么一个接口只有一个具体类型实现也是可以的. 在这种情况下, 接口时一种解耦两个包的好方式._  
_因为接口仅在有两个或者多个类型满足的情况下存在, 所以它必然会抽象掉那些特有的实现细节. 这种设计的结果就是出现了具有更简单和更少方法的接口, 比如 io.Writer 和 fmt.Stringer 都只有一个方法. 设计新类型时越小的接口越容易满足. 一个不错的接口设计经验是仅要求你需要的_

### 第 8 章 goroutine 和通道

_Go 有两种并发编程风格. 这一章展示 goroutine 和通道 (channel), 它们支持通信顺序进程 (Communicating Sequential Process, CSP), CSP 是一个并发模式, 在不同的执行体 (goroutine) 之间传递值, 但是变量本身局限于单一执行体._

#### 8.1 goroutine

_在 Go 里, 每一个并发执行的活动称为 goroutine._  
_当一个程序启动时, 只有一个 goroutine 来调用 main 函数, 称它为主 goroutine. 新的 goroutine 通过 go 语句进行创建. 语法上, 一个 go 语句是在普通的函数或者方法调用前加上 go 关键字前缀. go 语句使函数在一个新创建的 goroutine 中调用. go 语句本身的执行立即完成:_

```go
f() // call f(); wait for it to return
go f() // create a new goroutine that calls f(); don't wait
```

_下面的例子中, 主 goroutine 计算第 45 个斐波那契数._

```go
func main() {
go spinner(100 * time.Millisecond)
const n = 45
fibN := fib(n) // slow
fmt.Printf("\rFibonacci(%d) = %d\n", n, fibN)
}
func spinner(delay time.Duration) {
for {
for _, r := range `-\|/` {
fmt.Printf("\r%c", r)
time.Sleep(delay)
} } }
func fib(x int) int {
if x < 2 {
return x
}
return fib(x-1) + fib(x-2)
}
```

_若干秒后, fib(45) 返回, main 函数输出结果:_

```go
Fibonacci(45) = 1134903170
```

_然后 main 函数返回, 当它发生时, 所有的 goroutine 都暴力地直接终结, 然后程序退出. 除了从 main 返回或者退出程序之外, 没有程序化的方法让一个 goroutine 来停止另一个, 但有方法和 goroutine 通信来要求它自己停止._

#### 8.2 示例: 并发时钟服务器

_第一个例子是顺序时钟服务器, 它以每秒一次的频率向客户端发送当前时间:_

```go
// Clock1 is a TCP server that periodically writes the time.
package main

import (
    "io"
    "log"
    "net"
    "time"
)

func main() {
    listener, err := net.Listen("tcp", "localhost:8000")
    if err != nil {
        log.Fatal(err)
    }
    for {
        conn, err := listener.Accept()
        if err != nil {
            log.Print(err) // e.g., connection aborted
            continue
        }
        handleConn(conn) // handle one connection at a time
    }
}
func handleConn(c net.Conn) {
    defer c.Close()
    for {
        _, err := io.WriteString(c, time.Now().Format("15:04:05\n"))
        if err != nil {
            return // e.g., client disconnected
        }
        time.Sleep(1 * time.Second)
    }
}
```

> Listen 函数创建一个 net.Listener 对象, 它在一个网络端口上监听进来的连接, 这里是 TCP 端口 localhost:8000. 监听器的 Accept 方法被阻塞, 直到有连接请求进来, 然后返回 net.Conn 对象来代表一个连接.  
> handleConn 函数处理一个完整的客户端连接. 在循环里, 它将 time.Now() 获取的当前时间发送给客户端. 因为 net.Conn 满足 io.Writer 接口, 所以可以直接向它进行写入. 当写入失败时循环结束, 很多时候是客户端断开链接, 这时 handleConn 函数使用延迟的 Close 调用关闭自己这边的连接, 然后继续等待下一个连接请求.  
> time.Time.Format 方法提供了格式化日期和时间的方式. 它的参数是一个模板, 指示如何格式化一个参考时间.  
> 为了连接到服务器, 需要一个像 nc ("netcat") 这样的程序, 以及一个用来操作网络连接的标准工具:

```go
 go build gopl.io/ch8/clock1
 ./clock1 &
 nc localhost 8000
13:58:54
13:58:55
13:58:56
13:58:57
^C
```

> 如果系统上没有安装 nc 或 netcat, 可以使用 telnet 或者一个使用 net.Dial 实现的 Go 版的 netcat 来连接 TCP 服务器:

```go
// Netcat1 is a read-only TCP client.
package main
import (
"io"
"log"
"net"
"os"
)
func main() {
conn, err := net.Dial("tcp", "localhost:8000")
if err != nil {
log.Fatal(err)
}
defer conn.Close()
mustCopy(os.Stdout, conn)
}
func mustCopy(dst io.Writer, src io.Reader) {
if _, err := io.Copy(dst, src); err != nil {
log.Fatal(err)
} }
```

> 这个程序从网络连接中读取, 然后写到标准输出, 直到到达 EOF 或者出错. mustCopy 函数是这一节的多个例子中使用的一个实用程序. 在不同的终端上同时运行两个客户端, 一个显示左边, 一个在右边:

```go
 go build gopl.io/ch8/netcat1
 ./netcat1
13:58:54  ./netcat1
13:58:55
13:58:56
^C
13:58:57
13:58:58
13:58:59
^C
 killall clock1
```

> killall 命令是 UNIX 的一个实用程序, 用来终止所有指定名字的进程.  
> 第二个客户端必须等到第一个结束才能正常工作, 因为服务器是顺序的, 一次只能处理一个客户请求. 让服务器支持并发只需要一个很小的改变: 在调用 handleConn 的地方添加一个 go 关键字, 使他在自己的 goroutine 内执行.

```go
gopl.io/ch8/clock2
for {
conn, err := listener.Accept()
if err != nil {
log.Print(err) // e.g., connection aborted
continue
}
go handleConn(conn) // handle connections concurrently
}
```

#### 8.3 示例: 并发回声服务器

_时钟服务器每个连接使用一个 goroutine. 在这一节, 我们要构建一个回声服务器, 每个连接使用多个 goroutine 来处理. 大多数的回声服务器仅仅将读到的内容写回去, 它可以使用下面简单的 handleConn 版本完成:_

```go
func handleConn(c net.Conn) {
io.Copy(c, c) // NOTE: ignoring errors
c.Close()
}
```

> 回声服务器可以模仿真实的回声, 第一次大的回声 ("HELLO!"), 在一定延迟后中等音量的回声 (Hello!), 然后安静的回声 ("hello!"), 最后什么都没有, 如下面这个版本的 handleConn 所示:

```go
func echo(c net.Conn, shout string, delay time.Duration) {
fmt.Fprintln(c, "\t", strings.ToUpper(shout))
time.Sleep(delay)
fmt.Fprintln(c, "\t", shout)
time.Sleep(delay)
fmt.Fprintln(c, "\t", strings.ToLower(shout))
}
func handleConn(c net.Conn) {
input := bufio.NewScanner(c)
for input.Scan() {
echo(c, input.Text(), 1*time.Second)
}
// NOTE: ignoring potential errors from input.Err()
c.Close()
}
```

> 我们需要升级客户端程序, 使它可以在终端上向服务器输入, 还可以将服务器的回复复制到输出, 这里提供了另一个使用并发的机会:

```go
func main() {
conn, err := net.Dial("tcp", "localhost:8000")
if err != nil {
log.Fatal(err)
}
defer conn.Close()
go mustCopy(os.Stdout, conn)
mustCopy(conn, os.Stdin)
}
```

> 当主 goroutine 从标准输入读取并发送到服务器的时候, 第二个 goroutine 读取服务器的回复且输出. 真实的回声会用三个独立的呼喊回声叠加组成. 为了模仿它, 我们需要更多的 goroutine. 再一次, 在调用 echo 时加入 go 关键字:

```go
func handleConn(c net.Conn) {
input := bufio.NewScanner(c)
for input.Scan() {
go echo(c, input.Text(), 1*time.Second)
}
// NOTE: ignoring potential errors from input.Err()
c.Close()
}
```

> 在添加这些 go 关键字的同时, 必须要仔细考虑方法 net.Conn 的并发调用是不是安全的, 对大多数类型来讲, 这都是不安全的.

#### 8.4 通道

_如果说 goroutine 是 Go 程序并发的执行体, 通道就是它们之间的连接. 通道是可以让一个 goroutine 发送特定值到另一个 goroutine 的通信机制. 每一个通道是一个具体类型的导管, 叫作通道的元素类型. 一个有 int 类型元素的通道写为 chan int._
_使用内置的 make 函数来创建一个通道:_

```go
ch := make(chan int) // ch has type 'chan int'
```

> 像 map 一样, 通道是一个使用 make 创建的数据结构的引用. 当复制或者作为参数传递到一个函数时, 复制的是引用, 这样调用者都引用同一份数据结构. 和其他引用类型一样, 通道的零值是 nil.  
> 同种类型的通道可以使用 == 符号进行比较. 当二者都是同一数据的引用时, 比较值为 true. 通道也可以和 nil 进行比较.  
> 通道有两个主要操作: 发送 (send) 和接收 (receive), 两者统称为通信. send 语句从一个 goroutine 传输一个值到另一个在执行接表达式的 goroutine. 两个操作都使用 <- 操作符书写. 发送语句中, 通道和值分别在 <- 的左右两边. 在接收表达式中, <- 放在通道操作数的前面. 在接收表达式中, 其结果未被使用也是合法的.

```go
ch <- x // a send statement
x=<-ch // a receive expression in an assignment statement
<-ch // a receive statement; result is discarded
```

> 通道支持的第三个操作: 关闭 (close), 它设置一个标志位来指示当前已经发送完毕, 这个通道后面就没有值了; 关闭后的发送操作将导致宕机. 在一个关闭的通道上进行接收操作, 将获取所有已经发送的值, 直到通道值为空; 这时任何接收操作会立即完成, 同时获取到一个通道元素类型对应的零值. 调用内置的 close 函数来关闭通道:

```go
close(ch)
```

> 使用简单的 make 调用创建的通道叫无缓冲 (unbuffered) 通道, 当 make 还可以接收第二个可选参数, 一个表示通道容量的整数. 如果容量是 0, make 创建一个无缓冲通道:

```go
ch = make(chan int) // unbuffered channel
ch = make(chan int, 0) // unbuffered channel
ch = make(chan int, 3) // buffered channel with capacity 3
```

##### 8.4.1 无缓冲通道

> 无缓冲通道上的发送操作将会阻塞, 直到另一个 goroutine 在对应的通道上执行接收操作, 这时值传送完成, 两个 goroutine 都可以继续执行. 相反, 如果接收操作先执行, 接收方 goroutine 将阻塞, 直到另一个 goroutine 在同一个通道上发送一个值.
> 使用无缓冲通道进行的通信导致发送和接收同步化. 因此, 无缓冲通道也称为同步通道. 当一个值在无缓冲通道上传递时, 接收后发送方 goroutine 才被再次唤醒.  
> 在讨论并发的时候, 当我们说 x 早于 y 发生时, 不仅仅是说 x 发生的时间早于 y, 而是说保证它是这样, 并且是可预期的, 比如更新变量, 我们可以依赖这个机制.  
> 当 x 既不比 y 早也不比 y 晚时, 我们说 x 和 y 并发. 这不意味着 x 和 y 一定同时发生, 只说明我们不能假设它们的顺序. 在两个 goroutine 并发同时访问一个变量的时候, 有必要对这样的事件进行排序, 避免程序的执行发生问题.

```go
func main() {
conn, err := net.Dial("tcp", "localhost:8000")
if err != nil {
log.Fatal(err)
}
done := make(chan struct{})
go func() {
io.Copy(os.Stdout, conn) // NOTE: ignoring errors
log.Println("done")
done <- struct{}{} // signal the main goroutine
}()
mustCopy(conn, os.Stdin)
conn.Close()
<-done // wait for background goroutine to finish
}
```

> 当用户关闭标准输出流时, mustCopy 返回, 主 goroutine 调用 conn.Close() 来关闭两端网络连接. 关闭写半边的连接会导致服务器看到 EOF. 关闭读半边的连接会导致后台 goroutine 调用 io.Copy 返回 "read from closed connection" 错误.  
> 在它返回前, 后台 goroutine 记来一条消息, 然后发送一个值到 done 通道. 主 goroutine 在退出前一直等待, 直到它接收到这个值. 最终效果是程序总是在退出前记录 "done" 消息.  
> 通过通道发送消息有两个重要方面需要考虑. 每一条消息有一个值, 但有时候通信本身以及通信发生的时间也很重要. 当我们强调这方面的时候, 把消息叫作事件 (event). 当事件没有携带额外消息时, 它单纯的目的是进行同步. 我们通过使用一个 struct{} 元素类型的通道来强调它, 尽管通常使用 bool 或 int 类型的通道来做相同的事情, 因为 done <- -1 比 done <- struct{}{} 要短.

##### 8.4.2 管道

通道可以用来连接 goroutine, 这样一个的输出是另一个的输入. 这个叫管道 (pipeline).

```go
func main() {
naturals := make(chan int)
squares := make(chan int)
// Counter
go func() {
for x := 0; ; x++ {
naturals <- x
}
}()
// Squarer
go func() {
for {
x := <-naturals
squares <- x * x
}
}()
// Printer (in main goroutine)
for {
fmt.Println(<-squares)
} }
```

> 正如所期望的那样, 程序输出无限的平方序列 0, 1, 4, 9, .... 像这样的管道出现在长期运行的服务器程序中, 其中通道用于在包含无限循环的 goroutine 之间整个生命周期中的通信.  
> 如果发送方知道没有更多数据要发送, 告诉接收者所在 goroutine 可以停止等待, 可以通过调用内置的 close 函数来关闭通道.  
> close(naturals)  
> 在通道关闭后, 任何后续的发送操作将会导致应用崩溃. 当关闭的通道被读完后, 所有后续的接收操作顺畅进行, 只是获取到的是零值. 关闭 naturals 通道导致计算平方的循环快速运转, 并将结果 0 传递给 printer goroutine.
> 没有一个直接的方式来判断通道已经关闭, 但是有接收操作的一个变种, 它产生两个结果: 接收到的通道元素, 以及一个布尔值 (通常称为 ok), 它为 true 的时候代表接收成功, false 表示当前的接收操作在一个关闭的并且读完的通道上. 使用这特性, 可以修改 square 的循环, 当 naturals 通道读完后, 关闭 squares 通道.

```go
go func() {
for {
x, ok := <-naturals
if !ok {
break // channel was closed and drained
}
squares <- x * x
}
close(squares)
}()
```

> 因为上面的语法比较笨拙, 而模式又比较通用, 所以该语言也提供了 range 循环语法以在通道上迭代. 这个语法更为方便接收在通道上所发送的值, 接收完成最后一个值后关闭循环.

```go
func main() {
naturals := make(chan int)
squares := make(chan int)
// Counter
go func() {
for x := 0; x < 100; x++ {
naturals <- x
}
close(naturals)
}()
// Squarer
go func() {
for x := range naturals {
squares <- x * x
}
close(squares)
}()
// Printer (in main goroutine)
for x := range squares {
fmt.Println(x)
} }
```

> 结束时, 关闭每一个通道不是必需的. 只有在通知接收方 goroutine 所有的数据都发送完毕的时候才需要关闭通道. 通道也是可以通过垃圾回收器根据它是否可以访问来决定是否回收它, 而不是根据它是否关闭. (不要将这个 close 操作和对于文件的 close 操作混淆. 当结束的时候对每一个文件调用 Close 方法是非常重要的.)  
> 试图关闭一个已经关闭的通道会导致宕机, 就像关闭一个空通道一样. 关闭通道还可以作为一个广播机制.

##### 8.4.3 单项通道类型

_当程序演进时, 将大的函数拆分成多个更小的是很自然的. 上一个例子使用了三个 goroutine, 两个通道来通信, 它们都是 main 的局部变量. 程序自然划分为三个函数:_

```go
func counter(out chan int)
func squarer(out, in chan int)
func printer(in chan int)
```

> squarer 函数位于管道的中间, 使用两个参数: 输入通道和输出通道. 它们有相同的类型, 但是用途是相反的: in 仅仅用来接收, out 仅仅用来发送, in 和 out 两个名字是特意使用的, 但是没有什么东西阻碍 squarer 函数通过 in 来发送或者通过 out 来接收.  
> 这是一个典型的安排, 当一个通道用做函数形参时, 它几乎总是被有意地限制不能发送或不能接收.  
> 将这种意图文档化可以避免误用, Go 地类型系统提供了单向通道类型, 仅仅导出发送或接收操作. 类型 chan<- int 是一个只能发送地通道, 允许发送但不允许接收. 反之, 类型 <-chan int 是一个只能接收的 int 类型通道, 允许接收但是不允许发送. 违反这个原则会在编译时被检查出来.  
> 因为 close 操作说明了通道上没有数据再发送, 仅仅在发送方 goroutine 上才能调用它, 所以试图关闭一个仅能接收的通道在编译时会报错.

```go
func counter(out chan<- int) {
for x := 0; x < 100; x++ {
out <- x
}
close(out)
}
func squarer(out chan<- int, in <-chan int) {
for v := range in {
out <- v * v
}
close(out)
}
func printer(in <-chan int) {
for v := range in {
fmt.Println(v)
} }
func main() {
naturals := make(chan int)
squares := make(chan int)
go counter(naturals)
go squarer(squares, naturals)
printer(squares)
}
```

> counter(naturals) 的调用隐式将 chan int 类型转化为参数要求的 chan<- int 类型. 调用 printer(squares) 做了类似 <-chan int 的转变. 在任何赋值操作中将双向通道转换为单向通道都是允许的, 但是反过来是不行的, 一旦有一个像 chan<- int 这样的单项通道, 是没有办法通过它获取到引用同一数据结构的 chan int 数据类型.

##### 8.4.4 缓冲通道

_缓冲通道有一个元素队列, 队列的最大长度在创建的时候通过 make 的容量来设置._

```go
ch = make(chan string, 3)
```

> 缓冲通道上的发送操作在队列的尾部插入一个元素, 接从收队列的头部移除一个元素. 如果通道满了, 发送操作会阻塞所在的 goroutine 直到另一个 goroutine 对它进行接收操作来留出可用空间. 反过来, 如果通道是空的, 执行接收操作的 goroutine 阻塞, 直到另一个 goroutine 在通道上发送数据.  
> 可以在当前通道上无阻塞的发送三个值:

```go
ch <- "A"
ch <- "B"
ch <- "C"
```

> 这时, 通道是满的, 第四个发送语句将会阻塞. 如果接收一个值:

```go
fmt.Println(<-ch) // "A"
```

> 通道既不满也不空, 所以这时一个接收或发送操作都不会阻塞. 通过这个方式, 通道的缓冲区将发送和接收 goroutine 进行解耦.  
> 不太常见的一个情况是, 程序需要知道通道缓冲区容量, 可以通过调用内置的 cap 函数获取它:

```go
fmt.Println(cap(ch)) // "3"
```

> 当使用内置的 len 函数时, 可以获取当前通道内的元素个数. 因为在并发程序中这个信息会随着检索操作很快过时, 所以它的价值很低, 但是它在错误诊断和性能优化时很有用.

```go
fmt.Println(len(ch)) // "2"
```

> 通过接下去的两次接收操作, 通道又变空, 第四次操作会被阻塞:

```go
fmt.Println(<-ch) // "B"
fmt.Println(<-ch) // "C"
```

> 这个例子中, 发送和接收操作都在同一个 goroutine 执行, 但在真实的程序中通常由不同的 goroutine 执行. 因为语法简单, 新手有时候粗暴地将缓冲通道作为队列在单个 goroutine 中使用, 但是这是个错误. 通道和 goroutine 的调用深度关联, 如果没有另一个 goroutine 从通道进行接收, 发送者 (也许是整个程序) 有被永久阻塞的风险. 如果仅仅需要一个简单的队列, 使用 slice 创建一个就可以.  
> 下面的例子展示一个使用缓冲通道的应用. 它并发地向三个镜像地址发送请求, 镜像指相同分布在不同区域的服务器. 它将它们的响应通过一个缓冲通道进行发送, 然后只接收第一个返回的响应, 因为它是最早到达的. 所以 mirrorQuery 函数甚至在两个比较慢的服务器还没响应之前就返回了一个结果. (偶然情况下, 会出现这个例子中几个 goroutine 同时在一个通道上并发发送, 或者同时从一个通道接收的情况. )

```go
func mirroredQuery() string {
responses := make(chan string, 3)
go func() { responses <- request("asia.gopl.io") }()
go func() { responses <- request("europe.gopl.io") }()
go func() { responses <- request("americas.gopl.io") }()
return <-responses // return the quickest response
}
func request(hostname string) (response string) { /* ... */ }
```

> 如果使用一个无缓冲通道, 两个比较慢的 goroutine 将被卡住, 以为它们发送响应结果到通道的时候没有 goroutine 来接收. 这个情况叫做 goroutine 泄露, 它属于一个 bug. 不像回收变量, 泄露的 goroutine 不会自动回收, 所以确保 goroutine 在不再需要的时候可以自动结束.  
> 无缓冲和缓冲通道的选择, 换乘通道容量大小的选择, 都会对程序的准确性产生影响. 无缓冲通道提供强同步保障, 因为每发送一次都需要一次对应的接收同步; 对于缓冲通道, 这些操作是解耦的. 如果我们知道要发送值的数量上限, 通常会创建一个容量是使用上限的缓冲通道, 在接收第一个值前就完成所有的发送. 在内存无法提供缓冲容量的情况下, 可能导致程序死锁.  
> 通道的缓冲也可能影响程序的性能. 想象蛋糕店的三个厨师, 在生产线上, 在把每一个蛋糕传递给下一个厨师之前, 一个烤, 一个加糖衣, 一个雕刻. 在空间较小的厨房, 每一个厨师完成一个流程, 必须等待下一个厨师准备好接受它; 这个场景类似于使用无缓冲通道来通信.  
> 如果在厨师之间有可以放一个蛋糕的位置, 一个厨师可以将制作好的蛋糕放在这里, 然后立即开始制作下一个, 这类似使用一个容量为 1 的缓冲通道. 只要厨师们以相同的速度工作, 大多数工作可以快速处理, 消除他们各自之间的速率差异. 如果厨师之间有更多的空间 (更长的缓冲区) 就可以消除更大的暂态速率波动而不影响组装流水线, 比如但一个厨师稍作休息时, 后面再抓紧跟上进度.  
> 另一方面, 如果生产线的上游比下游快, 缓冲区满的时间占大多数. 如果后续流程更快, 缓冲区通常是空的, 这时缓冲区的存在是没有价值的.  
> 组装流水线是对于通道和 goroutine 合适的比喻. 例如, 如果第二段更复杂, 一个厨师可能跟不上第一个厨师的供应, 或者跟不上第三个厨师的需求. 为了解决这个问题, 我们可以雇佣另个厨师来帮助第二段流程, 独立执行相同的任务. 这个类似于创建另一个 goroutine 使用同一个通道来通信.

#### 8.5 并行循环

_这一节探讨一些通用的并行模式, 来并行执行所有的循环迭代. 考虑生成一批全尺寸图像的缩略图._

```go
package thumbnail
// ImageFile reads an image from infile and writes
// a thumbnail-size version of it in the same directory.
// It returns the generated file name, e.g., "foo.thumb.jpg".
func ImageFile(infile string) (string, error)

// makeThumbnails makes thumbnails of the specified files.
func makeThumbnails(filenames []string) {
    for _, f := range filenames {
        if _, err := thumbnail.ImageFile(f); err != nil {
            log.Println(err)
        }
    }
}
```

> 很明显, 处理文件的顺序没有关系, 因为每一个缩放操作和其他的操作独立. 像这样由一些完全独立的子问题组成的问题称为高度并行.
> 并行执行这些操作, 忽略文件 I/O 的延迟和同一文件使用多个 CPU 进行图像 slice 计算. 第一个并行版本仅仅添加 go 关键字. 现在忽略错误, 后面再处理:

```go
// NOTE: incorrect!
func makeThumbnails2(filenames []string) {
for _, f := range filenames {
go thumbnail.ImageFile(f) // NOTE: ignoring errors
}
}
```

> 这个版本运行真的太快, 事实上, 即使在文件名称 slice 中只有一个元素的情况下也要比原始版本要快. 如果这里没有并行机制, 并发的版本怎么可能运行的更快? 答案是 makeThumbnails2 在没有完成想要完成的事情之前就返回了. 它启动了所有的 goroutine, 每个文件一个, 但是没有等待它们执行完毕.
> 没有一个直接的访问等待 goroutine 结束, 但是可以修改内层 goroutine, 通过一个共享的通道发送事件向外层 goroutine 报告它的完成. 因为我们知道 len(filename) 内层 goroutine 的确切个数, 所以外层 goroutine 只需要返回前对完成事件进行计数:

```go
// makeThumbnails3 makes thumbnails of the specified files in parallel.
func makeThumbnails3(filenames []string) {
    ch := make(chan struct{})
    for _, f := range filenames {
        go func(f string) {
            thumbnail.ImageFile(f) // NOTE: ignoring errors
            ch <- struct{}{}
        }(f)
    } // Wait for goroutines to complete.
    for range filenames {
        <-ch
    }
}
```

> 注意, 这里作为一个字面量函数的显示参数传递 f, 而不是在 for 循环中声明 f :

```go
for _, f := range filenames {
go func() {
thumbnail.ImageFile(f) // NOTE: incorrect!
// ...
}()
}
```

> 回想 5.6.1 节描述的在匿名函数中获取循环变量的问题. 上面单变量 f 的值被所有的匿名函数共享并且被后续迭代所更新. 这时新的 goroutine 执行字面量函数, for 循环可能已经更新 f, 并且开始另一个迭代或者已经完全结束, 所以当这些 goroutine 读取 f 的值时, 它们所看到的都是 slice 的最后一个元素, 通过添加显示参数, 可以确保当 go 语句执行的时候, 使用 f 的当前值.
> 我们想让每一个工作 goroutine 中向主 goroutine 返回什么&#63; 如果调用 thumbnail.ImageFile 无法创建文件, 它将返回一个错误.

```go
// makeThumbnails4 makes thumbnails for the specified files in parallel.
// It returns an error if any step failed.
func makeThumbnails4(filenames []string) error {
    errors := make(chan error)
    for _, f := range filenames {
        go func(f string) {
            _, err := thumbnail.ImageFile(f)
            errors <- err
        }(f)
    }
    for range filenames {
        if err := <-errors; err != nil {
            return err // NOTE: incorrect: goroutine leak!
        }
    }
    return nil
}
```

> 这个函数有一个微妙的缺陷, 当遇到一个非 nil 错误时, 它将错误返回给调用者, 这样没有 goroutine 继续从 errors 返回通道上进行接收, 直到读完. 每一个现存的工作 goroutine 在试图发送值到此通道的时候将永久阻塞, 永不终止. 这种情况下 goroutine 泄露可能导致整个程序卡住或者内存耗尽.  
> 最简单的方案是使用一个有足够容量的缓冲通道, 这样没有工作 goroutine 在发送消息时候阻塞.(另一个方案是在主 goroutine 返回第一个错误的同时, 创建另一个 goroutine 来读完通道.)  
> 下一版本的 makeThumbnails 使用一个缓冲通道来返回生成的图像文件的名称以及任何错误消息:

```go
// makeThumbnails5 makes thumbnails for the specified files in parallel.
// It returns the generated file names in an arbitrary order,
// or an error if any step failed.
func makeThumbnails5(filenames []string) (thumbfiles []string, err error) {
    type item struct {
        thumbfile string
        err       error
    }
    ch := make(chan item, len(filenames))
    for _, f := range filenames {
        go func(f string) {
            var it item
            it.thumbfile, it.err = thumbnail.ImageFile(f)
            ch <- it
        }(f)
    }
    for range filenames {
        it := <-ch
        if it.err != nil {
            return nil, it.err
        }
        thumbfiles = append(thumbfiles, it.thumbfile)
    }
    return thumbfiles, nil
}

```

> makeThumbnails 的终极版本 (参看下面) 返回新的文件所占用的总字节数. 不想前一个版本, 它不是使用 slice 接收文件名, 而是借助一个字符通道, 这样我们不能预测迭代的次数.  
> 为了知道什么时候最后一个 goroutine 结束 (它不一定最后启动), 需要在每一个 goroutine 启动前递增计数, 在每一个 goroutine 结束时候递减计数. 这需要一个特殊类型的计数器, 它可以被多个 goroutine 安全地操作, 然后有一个方法一直等到它变为 0. 这个计数器类型是 sync.WaitGroup, 下面的代码展示如何使用它:

```go

// makeThumbnails6 makes thumbnails for each file received from the channel.
// It returns the number of bytes occupied by the files it creates.
func makeThumbnails6(filenames <-chan string) int64 {
    sizes := make(chan int64)
    var wg sync.WaitGroup // number of working goroutines
    for f := range filenames {
        wg.Add(1)
        // worker
        go func(f string) {
            defer wg.Done()
            thumb, err := thumbnail.ImageFile(f)
            if err != nil {
                log.Println(err)
                return
            }
            info, _ := os.Stat(thumb) // OK to ignore error
            sizes <- info.Size()
        }(f)
    }
    // closer
    go func() {
        wg.Wait()
        close(sizes)
    }()
    var total int64
    for size := range sizes {
        total += size
    }
    return total
}
```

> 注意 Add 和 Done 方法的不对称性. Add 递增计算器, 它必须在工作 goroutine 开始之前执行, 而不是在中间. 另一方面, 不能保证 Add 会在关闭者 goroutine 调用 Wait 之前发生. 另外, Add 有一个参数, 但 Done 没有, 它等价于 Add(-1). 使用 defer 来确保在发送错误情况的情况下计数器可以递减. 在不知道迭代次数的情况下, 上面的代码结构是通用的, 符合习惯的并行循环模式.  
> sizes 通道将每一个文件的大小带回 goroutine, 它使用 range 循环进行接收然后计算总和. 注意, 在关闭者 goroutine 中, 在关闭 sizes 通道之前, 等待所有工作者结束. 这里两个操作 (等待和关闭) 必须和 sizes 通道上面的迭代并行执行. 考虑替代方案: 如果我们将等待操作放在循环之前的主 goroutine 中, 因为通道会满, 它将永远不结束; 如果放在循环后面, 它将不可达, 因为没有任何东西可用来关闭通道, 循环可能永不结束.

#### 8.6 示例: 并发的 Web 爬虫

_在 5.6 节中, 我们制作了一个简单的网页爬虫, 它以广度优先的顺序来探索网页链接图. 这一节中, 我们使它可以并发运行, 这样对 crawl 的独立调用可以充分利用 Web 上的 I/O 并发机制._

```go
func crawl(url string) []string {
    fmt.Println(url)
    list, err := links.Extract(url)
    if err != nil {
        log.Print(err)
    }
    return list
}
```

> main 函数类似于 breadthFirst. 一个任务列表记录需要处理的条目队列, 每一个条目是一个待爬取的 URL 列表, 这次我们使用通道代替 slice 来表示队列. 每一次对 crawl 的调用发生在它自己的 goroutine 中, 然后将发现的链接发送回任务列表:

```go

func main() {
    worklist := make(chan []string)
    // Start with the command-line arguments.
    go func() { worklist <- os.Args[1:] }()
    // Crawl the web concurrently.
    seen := make(map[string]bool)
    for list := range worklist {
        for _, link := range list {
            if !seen[link] {
                seen[link] = true
                go func(link string) {
                    worklist <- crawl(link)
                }(link)
            }
        }
    }
}
```

> 注意, 该爬取 goroutine 将 link 作为显示参数来使用, 以避免 5.6.1 节看到的循环变量捕获问题. 也要注意, 发送给任务列表的命令行参数必须在自己的 goroutine 中运行来避免死锁, 死锁是一种卡住的情况, 其中主 goroutine 和一个爬取 goroutine 同时发送给对方但是双方都没有接收. 另一个可选方案是使用缓冲通道.  
> 这个爬虫高度并发, 输出巨量的 URL. 但是它有两个问题, 第一个问题是在执行若干秒后, 它自己出现错误日志:

```go
2015/07/15 18:22:12 Get ...: dial tcp: lookup blog.golang.org: no such host
2015/07/15 18:22:12 Get ...: dial tcp 23.21.222.120:443: socket:
too many open files
```

> 第一条消息比较令人意外, 因为它报告的是对一个可靠的域名出现了解析失败. 接下去的错误消息说明程序同时创建了太多的网络连接, 超过了程序能打开文件数的限制, 导致类似于 DNS 查询和 net.Dial 的连接失败.  
> 程序的并行度太高了, 无限制的并行通常不是一个好主意, 因为系统中总有限制因素, 例如, 对于计算机应用 CPU 的核数, 对于磁盘 I/O 操作磁头和磁盘的个数, 下载流所使用的网络带宽, 或者 Web 服务本身的容量. 解决方法是根据资源可用情况限制并发的个数, 以匹配合适的并行度. 该例子中有一个简单的方法是确保对于 links.Extract 的同时调用不超过 n 个, 即比文件描述符符所规定的 20 个少得多. 这种方式类似于一个拥挤夜店的门卫只有在有客人离开的时候才允许其他客人进去.  
> 我们可以使用容量为 n 的缓冲通道来建立一个并发原语, 称为计数信号量. 概念上, 对于缓冲通道中的 n 个空闲槽, 每一个代表一个令牌, 持有者可以执行. 通过发送一个值到通道中来领取令牌, 从通道中接收一个值来释放令牌, 创建一个新的空闲槽. 这保证了在没有接收操作的时候, 最多同时有 n 个发送. (尽管使用已填充槽比令牌更直观, 但使用空闲槽在创建通道缓冲区之后可以省掉填充的过程.) 因为通道元素类型在这里不重要, 所以我们使用 struct{}, 它所占用的空间大小是 0.
> 重写 crawl 函数, 使用令牌的获取和释放操作来包括对 links.Extract 函数的调用, 这样保证最多同时 20 个调用可以进行. 保持信号量操作离它所约束的 I/O 操作越近越好:

```go
// tokens is a counting semaphore used to
// enforce a limit of 20 concurrent requests.
var tokens = make(chan struct{}, 20)

func crawl(url string) []string {
    fmt.Println(url)
    tokens <- struct{}{} // acquire a token
    list, err := links.Extract(url)
    <-tokens // release the token
    if err != nil {
        log.Print(err)
    }
    return list
}

```

> 第二个问题是这个程序永远不会结束, 即使它已经从初始 URL 发现了所有的可达链接. 为了让程序终止, 但任务列表为空且爬取 goroutine 都结束以后, 需要从主循环退出:

```go
func main() {
    worklist := make(chan []string)
    var n int // number of pending sends to worklist
    // Start with the command-line arguments.
    n++
    go func() { worklist <- os.Args[1:] }()
    // Crawl the web concurrently.
    seen := make(map[string]bool)
    for ; n > 0; n-- {
        list := <-worklist
        for _, link := range list {
            if !seen[link] {
                seen[link] = true
                n++
                go func(link string) {
                    worklist <- crawl(link)
                }(link)
            }
        }
    }
}
```

> 这个版本中, 计数器 n 跟踪发送到任务列表中的任务个数. 每次知道一个条目被发送到任务列表时, 就递增变量 n, 第一次递增是在发送初始化命令行参数之前, 第二次递增是在每次启动一个新的爬取 goroutine 的时候. 主循环从 n 减到 0, 这时候再没有任务需要完成.  
> 现在, 并发爬虫的速度大约比 5.6 节中广度优先版本快 20 倍. 在它完成任务的时候, 应该没有错误出现, 并且正确退出.  
> 下面的程序展示一个替代方案, 解决过度并发的问题. 这个版本使用最初的 crawl 函数, 它没有计数信号量, 但是通过 20 个长期存活的爬虫 goroutine 来调用它, 这样确保最多 20 个 HTTP 请求并发执行:

```go
func main() {
    worklist := make(chan []string)  // lists of URLs, may have duplicates
    unseenLinks := make(chan string) // de-duplicated URLs
    // Add command-line arguments to worklist.
    go func() { worklist <- os.Args[1:] }()
    // Create 20 crawler goroutines to fetch each unseen link.
    for i := 0; i < 20; i++ {
        go func() {
            for link := range unseenLinks {
                foundLinks := crawl(link)
                go func() { worklist <- foundLinks }()
            }
        }()
    }
    // The main goroutine de-duplicates worklist items
    // and sends the unseen ones to the crawlers.
    seen := make(map[string]bool)
    for list := range worklist {
        for _, link := range list {
            if !seen[link] {
                seen[link] = true
                unseenLinks <- link
            }
        }
    }
}
```

> 爬取 goroutine 使用同一个通道 unseenLinks 进行接收. 主 goroutine 负责对从任务列表接收到的条目进行去重, 然后发送每一个没有爬取过的条目到 unseenLinks 通道, 然后被爬取 goroutine 接收.  
> senn map 被限制在主 goroutine 里面, 它仅仅需要被这个 goroutine 访问. 与其他形式的信息隐藏一样, 范围限制可以帮助我们推导程序的正确性. 例如, 局部变量不能在声明它的函数之外通过名字来引用; 没有从函数中逃逸的变量不能从函数外面访问; 一个对象的封装域只能对象自己的方法访问. 所有的场景中, 信息隐藏帮助限制程序不同部分之间不经意的交互.  
> crawl 发现的链接通过精心设计的 goroutine 发送到任务列表来避免死锁.

#### 8.7 使用 select 多路复用

_下面的程序对火箭发射进行倒计时. time.Tick 函数返回一个通道, 它定期发送事件, 像一个节拍器一样. 每个事件值是一个时间戳:_

```go
func main() {
    fmt.Println("Commencing countdown.")
    tick := time.Tick(1 * time.Second)
    for countdown := 10; countdown > 0; countdown-- {
        fmt.Println(countdown)
        <-tick
    }
    launch()
}
```

> 让我们通过在倒计时进行时按下回车键来取消发射过程的能力. 第一步, 启动一个 goroutine 试图从标准输入中读取一个字符, 如果发射成功, 发送一个值到 abort 通道:

```go
abort := make(chan struct{})
go func () {
    os.Stdin.Read(make([]byte, 1)) // read a single byte
    abort <- struct{}{}
}()
```

> 现在每一次倒计时迭代需要等待事件到达两个通道中的一个: 计数器通道, 前提是一切顺利 (nominal); 或者中止事件前提是有 "异常" (anomaly). 不能只从一个通道上接收, 因为哪一个操作都会在完成前阻塞. 所以需要多路复用那些操作过程, 为了实现这个目的, 要一个 select 语句:

```go
select {
case <-ch1:
// ...
case x := <-ch2:
// ...use x...
case ch3 <- y:
// ...
default:
// ...
}
```

> 上面展示的是 select 语句的通用形式. 像 switch 语句一样, 它有一系列的情况和一个可选的默认分支. 每一种情况指定一次通信 (在一些通道上进行发送或接收操作) 和关联的一段代码块. 接收表达式操作可能出现在它本身上, 像第一个情况, 或者在一个短变量声明中, 像第二个情况; 第二种形式可以让你应用所接收的值.  
> select 一直等待, 直到一次通信来告知有一些情况可以执行. 然后, 它进行这次通信, 执行此情况对应的语句; 其他通信将不会发生. 对于没有对应情况的 select, select{} 将永远等待.  
> 让我们回到火箭发射程序. time.After 函数立即返回一个通道, 然后启动一个新的 goroutine 在间隔指定时间后, 发送一个值到它上面. 下面的 select 语句等两个事件中第一个到达的事件, 中止事件或者指示事件过去 10s 的事件. 如果过了 10s 没有中止, 开始发射:

```go
func main() {
    // ...create abort channel...
    fmt.Println("Commencing countdown. Press return to abort.")
    select {
    case <-time.After(10 * time.Second):
        // Do nothing.
    case <-abort:
        fmt.Println("Launch aborted!")
        return
    }
    launch()
}
```

> 下面的例子更微妙一些. 通道 ch 的缓冲区大小为 1, 它要么是空的, 要么是满的, 因此只有在其中一个情况下可以执行, 要么在 i 是偶数时发送, 要么在 i 是奇数时接收. 它总是输出 0 2 4 6 8 :

```go
    ch := make(chan int, 1)
    for i := 0; i < 10; i++ {
        select {
        case x := <-ch:
            fmt.Println(x) // "0" "2" "4" "6" "8"
        case ch <- i:
        }
    }
```

> 如果多个情况同时满足, select 随机选择一个, 这样保证每一个通道有相同的机会被选中. 在前一个例子中增加缓冲区的容量, 会使输出变得不确定, 因为但缓冲区既不空也不满的情况, 相当于 select 语句在扔硬币做选择.  
> 让该发射程序输出倒计时. 下面的 select 语句使每一次迭代使用 1s 来等待中止, 但不会更长:

```go
func main() {
    // ...create abort channel...
    fmt.Println("Commencing countdown. Press return to abort.")
    tick := time.Tick(1 * time.Second)
    for countdown := 10; countdown > 0; countdown-- {
        fmt.Println(countdown)
        select {
        case <-tick:
            // Do nothing.
        case <-abort:
            fmt.Println("Launch aborted!")
            return
        }
    }
    launch()
}
```

> time.Tick 函数的行为很像创建一个 goroutine 在循环里调用 time.Sleep, 然后在它每次醒来时发送事件. 当上面的倒计时函数返回时, 它停止从 tick 通道中接收事件, 但是倒计时器 goroutine 还在运行, 徒劳地向一个没有 goroutine 在接收地通道不断发送 (发生 goroutine 泄露).  
> Tick 函数很方便使用, 但是它仅仅在应用地整个生命周期中都需要时才合适. 否则, 我们需要使用这个模式:

```go
ticker := time.NewTicker(1 * time.Second)
<-ticker.C // receive from the ticker's channel
ticker.Stop() // cause the ticker's goroutine to terminate
```

> 有时候我们试图在一个通道上发送或接收, 但是不想在通道没有准备好地情况下被阻塞 (非阻塞通信). 这时候用 select 语句可以做到. select 可以有一个默认情况, 它用来指定在没有其他通信发生时可以立即执行地动作.  
> 下面地 select 语句尝试从 abort 通道接收一个值, 如果没有值, 它上面都不做. 这个是一个非阻塞的接收操作; 重复这个动作称为对通道轮询:

```go
select {
case <-abort:
fmt.Printf("Launch aborted!\n")
return
default:
// do nothing
}
```

> 通道的零值是 nil. 令人惊讶的是, nil 通道有时候很有用. 因为在 nil 通道上发送和接收将永远阻塞, 对于 select 语句中的情况, 如果其通道是 nil, 它将永远不会被选择. 这次让我们用 nil 来开启或禁用特性所对应的情况, 比如超时处理或者取消操作, 响应其他的输入事件或者发送事件.

#### 8.8 示例: 并发目录遍历

_这一节中, 我们构建一个程序, 根据命令行指定的输入, 报告一个或多个目录的磁盘使用情况, 类似于 UNIX du 命令. 大多数的工作由下面的 walkDir 函数完成, 它使用 dirents 辅助函数来枚举目录中的条目._

```go

// walkDir recursively walks the file tree rooted at dir
// and sends the size of each found file on fileSizes.
func walkDir(dir string, fileSizes chan<- int64) {
    for _, entry := range dirents(dir) {
        if entry.IsDir() {
            subdir := filepath.Join(dir, entry.Name())
            walkDir(subdir, fileSizes)
        } else {
            fileSizes <- entry.Size()
        }
    }
}

// dirents returns the entries of directory dir.
func dirents(dir string) []os.FileInfo {
    entries, err := ioutil.ReadDir(dir)
    if err != nil {
        fmt.Fprintf(os.Stderr, "du1: %v\n", err)
        return nil
    }
    return entries
}

```

> ioutil.ReadDir 函数返回一个 os.FileInfo 类型的 slice, 针对单个文件同样的信息可以通过调用 os.Stat 函数来返回. 对每一个子目录, walkDir 递归调用它自己, 对于每一个文件, walkDir 发送一条消息到 fileSizes 通道. 消息是文件占用的字节数.  
> 如下所示, main 函数使用两个 goroutine. 后台 goroutine 调用 walkDir 遍历命令行上指定的每一个目录, 最后关闭 fileSizes 通道. 主 goroutine 计算从通道中接收的文件的大小的和, 最后输出总数.

```go

// The du1 command computes the disk usage of the files in a directory.
package main
import (
"flag"
"fmt"
"io/ioutil"
"os"
"path/filepath"
)
func main() {
    // Determine the initial directories.
    flag.Parse()
    roots := flag.Args()
    if len(roots) == 0 {
        roots = []string{"."}
    }
    // Traverse the file tree.
    fileSizes := make(chan int64)
    go func() {
        for _, root := range roots {
            walkDir(root, fileSizes)
        }
        close(fileSizes)
    }()
    // Print the results.
    var nfiles, nbytes int64
    for size := range fileSizes {
        nfiles++
        nbytes += size
    }
    printDiskUsage(nfiles, nbytes)
}
func printDiskUsage(nfiles, nbytes int64) {
    fmt.Printf("%d files %.1f GB\n", nfiles, float64(nbytes)/1e9)
}
```

> 如果程序可以通知它的进度, 将会更友好. 但是仅仅把 printDiskUsage 调用移动到循环内部会使它输出数千行结果.  
> 下面这个 du 的变种周期性地输出总数, 只有在 -v 标识指定地时候才输出, 因为不是所有用户都想看进度消息. 后台 goroutine 依然从根部开始迭代. 主 goroutine 现在使用一个计时器每 500ms 定期产生事件, 使用一个 select 语句或者等待一个关于文件大小地消息, 这时它更新总数, 或者等待一个计时事件, 这时它输出当前地总数. 如果 -v 标识没有指定, tick 通道依然为 nil, 它对应地情况在 select 中实际上被禁用.

```go
func main() {
    // ...start background goroutine...
    // Print the results periodically.
    var tick <-chan time.Time
    if *verbose {
        tick = time.Tick(500 * time.Millisecond)
    }
    var nfiles, nbytes int64
loop:
    for {
        select {
        case size, ok := <-fileSizes:
            if !ok {
                break loop // fileSizes was closed
            }
            nfiles++
            nbytes += size
        case <-tick:
            printDiskUsage(nfiles, nbytes)
        }
    }
    printDiskUsage(nfiles, nbytes) // final totals
}

```

> 因为这个程序没有使用 range 循环, 所以第一个 select 情况必须显示判断 fileSizes 通道是否已经关闭, 使用两个返回值地形式进行接收操作. 如果通道已经关闭, 程序退出循环. 标签化的 break 语句将跳出 select 和 for 循环逻辑; 没有标签的 break 只能跳出 select 的逻辑, 导致循环的下一次迭代.  
> 但是它依然耗费太长时间. 这里没有理由不能并发调用 walkDir 从而充分利用磁盘系统的并行机制. 第三个版本的 du 为每一个 walkDir 的调用创建一个新的 goroutine. 它使用 sync.WaitGroup 来为当前存活的 walkDir 调用计数, 一个关闭者 goroutine 在计数器减为 0 的时候关闭 fileSizes 通道.

```go
func main() {
    // ...determine roots...
    // Traverse each root of the file tree in parallel.
    fileSizes := make(chan int64)
    var n sync.WaitGroup
    for _, root := range roots {
        n.Add(1)
        go walkDir(root, &n, fileSizes)
    }
    go func() {
        n.Wait()
        close(fileSizes)
    }()
    // ...select loop...
}
func walkDir(dir string, n *sync.WaitGroup, fileSizes chan<- int64) {
    defer n.Done()
    for _, entry := range dirents(dir) {
        if entry.IsDir() {
            n.Add(1)
            subdir := filepath.Join(dir, entry.Name())
            go walkDir(subdir, n, fileSizes)
        } else {
            fileSizes <- entry.Size()
        }
    }
}
```

> 因为程序高峰时创建数千个 goroutine, 所以我们不得不修改 dirents 函数来使用计数信号量, 以防止它同时打开太多文件:

```go
// sema is a counting semaphore for limiting concurrency in dirents.
var sema = make(chan struct{}, 20)
// dirents returns the entries of directory dir.
func dirents(dir string) []os.FileInfo {
sema <- struct{}{} // acquire token
defer func() { <-sema }() // release token
// ...
```

> 尽管系统与系统之间有很多不同, 但是这个版本的数度比之前一个版本快几倍.

#### 8.9 取消

_有时候我们需要让一个 goroutine 停止它当前任务, 例如, 一个 Web 服务器对客户请求处理到一半的时候客户端断开了._  
_一个 goroutine 无法直接中止另一个, 因为这样会让所有的共享变量状态处于不确定状态. 在 8.7 节火箭发射程序中, 我们给 abort 通道发送一个值, 倒计时 goroutine 把它理解为停止自己的请求. 但是怎样才能取消两个或者特定个数的 goroutine 呢&#63;_  
_一个可能是给 abort 通道发送和要取消的 goroutine 一样多的事件. 如果一些 goroutine 已经自己终止了, 这样计数就多了, 然后发送过程会卡住. 如果那些 goroutine 可以自我繁殖, 数量又会太少, 其中一些 goroutine 依然不知道要取消. 通常, 任何时刻都很难知道有多少 goroutine 正在工作. 更多情况下, 当一个 goroutine 从 abort 通道接收到值时, 它利用这个值, 这样其他的 goroutine 接收不到这个值. 对于取消操作, 我们需要一个可靠的机制在一个通道上广播一个事件, 这样很多 goroutine 可以认为它发生了, 然后可以看到它已经发生._  
_回忆一下, 当一个通道关闭且已取完所有发送的值之后, 接下来的接收操作立即返回, 得到零值. 我们可以利用它创建一个广播机制: 不在通道上发送值, 而是关闭它._  
_我们在前一节的 du 程序中加入取消机制. 第一步, 创建一个取消通道, 在它上面不发送任何值, 但是它的关闭声明程序需要停止它正在做的事情. 也定义了一个工具函数 cancelled, 在它被调用的时候检测或轮询取消状态._

```go
var done = make(chan struct{})

func cancelled() bool {
    select {
    case <-done:
        return true
    default:
        return false
    }
}
```

> 接下来, 创建一个读取标准输入的 goroutine, 它通常连接到终端. 一旦开始读取任何输入时, 这个 goroutine 通过关闭 done 通道来广播取消事件.

```go
// Cancel traversal when input is detected.
go func() {
os.Stdin.Read(make([]byte, 1)) // read a single byte
close(done)
}()
```

> 现在我们需要让 goroutine 来响应取消操作. 在主 goroutine 中, 添加第三个情况到 select 语句中, 它尝试从 done 通道接收. 如果选择这个情况, 函数将返回, 但是在返回前它必须耗尽 fileSizes 通道, 丢弃所有的值, 直到通道关闭. 做这些时为了保证所有的 walkDir 调用可以执行完, 不会卡在向 fileSizes 通道发送消息上.

```go

    for {
        select {
        case <-done:
            // Drain fileSizes to allow existing goroutines to finish.
            for range fileSizes {
                // Do nothing.
            }
            return
        case size, ok := <-fileSizes:
            // ...
        }
    }
```

> walkDir goroutine 在开始的时候轮询取消状态, 如果设置状态, 什么都不做立即返回. 它让在取消后创建的 goroutine 什么都不做:

```go
func walkDir(dir string, n *sync.WaitGroup, fileSizes chan<- int64) {
    defer n.Done()
    if cancelled() {
        return
    }
    for _, entry := range dirents(dir) {
        // ...
    }
}
```

> 在 walkDir 循环中来进行取消状态轮询也许是划算的, 它避免在所取消后创建新的 goroutine. 取消需要权衡: 更快的响应通常需要更多的程序逻辑变更入侵. 确保在取消事件以后没有更多昂贵的操作发生, 可能需要更新代码中很多的地方, 但通常我们可以通过在少量重要的地方检查取消状态来达到目的.  
> 程序的一点性能剖析揭示了它的瓶颈在于 dirents 中获取信号量令牌操作. 下面的 select 让取消操作延迟从数百毫秒减为几十毫秒:

```go
func dirents(dir string) []os.FileInfo {
select {
case sema <- struct{}{}: // acquire token
case <-done:
return nil // cancelled
}
defer func() { <-sema }() // release token
// ...read directory...
}
```

> 现在, 当取消事件发生时, 所有后台 goroutine 迅速停止, 然后 mian 函数返回. 当然, 当 mian 返回时, 程序随之退出, 不过这里没有谁在后面通知 mian 函数来进行清理. 在测试中有一个技巧: 如果在取消事件到来的时候 main 函数没有返回, 执行一个 panic 调用, 然后运行时将转储程序中所有 goroutine 的栈. 如果主 goroutine 时最后一个剩下的 goroutine, 它需要自己进行清理. 但如果还有其他的 goroutine 存活, 它们可能还没有合适地取消, 或者它们已经取消, 可是需要地事件比较长; 多一点调查总是值得地. 崩溃转储信息通常含有足够地信息来分辨这些情况.

#### 8.10 示例: 聊天服务器

聊天服务器, 它可以在几个用户之间互相广播文本消息. 这个程序里有 4 个 goroutine. 主 goroutine 和广播 (broadcaster) goroutine, 每一个连接里有一个连接处理 (handleConn)

> 下面一个是给广播器, 它使用局部变量 clients 来记录当前连接地客户端集合. 每个客户唯一被记录地信息是其对外发送消息通道地 ID, 下面是细节:

```go
type client chan<- string // an outgoing message channel
var (
    entering = make(chan client)
    leaving  = make(chan client)
    messages = make(chan string) // all incoming client messages
)

func broadcaster() {
    clients := make(map[client]bool) // all connected clients
    for {
        select {
        case msg := <-messages:
            // Broadcast incoming message to all
            // clients' outgoing message channels.
            for cli := range clients {
                cli <- msg
            }
        case cli := <-entering:
            clients[cli] = true
        case cli := <-leaving:
            delete(clients, cli)
            close(cli)
        }
    }
}
```

> 广播者监听两个全局地通道 entering 和 leaving, 通过它们通知客户地到来和离开, 如果它从其中一个接收事件, 它将更新 clients 集合. 如果客户离开, 那么它关闭对应客户端对外发送消息地通道. 广播者也监听来自 messages 通道地事件, 所有地客户端都将消息发送到这个通道. 当广播者接收到其中一个事件时, 它把消息广播给所有客户.  
> 现在来看一下每个客户自己地 goroutine. handleConn 函数创建一个对外发送消息地新通道, 然后通过 entering 通道通知广播者新客户到来. 接着, 它读取客户端发送来地每一行文本, 通过全军接收消息通道将每一行发送给广播者, 发送时在每条消息前面加上发送者 ID 作为前缀. 一旦客户端读取完毕消息, handleConn 通过 leaving 通道通知客户离开, 然后关闭连接.

```go
func handleConn(conn net.Conn) {
    ch := make(chan string) // outgoing client messages
    go clientWriter(conn, ch)
    who := conn.RemoteAddr().String()
    ch <- "You are " + who
    messages <- who + " has arrived"
    entering <- ch
    input := bufio.NewScanner(conn)
    for input.Scan() {
        messages <- who + ": " + input.Text()
    }
    // NOTE: ignoring potential errors from input.Err()
    leaving <- ch
    messages <- who + " has left"
    conn.Close()
}
func clientWriter(conn net.Conn, ch <-chan string) {
    for msg := range ch {
        fmt.Fprintln(conn, msg) // NOTE: ignoring network errors
    }
}
```

> 另外, handleConn 函数还为每一个客户端创建了写入 ( clientWriter ) goroutine, 它接收消息, 广播到客户地发送消息通道中, 然后将它们写入到客户地网络连接中. 客户写入者地循环在广播者收到 leaving 通知并关闭客户地发送消息通道后终止.  
> 下面地信息展示了同一个机器上地一个服务器和两个客户端, 它们使用 netcat 程序来聊天:

```go
 go build gopl.io/ch8/chat
 go build gopl.io/ch8/netcat3
 ./chat &
 ./netcat3
You are 127.0.0.1:64208  ./netcat3
127.0.0.1:64211 has arrived You are 127.0.0.1:64211
Hi!
127.0.0.1:64208: Hi! 127.0.0.1:64208: Hi!
Hi yourself.
127.0.0.1:64211: Hi yourself. 127.0.0.1:64211: Hi yourself.
^C
127.0.0.1:64208 has left
 ./netcat3
You are 127.0.0.1:64216 127.0.0.1:64216 has arrived
Welcome.
127.0.0.1:64211: Welcome. 127.0.0.1:64211: Welcome.
^C
127.0.0.1:64211 has left
```

> 当有 n 个客户 session 在连接地时候, 程序并发运行着 2n+2 个互相通信的 goroutine, 它不需要隐式的加锁操作. clients map 限制在广播器这一个 goroutine 中被访问, 所以并不会并发访问它. 唯一被多个 goroutine 共享的变量是通道以及 net.Conn 实例, 它们又都是并发安全的.

### 第 9 章 使用共享变量实现并发

_本章将深入到并发机制内部, 特别是多个 goroutine 共享变量相关的问题, 以及识别这些问题的分析技术, 还有解决这些问题的模式. 最后将解释一下 goroutine 和操作系统线程的差别._

#### 9.1 竞态

_在串行程序中 (即一个程序只有一个 goroutine), 程序中各个步骤的执行顺序由程序逻辑来决定. 比如, 在一系列语句中, 第一句在第二句之前执行, 以此类推. 当一个程序有两个或者多个 goroutine 时, 每个 goroutine 内部的各个步骤也是顺序执行的, 但是我们无法知道一个 goroutine 中的事件 x 和另一个 goroutine 中的事件 y 的先后顺序. 如果我们无法明确的说一个事件肯定先于另外一个事件, 那么这两个事件就是并发的._  
_考虑一个能在串行程序中正确工作的函数. 如果这个函数在并发调用时仍然能正确工作, 那么这个函数是并发安全 (concurrency-safe) 的, 在这里并发调用是指, 在没有额外同步机制的情况下, 从两个或者多个 goroutine 同时调用这个函数. 这个概念也可以推广到其他函数, 比如方法或者作用于特定类型的一些操作. 如果一个类型的所有可访问和操作都是并发安全时, 则它可称为并发安全的类型._  
_让一个程序并发安全并不需要其中的每一个具体类型都是并发安全的. 实际上, 并发安全的类型其实是特例而不是普遍存在的, 所以仅在文档指出类型是安全的情况下, 才可以并发地访问一个变量. 对于绝大部分变量, 如果回避并发访问, 那么限制变量只存在于一个 goroutine 内, 那么维护一个更高层地互斥不变量._  
_与之对应地是, 导出地包级别函数通常可以认为是并发安全的. 因为包级别的变量无法限制在一个 goroutine 内, 所以那些修改这些变量的函数就必须采用互斥机制._  
_函数并发调用时不工作的原因有很多, 包括死锁, 活锁 (livelock) 以及资源耗尽._  
_竞态是指在多个 goroutine 按某些交错顺序执行时无法给出正确的结果. 竞态对于程序是致命的, 因为它们可能潜伏在程序中, 出现频率也很低, 有可能仅在高负载环境或者使用特定的编译器, 平台和构架时才出现. 这些都让竞态很艰难再现和分析._
_考虑一个简单的银行账户程序:_

```go
// Package bank implements a bank with only one account.
package bank
var balance int
func Deposit(amount int) { balance = balance + amount }
func Balance() int { return balance }
```

> ( Deposit 的函数体也可以写作等价的 balance += amount, 但比较长的形式解释起来比较方便.)  
> 对于一个如此简单的程序, 我们可以一眼看出, 任意串行地调用 Deposit 和 Balance 都可以得到正确地结果. 即 Balance 会输出之前存入地金额总数. 但如果这些函数地调用顺序不是串行而是并行, Balance 就不保证输出正确地结果了. 考虑如下两个 goroutine, 它们代表对一个共享账户地两笔交易:

```go
// Alice:
go func () {
    bank.Deposit(200)                // A1
    fmt.Println("=", bank.Balance()) // A2
}()
// Bob:
go bank.Deposit(100) // B
```

> Alice 存入 200 美元, 然后她查询她地余额, 与此同时 Bob 存入了 100 美元. A1, A2 两步与 B 是并发进行地, 我们无法预测实际执行顺序. 自觉来看, 可能存在, 三种不同顺序, 分别为 "Alice 先", "Bob 先" 和 "Alice/Bob/Alice". 下面地表格显示了每一个步骤之后 balance 变量地值. 带引号地字符串代表输出地账户余额.

```go
Alice first Bob first Alice/Bob/Alice
0 0 0
A1 200 B 100 A1 200
A2 "= 200" A1 300 B 300
B 300 A2 "= 300" A2 "= 300"
```

> 在所有地情况下最终地账户余额都是 300 美元. 唯一不同地是 Alice 看到地账户余额是否包含了 Bob 地交易, 但客户对所有情况都不会有不满.  
> 但这种知觉是错的. 这里还有第四种可能, Bob 地存款在 Alice 地存款操作中间执行, 晚于账户余额读取 (balance+amount), 但早于余额更新 (balance = ...), 这会导致 Bob 存的钱消失了. 这是因为 Alice 的存款操作 A1 实际上是两个串行的操作, 读部分和写部分, 我们称之为 A1r 和 Alw. 下面就是有问题的执行顺序:

```go
Data race
0
A1r 0 ... = balance + amount
B 100
A1w 200 balance = ...
A2 "= 200"
```

> 在 A1r 之后, 表达式 balance + amount 求值结果为 200, 这个值在 A1w 步骤中用于写入, 完全没理会中间操作. 最终的余额为仅有 200 美元, 银行从 Bob 手上挣了 100 美元.  
> 程序中的这种状况是竞态的变量中的一种, 称为数据竞态 (data race). 数据竞态发生于两个 goroutine 并发读写同一个变量并且至少其中一个是写入时.  
> 当发生数据竞态的变量类型是大于一个机器字长的类型 (比如接口, 字符串或 slice) 时, 事情就更复杂了. 下面的代码并发把 x 更新为两个不同长度的 slice.

```go
var x []int
go func() { x = make([]int, 10) }()
go func() { x = make([]int, 1000000) }()
x[999999] = 1 // NOTE: undefined behavior; memory corruption possible!
```

> 最后一个表达式中 x 的值是未定义的, 它可能是 nil, 一个长度为 10 的 slice 或者一个长度为 1000000 的 slice. 回想一下 slice 的三个部分: 指针, 长度和容量. 如果指针来自于第一个 make 调用而长度来自第二个 make 调用, 那么 x 会变成一个嵌合体, 它名义上长度为 1000000 但是底层数组只有 10 个元素. 在这种情况下, 尝试存储到 999999 个元素会伤及很遥远的一段内存, 其恶果无法预测, 问题也难以调试和定位. 这种语义上的雷区称为未定义行为.  
> 并行程序是几个串行串行交替执行这一个概念也是一个错觉. 数据竞态可能由奇怪的原因引发. 一个好的习惯是根本就没有什么温和的数据竞态.  
> 数据竞态发生于两个 goroutine 并发读写同一个变量并且至少其中一个是写操作. 从定义不难看出, 有三种方法避免数据竞态.  
> 第一种发生是不修改变量. 考虑如下的 map, 它进行了延迟初始化, 对于每个键, 在第一次访问时才触发加载. 如果 Icon 的调用是串行的, 那么程序能正常工作, 但如果 Icon 的调用是并发的, 在访问 map 时就存在数据竞态.

```go
var icons = make(map[string]image.Image)

func loadIcon(name string) image.Image

// NOTE: not concurrency-safe!
func Icon(name string) image.Image {
    icon, ok := icons[name]
    if !ok {
        icon = loadIcon(name)
        icons[name] = icon
    }
    return icon
}
```

> 如果在创建其他 goroutine 之前就用完整的数据来初始化 map, 并且不再修改. 那么无论多少 goroutine 也可以安全并发调用 Icon, 因为每个 goroutine 都只读取这个 map.

```go
var icons = map[string]image.Image{
"spades.png": loadIcon("spades.png"),
"hearts.png": loadIcon("hearts.png"),
"diamonds.png": loadIcon("diamonds.png"),
"clubs.png": loadIcon("clubs.png"),
}
// Concurrency-safe.
func Icon(name string) image.Image { return icons[name] }
```

> 在上面的例子中, icons 变量的赋值发生在包初始化时, 也就是在程序的 main 函数开始运行之前. 一旦初始化完成后, icons 就不再修改了. 那么从不修改的数据结构以及不可变数据结构本质时并发安全的, 也不需要做任何同步. 但显然我们不能把这个方法用到必然会有更新的场景, 比如一个银行账号.  
> 第二种避免数据竞争的方法是避免从多个 goroutine 访问同一个变量.  
> 由于其他 goroutine 无法直接访问相关变量, 因此它们必须使用通道来向受限 goroutine 发送查询请求或者更新变量. 这也是这句 Go 箴言的含义: "不要通过共享内存来通信, 而应该通过通信来共享内存". 使用通道请求来代理一个受限变量的所有访问的 goroutine 称为该变量的监控 goroutine (monitor goroutine). 比如, broadcaster goroutine 监控了 对 clients map 的访问.  
> 下面就是重写的银行案例, 用一个叫 teller 的监控 goroutine 限制 balance 变量:

```go
// Package bank provides a concurrency-safe bank with one account.
package bank
var deposits = make(chan int) // send amount to deposit
var balances = make(chan int) // receive balance
func Deposit(amount int)      { deposits <- amount }
func Balance() int            { return <-balances }
func teller() {
    var balance int // balance is confined to teller goroutine
    for {
        select {
        case amount := <-deposits:
            balance += amount
        case balances <- balance:
        }
    }
}
func init() {
    go teller() // start the monitor goroutine
}

```

> 即使一个变量无法在整个生命周期受限于单个 goroutine, 加以限制仍然可以解决并发访问的好方法. 比如一个常见的场景, 可以通过借助通道来把共享变量地址从上一步传递到下一步, 从而在流水线上的多个 goroutine 之间共享该变量. 在流水线中的每一步, 在把变量地址传递给下一步后就不再访问该变量的, 这样对于所有对这个变量的访问都是串行的. 换个说法, 这个变量先受限于流水线的第一步, 再受限于下一步, 以此类推. 这种受限有时候也称为串行受限.  
> 在下面的例子中, Cakes 是串行受限的, 首先受限于 baker goroutine, 然后受限于 icer goroutine.

```go
type Cake struct{ state string }

func baker(cooked chan<- *Cake) {
    for {
        cake := new(Cake)
        cake.state = "cooked"
        cooked <- cake // baker never touches this cake again
    }
}
func icer(iced chan<- *Cake, cooked <-chan *Cake) {
    for cake := range cooked {
        cake.state = "iced"
        iced <- cake // icer never touches this cake again
    }
}
```

> 第三种避免数据竞态的方法是允许多个 goroutine 访问同一个变量, 但在同时只能有一个 goroutine 可以访问. 这种方法称为互斥机制, 这是下一节的主题.

#### 9.2 互斥锁: sync.Mutex

_在 8.6 节, 使用一个缓冲通道实现了一个计数信号量, 用于确认同时发起 HTTP 请求的 goroutine 数量是不超过 20. 使用同样的理念, 也可以用一个容量为 1 的通道来保证同一个时间最多有一个 goroutine 能访问共享变量. 一个计数上限为 1 的信号量称为 二进制信号量 (binary semaphore)._

```go
var (
    sema    = make(chan struct{}, 1) // a binary semaphore guarding balance
    balance int
)

func Deposit(amount int) {
    sema <- struct{}{} // acquire token
    balance = balance + amount
    <-sema // release token
}
func Balance() int {
    sema <- struct{}{} // acquire token
    b := balance
    <-sema // release token
    return b
}
```

> 互斥锁模式应用非常广泛, 所以 sync 包有一个单独的 Mutex 类型来支持这种模式. 它的 Lock 方法由于获取令牌 (token, 此过程也称为上锁), Unclock 方法用于释放令牌:

```go
import "sync"
var (
    mu      sync.Mutex // guards balance
    balance int
)

func Deposit(amount int) {
    mu.Lock()
    balance = balance + amount
    mu.Unlock()
}
func Balance() int {
    mu.Lock()
    b := balance
    mu.Unlock()
    return b
}
```

> 一个 goroutine 在每次访问银行的变量 (此处仅有 balance) 之前, 它都必须先调用互斥量的 Lock 方法来获取一个互斥锁. 如果其他 goroutine 已经取走了互斥锁, 那么操作会一直阻塞到其他 goroutine 调用 Unclock 之后 (此时互斥锁再度可用). 互斥量保护共享变量. 按照惯例, 被互斥锁保护的变量声明但紧接在互斥量声明之后. 如果实际情况不是如此, 请确认已加注释来说明此事.  
> 在 Lock 和 Unlock 之间的代码, 可以自由地读取和修改共享变量, 这一部分称为临界区域. 在锁的持有人调用 Unlock 之前, 其他 goroutine 不能获取锁. 所以很重要的一点是, goroutine 在使用完以后就应当释放锁, 另外需要包括函数所有分支, 特别是错误分支.
> 上面的银行程序展现了一个典型的并发模式. 几个导出函数函数封装了一个或者多个变量, 于是只能通过这些函数来访问这些变量 (对于一个对象的变量, 则用方法来封装). 每个函数在开始时申请一个互斥锁, 在结束时再释放掉, 通过这种形式来确保共享变量不会被并发访问. 这种函数, 互斥锁, 变量的组合方式被称为监控 (monitor) 模式. (之前再监控 goroutine 中也使用了监控 (monitor) 这个词, 都代表使用一个代理人 (broker) 来确保变量按顺序访问.)  
> 因为 Deposit 和 Balance 函数中的临界区域都很短 (只有一行, 也没有分支), 所以直接在函数结束时调用 Unlock 也很方便. 在更复杂的临界场景中, 特别是必须通过提前返回来处理错误的场景, 很难确定所有分支中 Lock 和 Unlock 都成对执行了. Go 语言的 defer 语句就可以解决这个问题: 通过延迟执行 Unlock 就可以解决这个问题: 通过延迟执行 Unlock 就可以把临界区隐式扩展到当前函数的结尾, 避免了必须在一个或者多个远离 Lock 的位置插入一条 Unlock 语句.

```go
func Balance() int {
mu.Lock()
defer mu.Unlock()
return balance
}
```

> 在上面例子中, Unlock 在 return 语句已经读完 balance 变量之后执行, 所以 Balance 函数就是并发安全的. 另外, 我们也不需要使用局部变量 b 了.  
> 而且, 在临界区崩溃时延迟执行 Unlock 也会正确执行, 这在使用 recover 的情况下尤其重要. 当然, defer 的执行成本比显示调用 Unlock 略大一些, 但不足以成为代码不清晰的理由. 在处理并发程序时, 永远应当优先考虑清晰度, 并且拒绝过早优化. 在可以使用的地方, 就尽量使用 defer 来让临界区扩展到函数结尾处.  
> 考虑如下 Withdraw 函数. 当成功时, 余额减少了指定的数量, 并且返回 true, 但如果余额不足, 无法完成交易, withdraw 恢复余额并且返回 false.

```go
// NOTE: not atomic!
func Withdraw(amount int) bool {
    Deposit(-amount)
    if Balance() < 0 {
        Deposit(amount)
        return false // insufficient funds
    }
    return true
}

```

> 这个函数最终给出正确的结果, 但它有一个不良的副作用. 在尝试进行超额提款时, 在某个瞬间余额会降到 0 以下. 这有可能导致一个下额取款会不合逻辑地被拒绝掉. 所以当 Bob 尝试购买一辆跑车时, 却会导致 Alice 无法支付早上地咖啡. Withdraw 的问题在于不是原子操作: 它包含三个串行操作, 每个操作都申请并释放了互斥锁, 但对于整个序列没有上锁.  
> 理想情况下, Withdraw 应当为整个操作申请一次互斥锁. 但如下尝试是正确的:

```go
// NOTE: incorrect!
func Withdraw(amount int) bool {
    mu.Lock()
    defer mu.Unlock()
    Deposit(-amount)
    if Balance() < 0 {
        Deposit(amount)
        return false // insufficient funds
    }
    return true
}
```

> Deposit 会通过调用 mu.Lock() 来尝试再次获取互斥锁, 但是由于互斥锁不能再入 (无法对一个已经上锁的互斥量再上锁), 因此会导致死锁, Withdraw 会一直卡住.  
> Go 语言的互斥量是不可再入的, 具体理由见后. 互斥量的目的是在程序执行过程中维持基于共享变量的特定不变量 (invariant). 其中一个不变量是 "没有 goroutine 正在访问这个共享变量", 但有可能互斥量也保护针对数据结构的其他不变量. 当 goroutine 获取一个互斥锁的时候, 它可能会假定这些不变量是满足的. 当它获取到互斥锁之后, 它可能会更新共享变量的值, 这样可能会临时不满足之前的不变量. 当它释放互斥锁时, 它必须保证之前的不变量已经还原且又能重新满足. 尽管一个可重入的互斥变量可以确保没有其他 goroutine 可以访问共享变量, 但是无法保护这些变量的其他不变量.  
> 一个常见的解决方案是把 Deposit 这样的函数拆分成两部分: 一个不导出的函数 deposit, 它假定已经获得互斥锁, 并完成实际业务逻辑; 以及一个导出函数 Deposit, 它用来获取锁并调用 deposit. 这样我们就可以用 deposit 来实现 Withdraw:

```go
func Withdraw(amount int) bool {
    mu.Lock()
    defer mu.Unlock()
    deposit(-amount)
    if balance < 0 {
        deposit(amount)
        return false // insufficient funds
    }
    return true
}
func Deposit(amount int) {
    mu.Lock()
    defer mu.Unlock()
    deposit(amount)
}
func Balance() int {
    mu.Lock()
    defer mu.Unlock()
    return balance
}

// This function requires that the lock be held.
func deposit(amount int) { balance += amount }
```

> 封装即通过在程序中减少对数据结构的非预期交互, 来帮助我们保证数据结构中的不变量. 因为类似的原因, 封装也可以用来保持并发中的不变性. 所以无论是为了保护包级别的变量, 还是结构中的字段, 当你使用一个互斥量时, 都请确保互斥量本身以及被保护的变量都没有导出.

#### 9.3 读写互斥锁: sync.RWMutex

_BOb 的 100 美元存款消失了, 没留下任何线索, Bob 感到很焦虑, 为了解决这个问题, Bob 写了一个程序, 每秒钟查询他的账户余额. 这个程序同时在他家里, 公司和他的手机上运行. 银行注意到快速增长的业务请求正在拖慢存款和取款操作, 因为所有的 Balance 请求都是串行运行的, 持有互斥锁并暂时妨碍其他 goroutine 运行._  
_因为 Balance 函数只须读取变量状态, 所以多个 Balance 请求其实可以安全并发运行, 只要 Deposit 和 Withdraw 请求没有同时运行即可. 在这种场景下, 我们需要一种特殊类型的锁, 它允许只读操作可以并发执行, 但是写操作需要获得完全独享权限. 这种锁为多读单写锁, Go 语言中的 sync.RWMutex 可以提供这种功能:_

```go
var mu sync.RWMutex
var balance int
func Balance() int {
mu.RLock() // readers lock
defer mu.RUnlock()
return balance
}
```

> Balance 函数现在可以调用 RLock 和 RUnlock 方法来分别获取和释放一个读锁 (也称为共享锁). Deposit 函数无须更改, 它通过调用 mu.Lock 和 mu.Unlock 来分别获取和释放一个写锁 (也称为互斥锁).  
> 经过上面的修改之后, Bob 的绝大部分 Balance 请求可以并行运行且能够更快完成. 因此, 锁可用的时间比例会更大, Deposit 请求也能得到更及时的响应.  
> RLock 仅可用于在临界区内对共享变量无写操作的情形. 一般来讲, 我们不应当假定那些逻辑上只读的函数和方法不会更新一些变量. 比如, 一个看起来只是简单访问器的方法可能会递增内部使用的计数器, 或者更新一个缓存来让重复的调用更快. 如果你有疑问, 那么就应当使用独享版本的 Lock.  
> 仅在绝大部分 goroutine 都在获取读锁并且锁竞争比较激烈时 (即, goroutine 一般都需要等待后才能获得锁), RWMutex 才有优势. 因为 RWMutex 需要更复杂的内部簿记工作, 所以在竞争不激烈时它比普通互斥锁慢.

#### 9.4 内部同步

_你可能会对 Balance 方法也需要互斥锁 (不管是基于通道的锁还是基于互斥的锁) 感到奇怪. 毕竟, 与 Deposit 不一样, 它只包含单个操作, 所以并不存在另一个 goroutine 插在之间执行的风险. 其实需要互斥锁的原因有两个. 第一个是防止 Balance 插到其他操作中间也是很重要的, 比如 Withdraw. 第二个原因更微妙, 因为同步不仅涉及多个 goroutine 的执行顺序问题, 同步还会影响到内存._  
_现代计算机一般都会有多个处理器, 每个处理器都用内存的本地缓存. 为了提高效率, 对内存的写入是缓存在每个处理器中的, 只在必要时才刷回内存. 甚至刷回内存的顺序都可能与 goroutine 的写入顺序不一致. 像通道通信或者互斥操作这样的同步原语都会导致处理器把积累的写操作刷回内存并提交, 所以这个时刻之前 goroutine 的执行结果就保证了对运行在其他处理器的 goroutine 可见. 考虑如下代码片段的可能输出:_

```go
var x, y int
go func() {
x=1 // A1
fmt.Print("y:", y, " ") // A2
}()
go func() {
y=1 // B1
fmt.Print("x:", x, " ") // B2
}()
```

> 由于这两个 goroutine 并发运行且没有使用互斥锁的情况下访问共享变量, 所以这里会有数据竞态. 于是我们对程序每次的输出不一样不应该感到奇怪. 根据对程序中标注语句不同交错模式, 我们可能会期望能看到四个结果中的一个:

```go
y:0 x:1
x:0 y:1
x:1 y:1
y:1 x:1
```

> 第四行可以由 A1,B1,A2,B2 或 B1,A1,A2,B2 这样的执行顺序产生. 但是, 程序产生的如下两个输出就在我们的意料之外了:

```go
x:0 y:0
y:0 x:0
```

> 但在某些特定的编译器, CPU 或者其他情况下, 这些确实可能发生. 上面四个语句可以什么样的顺序交错执行才能解释这个结果呢&#63;  
> 在单个 goroutine 内, 每个语句的效果保证按照执行的顺序发生, 也就是说, goroutine 是串行一致的 (sequential consistent). 但在缺乏使用通道或者互斥量来显示同步的情况下, 并不能保证所有的 goroutine 看到的事件顺序都是一致的. 尽管 goroutine A 肯定能在读取 y 之前能观察到 x=1 的效果, 但它并不一定能观察到 goroutine B 对 y 写入的结果, 所以 A 可能会输出 y 的一个过期值.  
> 尽管很容易把并发简单理解为多个 goroutine 中语句的某种交错执行方式, 但正如上面的例子所显示的, 这并不是一个现代编译器和 CPU 的工作方式. 因为赋值和 Print 对应不同的变量, 所以编译器可能会认为两个语句的执行结果不会影响结果, 然后就交换了这两个语句的执行顺序. CPU 也有类似的问题, 如果两个 goroutine 在不同的 CPU 上执行, 每个 CPU 都有自己的缓存, 那么一个 goroutine 的写入操作在同步到内存之前对另一个 goroutine 的 Print 语句是不可见的.  
> 这些并发问题都可以通过采用简单, 成熟的模式来避免, 即在可能的情况下, 把变量限制到单个 goroutine 中, 对于其他变量, 使用互斥锁.

#### 9.5 延迟初始化: sync.Once

_延迟一个昂贵的初始化步骤到有实际需求的时刻是一个很好的实践. 预先初始化一个变量会增加程序启动延时, 并且如果实际执行时有可能根本用不上这个变量, 那么初始化也是不必须的. 回到本章之前的 icons 变量:_

```go
var icons map[string]image.Image
```

> 这个版本的 Icon 使用延迟初始化:

```go
func loadIcons() {
    icons = map[string]image.Image{
        "spades.png":   loadIcon("spades.png"),
        "hearts.png":   loadIcon("hearts.png"),
        "diamonds.png": loadIcon("diamonds.png"),
        "clubs.png":    loadIcon("clubs.png"),
    }
}

// NOTE: not concurrency-safe!
func Icon(name string) image.Image {
    if icons == nil {
        loadIcons() // one-time initialization
    }
    return icons[name]
}
```

> 因此, 一个 goroutine 发现 icons 不是 nil 并不意味着变量的初始化肯定已经完成.  
> 保证所有 goroutine 都能观察到 loadIcons 效果最简单的正确方法就是用一个互斥锁来做同步:

```go
var mu sync.Mutex // guards icons
var icons map[string]image.Image

// Concurrency-safe.
func Icon(name string) image.Image {
    mu.Lock()
    defer mu.Unlock()
    if icons == nil {
        loadIcons()
    }
    return icons[name]
}
```

> 采用互斥锁访问 icons 的额外代价是两个 goroutine 不能并发访问这个变量, 即使在变量已经安全完成初始化且不再更改的情况下, 也会造成这个后果. 使用一个可以并发读的锁就可以改善这个问题:

```go
var mu sync.RWMutex // guards icons
var icons map[string]image.Image

// Concurrency-safe.
func Icon(name string) image.Image {
    mu.RLock()
    if icons != nil {
        icon := icons[name]
        mu.RUnlock()
        return icon
    }
    mu.RUnlock()
    // acquire an exclusive lock
    mu.Lock()
    if icons == nil { // NOTE: must recheck for nil
        loadIcons()
    }
    icon := icons[name]
    mu.Unlock()
    return icon
}
```

> 这个有两个临界区. goroutine 实现获取一个读锁, 查阅 map, 然后释放这个读锁. 如果条目能找到, 就返回它. 如果条目没有找到, goroutine 再获取一个写锁. 由于不先释放一个共享锁就无法直接把它升级到互斥锁, 为了避免在过度其他 goroutine 已经初始化了 icons, 所以我们必须重新检查 nil 值.  
> 上面的模式具有更好的并发性, 但它更复杂并且容易出错. 幸运的是, sync 包提供了针对一次性初始化问题的特化解决方案: sync.Once. 从概念上来讲, Once 包含一个布尔变量和一个互斥量, 布尔变量记录初始化是否已经完成, 互斥量则负责保护这个布尔变量和一个互斥量, 布尔变量记录初始化是否已经完成, 互斥量则负责保护这个布尔变量和客户端的数据结构. Once 的唯一方法 Do 以初始化函数作为它的参数. 让我们看一下 Once 简化后的 Icon 函数:

```go
var loadIconsOnce sync.Once
var icons map[string]image.Image

// Concurrency-safe.
func Icon(name string) image.Image {
    loadIconsOnce.Do(loadIcons)
    return icons[name]
}

```

> 每次调用 Do(loadIcons) 时会先锁定互斥量并检查里面的布尔变量. 在第一次调用时, 这个布尔变量为假, Do 会调用 loadIcons 然后把变量设置为真. 后续的调用相当于空操作, 只是通过互斥量的同步来保证 loadIcons 对内存产生的效果 (在这里就是 icons 变量) 对所有的 goroutine 可见. 以这种方式来使用 sync.Once, 可以避免变量在正确构造之前就被其他 goroutine 分享.
>
> #### 9.6 竞态检测器

_即使以最大努力的仔细, 仍然很容易在并发上犯错. 幸运的是, Go 语言运行和工具链装备了一个精致并易于使用的动态分析工具: 竞态检测器 (race detector)._

> 简单的把 -race 命令行参数加到 go build, go run, go test 命令里面即可使用该功能. 它会让编译器为你的应用或测试构建一个修改后的版本,这个版本有额外的手法用于高效记录在执行时对共享变量的所有访问, 以及读写这些变量的 goroutine 标志. 除此之外, 修改后的版本还会记录所有同步事件, 包括 go 语句, 通道操作, (\*sync.Mutex).Lock 调用,(\*sync.WaitGroup).Wait 调用等. (完整的同步事件集合可以在语言规划的 "The Go Memory Model") 文档中找到)
> 竞态检测器会研究事件流, 找到那些有问题的案件, 即一个 goroutine 写入一个变量后, 中间没有任何操作, 就有另外一个 goroutine 读写了该变量. 这种案例表明有对共享变量的并发访问, 即数据竞态. 这个工具会输出一份报告, 包括变量的标识以及读写 goroutine 当时的调用栈. 通常情况下这些信息足以定位问题.  
> 竞态检测器报告所有实际运行了的数据竞态. 然而, 它只能检测到那些在运行时发生的竞态, 无法用来保证肯定不会发生竞态. 然而, 它只能检测到那些在运行时发生的竞态, 无法用来保证肯定不会发生竞态. 为了最佳效果, 请确保你的检测包含了并发使用包的场景.  
> 由于存在额外的簿记工作, 带竞态检测功能的程序在执行时需要更长的时间和更多的内存, 但即使对于很多生产环境的任务, 这种额外开支也是可以接收的. 对于那些不常发生的竞态, 使用竞态检测器可以帮你节省数小时甚至数天的调试时间.

#### 9.7 示例: 并发非阻塞缓存

_在本节中, 我们会创建一个并发非阻塞的缓存系统. 它能解决并发实战很常见但已有的库也不能很好的解决一个问题: 函数记忆 (memoizing) 问题, 即缓存函数的结果, 达到多次调用但只须计算一次的效果. 我们的解决方案是并发安全的, 并且要避免简单地对整个缓存使用单个锁而带来地锁争夺问题._
_我们将使用下面的 httpGetBody 函数作为示例来演示函数记忆. 它会发起一个 HTTP GET 请求并读取响应体. 调用这个函数相当昂贵, 所以我们希望避免不必要的重复调用._

```go

func httpGetBody(url string) (interface{}, error) {
    resp, err := http.Get(url)
    if err != nil {
        return nil, err
    }
    defer resp.Body.Close()
    return ioutil.ReadAll(resp.Body)
}

```

> 最后一行略有一些微妙, ReadAll 返回两个结果, 一个 []byte 和一个 error, 因为它们分别可以直接赋给 httpGetBody 声明的结果类型类型 interface{} 和一个 error, 所以我们可以直接返回这个结果不做额外的处理. httpGetBody 选择这样的结果类型是为了满足我们要做的缓存系统设计.  
> 下面是缓存的初始版本:

```go
// Package memo provides a concurrency-unsafe
// memoization of a function of type Func.
package memo
// A Memo caches the results of calling a Func.
type Memo struct {
    f     Func
    cache map[string]result
}

// Func is the type of the function to memoize.
type Func func(key string) (interface{}, error)
type result struct {
    value interface{}
    err   error
}

func New(f Func) *Memo {
    return &Memo{f: f, cache: make(map[string]result)}
}

// NOTE: not concurrency-safe!
func (memo *Memo) Get(key string) (interface{}, error) {
    res, ok := memo.cache[key]
    if !ok {
        res.value, res.err = memo.f(key)
        memo.cache[key] = res
    }
    return res.value, res.err
}
```

> 一个 Memo 实例包含了被记忆的函数 f (类型为 Func), 以及缓存, 类型为从字符串到 result 的一个映射表. 每个 result 都是调用 f 产生的结果对: 一个值和一个错误. 在设计的推进过程中我们会展示 Memo 的几种变体, 但所哟变体都会遵守这些基本概念.  
> 下面的例子展示如何使用 Memo. 对于一串请求 URL 中的每个元素, 首先调用 Get, 记录延时和它返回的数据长度:

```go
    m := memo.New(httpGetBody)
    for url := range incomingURLs() {
        start := time.Now()
        value, err := m.Get(url)
        if err != nil {
            log.Print(err)
        }
        fmt.Printf("%s, %s, %d bytes\n",
            url, time.Since(start), len(value.([]byte)))
    }
```

> 我们可以使用 testing 包来系统地调查记忆效果. 从下面地测试结果来看, 我们可以看到 URL 流有重复项, 尽管每个 URL 第一次调用 (\*Memo).Get 都会消耗数百毫秒地时间, 但对这个 URL 地第二次请求在 1 us 内就返回了同样地结果.

```go
 go test -v gopl.io/ch9/memo1
=== RUN Test
https://golang.org, 175.026418ms, 7537 bytes
https://godoc.org, 172.686825ms, 6878 bytes
https://play.golang.org, 115.762377ms, 5767 bytes
http://gopl.io, 749.887242ms, 2856 bytes
https://golang.org, 721ns, 7537 bytes
https://godoc.org, 152ns, 6878 bytes
https://play.golang.org, 205ns, 5767 bytes
http://gopl.io, 326ns, 2856 bytes
--- PASS: Test (1.21s)
PASS
ok gopl.io/ch9/memo1 1.257s
```

> 这次测试中所有地 Get 都是串行运行的. 因为 HTTP 请求用并发改善的空间很大所以我们修改测试来让所有请求并发进行. 这个测试使用 sync.WaitGroup 来做到等最后一个请求完成后再返回的效果.

```go
m := memo.New(httpGetBody)
var n sync.WaitGroup
for url := range incomingURLs() {
n.Add(1)
go func (url string) {
start := time.Now()
value, err := m.Get(url)
if err != nil {
log.Print(err)
}
fmt.Printf("%s, %s, %d bytes\n",
url, time.Since(start),               len(value.([]byte)))
        n.Done()
    }(url)
}
n.Wait()
```

> 这次的测试运行起来快了很多, 但是它并不是每一次都正常运行. 我们可能能注意到意料之外的缓存无效，以及缓存命中后返回错误的结果, 甚至崩溃。  
> 更糟糕的是, 有的时候它能正常运行, 所以我们可能甚至都没有注意到它会有问题. 但如果我们加上 -race 标志后再运行, 那么竞态检测器经常会输出与下面类似的一份报告:

```go
 go test -run=TestConcurrent -race -v gopl.io/ch9/memo1
=== RUN TestConcurrent
...
WARNING: DATA RACE
Write by goroutine 36:
runtime.mapassign1()
~/go/src/runtime/hashmap.go:411 +0x0
gopl.io/ch9/memo1.(*Memo).Get()
~/gobook2/src/gopl.io/ch9/memo1/memo.go:32 +0x205
. . . .

Previous write by goroutine 35:
runtime.mapassign1()
~/go/src/runtime/hashmap.go:411 +0x0
gopl.io/ch9/memo1.(*Memo).Get()
~/gobook2/src/gopl.io/ch9/memo1/memo.go:32 +0x205
. . . .
Found 1 data race(s)
FAIL gopl.io/ch9/memo1 2.393s
```

> 上面提到的 memo.go:32 告诉我们两个 goroutine 在没使用同步的情况下更新了 cache map. 整个 Get 函数其实不是并发安全的: 它存在数据竞态.

```go
28 func (memo *Memo) Get(key string) (interface{}, error) {
29 res, ok := memo.cache[key]
30 if !ok {
31 res.value, res.err = memo.f(key)
32 memo.cache[key] = res
33 }
34 return res.value, res.err
35 }
```

> 让缓存并发安全最简单的方法就是用一个基于监控的同步机制. 我们需要的是给 Memo 加一个互斥量, 并在 Get 函数的开头获取互斥锁, 在返回前释放互斥锁, 这个样两个 cache 相关的操作就发生在临界区域了:

```go
type Memo struct {
    f     Func
    mu    sync.Mutex // guards cache
    cache map[string]result
}

// Get is concurrency-safe.
func (memo *Memo) Get(key string) (value interface{}, err error) {
    memo.mu.Lock()
    res, ok := memo.cache[key]
    if !ok {
        res.value, res.err = memo.f(key)
        memo.cache[key] = res
    }
    memo.mu.Unlock()
    return res.value, res.err
}

```

> 现在即使并发运行测试, 静态检测器也没有报警. 但是这次对 Memo 的修改让我们之前对性能的优化失败了. 由于每次调用 f 时都上锁, 因此 Get 把我们希望并行的 I/O 操作串行化了. 我们需要的是一个非阻塞的缓存, 一个不会把他需要记忆的函数串行运行的缓存.  
> 在下面一个版本的 Get 实现中, 主调 goroutine 会分两次获取锁: 第一次用于查询, 第二次用于查询无返回结果时进行更新. 在两次之间, 其他 goroutine 也可以使用缓存.

```go
func (memo *Memo) Get(key string) (value interface{}, err error) {
    memo.mu.Lock()
    res, ok := memo.cache[key]
    memo.mu.Unlock()
    if !ok {
        res.value, res.err = memo.f(key)
        // Between the two critical sections, several goroutines
        // may race to compute f(key) and update the map.
        memo.mu.Lock()
        memo.cache[key] = res
        memo.mu.Unlock()
    }
    return res.value, res.err
}
```

> 性能再度得到了提升, 但我们注意到某系 URL 被获取了两次. 在两个或者多个 goroutine 几乎同时调用 Get 来获取同一个 URL 时就会出现这个问题. 两个 goroutine 都首先查询缓存, 发现缓存中没有需要的数据, 然后调用那个慢函数 f, 最后又都用获取的结果来更新 map, 其中一个结果会被另一个覆盖.  
> 在理想情况下我们应该避免这种额外的处理. 这个功能有时称为重复抑制 (duplicate suppression). 在下面的 Memo 版本中, map 的每一个元素是指向一个通道 ready. 在设置 entry 的 result 字段后, 通道会关闭, 正在等待的 goroutine 会收到广播, 然后就可以从 entry 读取结果了.

```go

type entry struct {
    res   result
    ready chan struct{} // closed when res is ready
}

func New(f Func) *Memo {
    return &Memo{f: f, cache: make(map[string]*entry)}
}

type Memo struct {
    f     Func
    mu    sync.Mutex // guards cache
    cache map[string]*entry
}

func (memo *Memo) Get(key string) (value interface{}, err error) {
    memo.mu.Lock()
    e := memo.cache[key]
    if e == nil {
        // This is the first request for this key.
        // This goroutine becomes responsible for computing
        // the value and broadcasting the ready condition.
        e = &entry{ready: make(chan struct{})}
        memo.cache[key] = e
        memo.mu.Unlock()
        e.res.value, e.res.err = memo.f(key)
        close(e.ready) // broadcast ready condition
    } else {
        // This is a repeat request for this key.
        memo.mu.Unlock()
        <-e.ready // wait for ready condition
    }
    return e.res.value, e.res.err
}

```

> 现在调用 Get 会先先获取保护 cache map 的互斥锁, 再从 map 中查询一个指向已有 entry 的指针, 如果没有查找到, 就分配并插入一个新的 entry, 最后释放锁. 如果查询的 entry 存在, 那么它的值可能还没准备好 (另一个 goroutine 有可能还在调用慢函数 f), 所以主调 goroutine 就需要等待 entry 准备好才能读取 entry 中的数据, 具体的实现方法就是从 ready 通道读取数据, 这个操作会一直阻塞到通道关闭.  
> 如果要查询的 entry 不存在, 那么当前的 goroutine 就需要新插入一个没有准备好的 entry 到 map 里, 并负责调用慢函数 f, 更新 entry, 最后向其他正在等待的 goroutine 广播数据已经准备完毕的消息.  
> 注意, entry 中的变量 e.res.value 和 e.res.err 被多个 goroutine 共享. 创建 entry 的 goroutine 设置了这两个变量的值, 其他 goroutine 在收到数据准备完毕的广播后开始读取这两个变量. 尽管变量被多个 goroutine 访问, 但此次不需要加上互斥锁. ready 通道的关闭先于其他 goroutine 收到广播事件. 这个情况下数据竞态不存在.  
> 这里的并发, 重复抑制, 非阻塞缓存就完成了.  
> 上面的 Memo 代码使用一个互斥量来保护被多个调用 Get 的 goroutine 访问的 map 变量. 接下来会对比另一种设计, 在新的设计中, map 变量限制在一个监控 goroutine 中, 而 Get 的调用者则不得不改为发送消息.  
> Func, result, entry 的声明与之前一致:

```Go
// Func is the type of the function to memoize.
type Func func(key string) (interface{}, error)
// A result is the result of calling a Func.
type result struct {
value interface{}
err error
}
type entry struct {
res result
ready chan struct{} // closed when res is ready
}
```

> 尽管 Get 的调用者通过这个通道来与监控 goroutine 通信, 但是 Memo 类型现在包含一个通道 requests. 该通道的元素类型是 request. 通过这种数据结构, Get 的调用者向监控 goroutine 发送被记忆函数的参数 (key), 以及一个通道 response, 结果在准备好后就通过 response 通道发回. 这个通道仅会传输一个值.

```go
// A request is a message requesting that the Func be applied to key.
type request struct {
    key      string
    response chan<- result // the client wants a single result
}
type Memo struct{ requests chan request }

// New returns a memoization of f. Clients must subsequently call Close.
func New(f Func) *Memo {
    memo := &Memo{requests: make(chan request)}
    go memo.server(f)
    return memo
}
func (memo *Memo) Get(key string) (interface{}, error) {
    response := make(chan result)
    memo.requests <- request{key, response}
    res := <-response
    return res.value, res.err
}
func (memo *Memo) Close() { close(memo.requests) }
```

> 上面的 Get 方法创建了一个响应 (response) 通道, 放在了请求里边, 然后把它发送给监控 goroutine, 再马上从响应通道中读取.  
> 如下所示, cache 变量被限制在监控 goroutine (即(\*Memo).server) 中. 监控 goroutine 从 request 通道中循环读取, 直到该通道被 Close 方法关闭. 对于每个请求, 它先查询缓存, 如果没找到则创建并插入一个新的 entry.

```go
func (memo *Memo) server(f Func) {
    cache := make(map[string]*entry)
    for req := range memo.requests {
        e := cache[req.key]
        if e == nil {
            // This is the first request for this key.
            e = &entry{ready: make(chan struct{})}
            cache[req.key] = e
            go e.call(f, req.key) // call f(key)
        }
        go e.deliver(req.response)
    }
}
func (e *entry) call(f Func, key string) {
    // Evaluate the function.
    e.res.value, e.res.err = f(key)
    // Broadcast the ready condition.
    close(e.ready)
}
func (e *entry) deliver(response chan<- result) {
    // Wait for the ready condition.
    <-e.ready
    // Send the result to the client.
    response <- e.res
}

```

> 与基于互斥锁的版本类似, 对于指定键的一次请求负责在该键上调用函数 f, 保存结果到 entry 中, 最后通过关闭 ready 通道来广播准备完毕状态. 这个流程通过 (\*entry).call 来实现.  
> 对同一个键的后续请求会在 map 中找到已有的 entry, 然后等待结果准备好, 最后通过响应通道把结果发回给调用 Get 的客户端 goroutine. 其中, call 和 deliver 方法都需要在独立的 goroutine 中运行, 以确保监控 goroutine 能持续处理新请求.  
> 上面的例子展示了可以使用两种方案来构建并发结构: 共享变量并上锁, 或者通信顺序进程 (communicating sequential process), 这两者也都不复杂.  
> 在给定的情况下也许很难判定哪种方案更好, 但了解这两种方案的对照关系是很有价值的. 有时候从一种方案切换到另一种能够让代码更简单.

#### 9.8 goroutine 与线程

_goroutine 与 操作系统 (OS) 线程的差异, 本质上属于量变, 但一个足够大的量变会变成质变._

##### 9.8.1 可增长的栈

_每个 OS 线程都有一个固定大小的栈内存 (通常为 2MB), 栈内存区域用于保存在其他函数调用期间那些正在执行或者临时暂停的函数中的局部变量. 这个固定的栈大小既太大又太小. 对于一个小的 goroutine, 2MB 的栈是一个巨大的浪费, 比如有的 goroutine 仅仅等待一个 WaitGroup 再关闭一个通道. 在 Go 程序中, 一次创建十万左右的 goroutine 也不罕见, 对于这种情况, 这栈就太大了. 另外, 对于复杂和深度递归的函数, 固定大小的栈始终不够大. 改变这个固定大小可以提高空间效率并允许创建更多的线程, 或者也可以容许更深的递归函数, 但无法同时做到上面两点._  
_作为对比, 一个 goroutine 在生命周期开始时只有一个很小的栈, 典型情况下为 2KB. 与 OS 线程类似, goroutine 的栈也用于存放那些正在执行或临时暂停的函数中的局部变量. 但与 OS 线程不同的是, goroutine 的栈不是固定大小的, 它可以按照需要增大和缩小. goroutine 的栈大小限制可以达到 1GB, 比线程典型固定大小栈高几个数量级. 当然, 只有极少的 goroutine 会使用这么大的栈._

##### 9.8.2 goroutine 调度

_OS 线程由 OS 内核来调度. 每隔几毫秒, 一个硬件时钟中断发到 CPU, CPU 调用一个叫调度器的内核函数. 这个函数暂停当前正在运行的线程, 把它的寄存器信息保存到内存, 查看线程列表并决定接下来运行哪一个线程, 再从内存恢复线程的注册表信息, 最后继续执行选中的线程. 因为 OS 线程由内核来调度, 所以控制权限从一个线程到另外一个线程需要一个完整的上下文切换 (context switch): 即保存一个线程的状态到内存, 再恢复另外一个线程状态, 最后更新调度器数据结构. 考虑这个操作涉及的内存局部性以及涉及的内存访问数量, 还有访问内存所谓的 CPU 周期数量的增加, 这个操作其实是很慢的._
_Go 运行时包含一个自己的调度器, 这个调度器使用一个称为 m:n 调度技术 (因为它可以复用/调度 m 个 goroutine 到 n 个 OS 线程). Go 调度器与内核调度器的工作类似, 但 Go 调度器只关心单个 Go 程序的 goroutine 调度问题._  
_与操作系统的线程不同的是, Go 调度器不是由硬件时钟定期触发的, 而是由特定的 Go 语言结构来触发的. 比如当一个 goroutine 调用 time.Sleep 或被通道阻塞或对互斥量操作时, 调度器就会将这个 goroutine 设置为休眠模式, 并运行其他 goroutine 直到前一个可重新唤醒为止. 因为不需要切换到内核语境, 所以调用一个 goroutine 比调度一个线程成本低很多._

##### 9.8.3 GOMAXPROCS

_Go 调度器使用 GOMAXPROCS 参数来确定使用多少个 OS 线程同时来执行 Go 代码. 默认值时机器上的 CPU 数量, 所以在一个有 8 个 CPU 的机器上, 调度器会把 Go 代码同时调度到 8 个 OS 线程上. (GOMAXPROCS 是 m:n 调度中的 n.) 正在休眠或者正被通道阻塞的 goroutine 不需要占用线程. 阻塞在 I/O 和其他系统调用中或调用非 Go 语言写的函数的 goroutine 需要一个独立的 OS 线程, 但这个线程不计算在 GOMAXPROCS 内._
_可以用 GOMAXPROCS 环境变量或者 runtime.GOMAXPROCS 函数来显示控制这个参数. 可以用一个小程序来看看 GOMAXPROCS 的效果, 这个程序无止境的输出 0 和 1._

```Go
for {
go fmt.Print(0)
fmt.Print(1)
}
 GOMAXPROCS=1 go run hacker-cliché.go
111111111111111111110000000000000000000011111...
 GOMAXPROCS=2 go run hacker-cliché.go
010101010101010101011001100101011010010100110...
```

> 在第一次运行时, 每次最多只能有一个 goroutine 运行. 最开始是主 goroutine, 它输出 1. 在一段时间后, Go 调度器让主 goroutine 休眠, 并且唤醒另外一个输出 0 的 goroutine, 让它有机会执行. 在第二次运行时, 这里可以有两个可用的 OS 线程, 所以两个 goroutine 可以同时运行, 以一个差不多的速率输出两个数字. 我们必须强调影响 goroutine 调度的隐式

##### 9.8.4 goroutine 没有标志

_在大部分支持多线程的操作系统和编程语言里, 当前线程都用一个独特的标识, 它通常可以取一个整数或者指针. 这个特性让我们可以轻松构建一个线程的局部存储, 它本质上就是一个全局的 map, 以线程的标识作为键, 这样每个线程都可以独立地用这个 map 存储和获取值, 而不受其他线程干扰._  
_goroutine 没有可提供程序员访问地标识. 这个是由设计决定地, 因为线程局部存储有一种被滥用地倾向. 比如, 当一个 Web 服务器用一个支持线程局部存储地语言来实现时, 很多函数都会通过访问这个存储来查找关于 HTTP 请求地信息. 但就像那些过度依赖于全局变量地程序一样, 这也会导致一种不健康地 "超距作用", 即函数地行为不仅取决于它地参数, 还取决于运行它地线程标识. 因此, 在线程地标识需要改变地场景 (比如需要使用工作线程时), 这些函数地行为就会变得诡异莫测._
_Go 语言鼓励一种简单地编程风格, 其中, 能影响一个函数地参数应当是显示指定的. 这不仅让程序更易阅读, 还让我们能自由地把一个函数地子任务分发到多个不同地 goroutine 而无需担心这些 goroutine 地标识._

### 第 10 章 包和 go 工具

_Go 配套的 go 工具, 一个复杂但是容易使用的命令行工具, 用来管理 Go 包的工作空间._

#### 10.1 引言

_任何包管理系统的目的都是通过关联的特性分类, 组织成便于理解和修改的单元, 使其与程序的其他包保持独立, 从而有助于设计和维护大型的程序. 模块化允许包在不同的项目中共享, 复用, 在组织中发布, 或者在全世界范围内使用._  
_每个包定义了一个不同的命名空间作为它的标识符. 每个名字关联一个具体的包, 它让我们在为类型, 函数等选取短小而且清晰的名字的同时, 不与程序其他部分冲突._  
_包通过控制名字是否导出使其对包外可见来提供封装能力. 限制包成员的可见性, 从而隐藏 API 后面的辅助函数和类型, 允许包的维护者修改包的实现而不影响包外部的代码. 限制变量的可见性也可以隐藏变量, 这样使用者仅可以通过导出函数来对其访问和更新, 它们可以保留自己的不变量以及并发程序中实现互斥访问._  
_当我们修改一个文件时, 我们必须重新编译文件所在的包和所有潜在的依赖包. 众所周知, Go 程序的编译比其他语言要快, 即便从零开始编译也如此. 这里有三个主要原因. 第一, 所有的导入都必须在每一个源文件的开头显示列出, 这样编译器在确定依赖性的时候就不需要读取和处理整个文件; 第二, 包的依赖性形成有向无循环图, 因为没有环, 所以包可以独立甚至并行编译. 第三, Go 包编译输出的目标文件不仅记录它自己的导出信息, 还记录它所依赖包的导出信息. 当编译一个包时, 编译器必须从每一个导入中读取一个目标文件, 但是不会超出这些文件._

#### 10.2 导入路径

_每一个包都通过一个唯一的字符串进行标识, 它称为导入路径, 它们用在 import 声明中._

```Go
import (
"fmt"
"math/rand"
"encoding/json"
"golang.org/x/net/html"
"github.com/go-sql-driver/mysql"
)
```

> Go 语言的规范没有定义字符串的含义或如何确定一个包的导入路径, 它通过工具来解决这些问题. go 工具也是 Go 程序员用来构建, 测试程序的主要工具, 尽管还有其他工具的存在.  
> 对于准备共享或公开的包, 导入路径需要全局唯一. 为了避免冲突, 除了标准库中的包之外, 其他包的导入路径应该以互联网域名 (组织机构拥有的域名或用于存放包的域名) 作为路径开始, 这样也方便查找包.

#### 10.3 包的声明

_在每一个 Go 源文件的开头都需要进行包声明. 主要的目的是当该包被其他包引入的时候作为其默认的标识符 (称为包名)._  
_例如, math/rand 包中每一个文件的开头都是 package rand, 这样当你导入这个包时, 可以访问它的成员, 比如 rand.Int, rand.Float64 等._

```Go
package main
import (
"fmt"
"math/rand"
)
func main() {
fmt.Println(rand.Int())
}
```

> 通常, 包名是导入路径的最后一段, 于是, 即使导入路径不同的两个包, 二者也可以拥有同样的名字. 例如, 两个包的导入路径分别是 math/rand 和 crypto/rand, 而包的名字都是 rand. 它们可以在同一个程序中使用它们.  
> 关于 "最后一段" 的惯例, 这个有三个例外. 第一个另外是: 不管包的导入路径是什么, 如果该包定义一条命令 (可执行的 Go 程序), 那么它总是使用名称 main. 这是告诉 go build 的信号, 它必须调用连接器生成可执行文件.  
> 第二个例外是: 目录中可能有一些文件以 \_test.go 结尾, 包名中会出现以\_test 结尾. 这样一个目录中有两个包: 一个普通, 加上一个外部测试包. \_test 后缀告诉 go test 两个包都需要构建, 并且指明文件属于哪个包. 外部测试包用来避免测试所依赖的导入图中的循环依赖.  
> 第三个例外是: 有一些依赖管理工具会在包导入路径的尾部追加版本号后缀, 如 "gopkg.in/yaml.v2". 包名不包含后缀, 因此这个情况下包名为 yaml.

#### 10.4 导入声明

一个 Go 源文件可以在 package 声明的后面和第一个非导入声明语句前面紧接着包含零个或多个 import 声明. 每一个导入可以单独指定一条导入路径, 也可以通过圆括号括起来的列表一次导入多个包. 下面两种形式是等价的, 但第二种形式更常见.

```Go
import "fmt"
import "os"
import (
"fmt"
"os"
)
```

导入的包可以通过空行进行分组; 这类分组通常表示不同领域和方面的包. 导入顺序不重要, 但按照惯例每一组都按照字母进行排序. (gofmt 和 goimports 工具都会自动进行分组并排序.)

```go
import (
"fmt"
"html/template"
"os"
"golang.org/x/net/html"
"golang.org/x/net/ipv4"
)
```

_如果需要把两个名字一样的包 (如 match/rand 和 crypto/rand) 导入到第三个包中, 导入声明就必须至少为其中的一个指定一个替代名字来避免冲突. 这叫作重命名导入._

```go
import (
"crypto/rand"
mrand "math/rand" // alternative name mrand avoids conflict
)
```

> 替代名字仅影响当前文件. 其他文件 (即便是同一个包中的文件) 可以使用默认名字来导入包, 或者一个替代名字也可以.  
> 重命名导入在没有冲突时也是非常有用的. 如果有时用到自动生成的代码, 导入的包名非常冗长, 使用一个替代名字可能更方便. 同样的缩写名字要一直用下去, 以避免产生混淆. 使用一个替代名字有助于规避常见的局部变量冲突. 例如, 如果一个文件可以包含许多以 path 命名的变量, 我们就可以使用 pathpkg 这个名字导入一个标准的 "path" 包.  
> 每个导入声明从当前包向导入的包建立一个依赖. 如果这些依赖形成一个循环, go build 工具就会报错.

#### 10.5 空导入

_如果导入的包的名字没有在文件中引用, 就会产生一个编译错误. 但是, 有时候, 我们必须导入一个包, 这仅仅是为了利用其副作用: 对包级别的变量执行初始化表达式求值, 并执行它的 init 函数. 为了防止 "未使用的导入" 错误, 我们必须使用一个重命名导入, 它使用一个替代的名字 \_, 这表示导入的内容为空白标识符. 通常情况下, 空白标识不可能被引用._

```go
import _ "image/png" // register PNG decoder
```

> 这称为空白导入. 多数情况下, 它用来实现一个编译时的机制, 使用空白引入导入额外的包, 来开启主程序中可选的特性. 首先我们来看如何使用它, 然后看它是如何工作的.
> 标准库地 image 包导出了 Decode 函数, 它从 io.Reader 读取数据, 并且识别使用哪一种图像格式来编码数据, 调用适当地解码器, 返回 image.Image 对象作为结果. 使用 image.Decode 可以构建一个简单地图像转换器, 读取某一种格式图像, 然后输出为另外一个格式:

```Go
// The jpeg command reads a PNG image from the standard input
// and writes it as a JPEG image to the standard output.
package main
import (
"fmt"
"image"
"image/jpeg"
_ "image/png" // register PNG decoder
"io"
"os"
)
func main() {
    if err := toJPEG(os.Stdin, os.Stdout); err != nil {
        fmt.Fprintf(os.Stderr, "jpeg: %v\n", err)
        os.Exit(1)
    }
}
func toJPEG(in io.Reader, out io.Writer) error {
    img, kind, err := image.Decode(in)
    if err != nil {
        return err
    }
    fmt.Fprintln(os.Stderr, "Input format =", kind)
    return jpeg.Encode(out, img, &jpeg.Options{Quality: 95})
}
```

> 注意空白导入 image/png. 没有这一行, 程序可以正常编译和链接, 但是不能识别和解码 PNG 格式输入.  
> 这里解释它是如何工作地. 标准库提供 GIF, PNG, JPEG 等格式地解码库, 用户自己可以提供其他格式地, 但是为了使可执行程序简短, 除非明确需要, 否则解码器不会被包含进应用程序. image.Decode 函数查询支持地表格. 每一个表项由 4 个部分组成: 格式地名字; 某种格式中使用地相同地前缀字符串, 用来识别编码格式; 一个用来解码被编码地函数 Decode; 以及另一个函数 DecodeConfig, 它仅仅解码图像地元数据, 比如尺寸和色域. 对于每一种格式, 通常通过在其支持地包地初始化函数中来调用 image.RegisterFormat 来向表格添加项. 例如 image/png 中地实现如下:

```Go
package png // image/png
func Decode(r io.Reader) (image.Image, error)
func DecodeConfig(r io.Reader) (image.Config, error)
func init() {
    const pngHeader = "\x89PNG\r\n\x1a\n"
    image.RegisterFormat("png", pngHeader, Decode, DecodeConfig)
}
```

> 这个效果就是, 一个应只需要空白导入格式化所需要地包, 就可以让 image.Decode 函数具备对应格式地解码能力.  
> database/sql 包使用类似地机制让用户按需求加入想要地数据库驱动程序. 例如:

```Go
import (
"database/mysql"
_ "github.com/lib/pq"              // enable support for Postgres
_ "github.com/go-sql-driver/mysql" // enable support for MySQL
)
db, err = sql.Open("postgres", dbname) // OK
db, err = sql.Open("mysql", dbname) // OK
db, err = sql.Open("sqlite3", dbname) // returns error: unknown driver "sqlite3"

```

#### 10.6 包及其命名

_本节将提供一些建议, 指出如何遵从 Go 地习惯来给包和它地成员进行命名._

- 当创建一个包时, 使用简短地名字, 但是不要短到像加密了一样. 在标准库中最常见地包包括: bufio, bytes, flag, fmt, http, io, json, os, sort, sync 和 time 等.
- 尽可能保持可读性和无歧义. 例如, 不要把一个辅助工具包命名为 util, 使用 imageutil 或 ioutil 等名称更具体和清晰. 避免选择经常用于相关局部变量的包名, 或者迫使使用者使用重命名导入, 例如使用以 path 命名地包.
- 包名通常使用统一地形式. 标准包 bytes, errors 和 strings 使用复数来避免覆盖响应的预声明类型, 使用 go/types 这个形式, 来避免和关键字冲突.
- 避免使用有其他含义的包名.
  _现在讨论成员的命名. 因为对其他包成员的每一个引用使用一个具体的标识符, 例如 fmt.Println, 描述包的成员和描述包名与成员名同样复杂. 我们不需要在 Println 中引用格式化的概念, 因为包名 fmt 还没有准备好. 当设计一个包时, 要考虑两个有意义的部分如何一起工作, 而不只是成员名. 这里有一些具体的例子:_

```Go
bytes.Equal flag.Int http.Get json.Marshal
```

_我们可以识别出一些通用的命名模式. strings 包提供一系列操作字符串的独立函数:_

```go
package strings
func Index(needle, haystack string) int
type Replacer struct{ /* ... */ }
func NewReplacer(oldnew ...string) *Replacer
type Reader struct{ /* ... */ }
func NewReader(s string) *Reader
```

> string 这个词不会出现在任何名字中. 客户通过 strings.Index, strings.Replacer 等引用它们.  
> 其他的一些包可以描述为单一类型包, 例如 html/template 和 math/rand, 这些包导出一个数据类型及其方法, 通常有一个 New 函数用来创建实例.

```go
package rand // "math/rand"
type Rand struct{ /* ... */ }
func New(source Source) *Rand
```

> 这可能造成重复, 例如 template.Template 或 rand.Rand 中, 这也是为什么这类包名通常都比较短.

#### 10.7 go 工具

_go 工具讲不同种类的工具集合并为一个命名集. 它是一个包管理器 (类似于 apt 或 rpm), 它可以查询包的作者, 计算它们的依赖关系, 从远程版本控制系统下载它们. 它是一个构建系统, 可计算文件依赖, 调用编译器, 汇编器和链接器._  
_最常用命令:_

```bash
build compile packages and dependencies
clean remove object files
doc show documentation for package or symbol
env print Go environment information
fmt run gofmt on package sources
get download and install packages and dependencies
install compile and install packages and dependencies
list list packages
run compile and run Go program
test test packages
version print Go version
vet run go tool vet on packages
Use "go help [command]" for more information about a command.
```

> 为了让配置最小化, go 工具非常依赖惯例.

##### 10.7.1 工作空间的组织

_大部分用户必须进行的唯一的配置是 GOPATH 环境变量, 它指定工作空间的根. 当需要切换到不同的工作空间时, 更新 GOPATH 变量的值即可._
_GOPATH 有三个子目录. src 子目录包含源文件. 每一个包放在一个目录中, 该目录相对于 GOPATH/src 的名字是包的导入路径. 注意, 一个 GOPATH 工作空间在 src 下包含多个源码控制仓库, 例如 gopl.io 或 golang.org. pkg 子目录是构建工具存储编译后的包的位置, bin 子目录放置可执行程序._  
_第二个环境变量是 GOROOT, 它指定 Go 发行版的根目录, 其中提供所有标准库的包. GOROOT 下面的目录结构类似于 GOPATH, 这样 fmt 包的源码放在 GOROOT/src/fmt 目录中. 用户无须设置 GOROOT, 因为默认情况下 go 工具使用它的安装路径._  
_go env 命令输出与工具链相关的已经设置有效值的环境变量及其所设置值, 还会输出未设置的有效环境变量及其默认值. GOOS 指定目标操作系统, GOARCH 指定目标处理器架构. GOPATH 是一个必须设置的变量._

##### 10.7.2 包的下载

_go get 命令可以下载单一的包, 也可以使用 ... 符号来下载子树或仓库, 该工具也计算并下载初始包所有的依赖性. 在完成包的下载后, 它会构建它们, 然后安装库和相应的命令. 下面的第一条命令获取 golint 工具, 它用来检查 Go 源码中的风格问题. 第二条命令执行 golint 来检查示例代码. 它会报告我们忘了给这个包写注释:_

```go
go get github.com/golang/lint/golint
GOPATH/bin/golint gopl.io/ch2/popcount
src/gopl.io/ch2/popcount/main.go:1:1:
package comment should be of the form "Package popcount ..."
```

_go get 命令已经支持多个流行代码托管站点, 如 GitHub, Bitbucket 和 Launchpad, 并且可以向版本控制系统发出合适请求. 对于不知名的网站, 你也许需要指出导入路径使用的是哪种版本控制协议. 执行 go help importpath 来获取更多细节._  
_go get 创建的目录是远程仓库的真实客户端, 而不仅仅是文件的副本, 这样可以使用版本控制命令来检查本地编辑的差异或者更新到不同版本._

```bash
cd GOPATH/src/golang.org/x/net
git remote -v
origin https://go.googlesource.com/net (fetch)
origin https://go.googlesource.com/net (push)
```

_注意, 包导入路径中明显的域名 (golang.org) 不同于 Git 服务器的实际域名 (go.googlesource.com). 这是 go 工具的一个特性, 如果位置由诸如 googlesource.com 或者 github.com 之类通用服务托管, 包可以在其导入路径中使用自定义域名. 在 <https://golang.org/x/net/html> 下面的 HTML 网页包含如下元数据, 它重定向 go 工具到实际托管地址的 Git 仓库:_

```go
 go build gopl.io/ch1/fetch
 ./fetch https://golang.org/x/net/html | grep go-import

```

_如果指定 -u 开关, go get 将确保它访问的所有包 (包括它们的依赖性) 更新到最新版本, 然后再构建和安装. 如果没有这个标记, 已经存在于本地的包不会更新._  
_在需要部署的项目中 (其中, 发布版本需要精准的版本控制), 就不太适合使用它. 通常的解决方案是加一层 vendor 目录, 构建一个关于所有必须依赖的本地副本, 然后非常小心的更新这个副本. 在 Go 1.5 之前, 这需要改变包的导入路径, 这样 golang.org/x/net/html 的副本 会变成 gopl.io/vendor/golang.org/x/net/html. 几乎所有最近版本的 go 工具都支持加 vendor 目录, 使用 go help gopath 来查看 vendor 目录的详细信息._

##### 10.7.3 包的构建

_go build 命令编译每一个命令行参数中的包. 如果包是一个库, 结果会被舍弃; 对于没有编译错误的包几乎不做检查. 如果包中的名字是 mian, go build 调用链接器在当前目录中创建可执行程序, 可执行程序的名字取自包的导入路径的最后一段._  
_每一个目录包含一个包, 每一个可执行程序或者 UNIX 命令都需要自己的目录. 这些目录可能是 cmd 目录的子目录._  
_如前所述, 包可以指定通过目录来指定, 可以使用导入路径或者一个相对目录名, 目录必须以 . 或 .. 开头, 即使这个不经常需要. 如果没有提供参数, 会使用当前目录作为参数. 所以, 以下命令:_

```go
 cd GOPATH/src/gopl.io/ch1/helloworld
 go build
```

_和以下命令:_

```go
 cd anywhere
 go build gopl.io/ch1/helloworld
```

_以及以下命令:_

```go
 cd GOPATH
 go build ./src/gopl.io/ch1/helloworld
```

_构建同样的包 (尽管每次写入的目录是 go build 命令运行时所在的目录). 但以下命令编译不同的包:_

```go
 cd GOPATH
 go build src/gopl.io/ch1/helloworld
Error: cannot find package "src/gopl.io/ch1/helloworld".
```

_包也可以使用一个文件列表来指定 (尽管这只是针对小型程序和一次性的实验). 如果包名时 mian, 可以执行程序的名字来自第一个 .go 文件名的主体部分._

```go
 cat quoteargs.go
package main
import (
"fmt"
"os"
)
func main() {
fmt.Printf("%q\n", os.Args[1:])
}
 go build quoteargs.go
 ./quoteargs one "two three" four\ five
["one" "two three" "four five"]
```

_特别是对于这类即用即抛的程序, 我们需要在构建之后尽快运行. go run 命令将这两部合并起来:_

> go run quoteargs.go one "two three" four\ five  
> ["one" "two three" "four five"]

_第一个不是 .go 文件结尾的参数会作为 Go 可执行程序的参数列表的开始._

_默认情况下, go build 命令构建所有需要的包以及它们所有的依赖性, 然后丢弃除了最终可以执行之外的所有编译后的代码. 依赖性分析和编译本身非常快, 但是当项目增长到数十个包和数十万行代码的时候, 重新编译依赖性的时间明显变慢,也许数秒钟的时间, 即使依赖的部分根本没有改变过._  
_go install 命令和 go build 非常相似, 区别是它会保存每一个包的编译代码和命令, 而不是把它们丢弃. 编译后的包保存在 GOPATH/pkg 目录中, 它对应于存放源文件的 src 目录, 可以执行的命令保存在 GOPATH/bin 目录中. 这样, go build 和 go install 对于没有改变的包和命令不需要重新编译, 从而使后续的构建更加快速. 惯例上, go build -i 可以将包安装在独立于构建目标的地方._  
_因为编译包根据操作系统平台和 CPU 体系结构不同而不同, 所以 go install 将保存它们的命令命名为与 GOOS 和 GOARCH 变量的值相关. 例如, 在 Mac 上面 golang.org/x/net/html 编译后的文件 on a Mac the golang.org/x/net/html.a 放在 GOPATH/pkg/darwin_amd64 目录下面._

```go
func main() {
fmt.Println(runtime.GOOS, runtime.GOARCH)
}
```

_下面的命令分别生成 64 位和 32 位的可执行程序:_

```go
 go build gopl.io/ch10/cross
 ./cross
darwin amd64
 GOARCH=386 go build gopl.io/ch10/cross
 ./cross
darwin 386
```

_例如, 为了处理底层可移植性问题或为重要的例程提供优化版本, 有一些包需要为特定的平台或者处理器编译不同版本的代码. 如果一个文件名包含操作系统或处理器处理体系结构名字 (如 net_linux.go 或 asm_amd64.s), go 工具只会在构建指定规格文件的时候才进行编译. 叫作构建标签的特殊注释, 提供更细粒度的控制. 例如, 如果一个文件包含下面的注释:_

> // +build linux darwin
> _注释在包声明之前 (它是文档注释), go build 只会在构建 Linux 或 Mac OS X 系统应用的时候才会对它进行编译, 下面的注释指出任何时候都不要编译这个文件:_  
> // +build ignore

_更多细节可以在 go/build 包的文档中的 Build Constraints 节找到:_

> go doc go/build

##### 10.7.4 包的文档化

Go 风格强烈鼓励有良好的包 API 文档. 每一个导出的包成员的声明以及包声明自身应该立刻使用注释来描述它的目的和用途.  
Go 文档注释总是完整的语句, 使用声明的包名作为开头的第一句注释通常是总结. 函数参数和其他的标识符无须括起来或者特别标注. 例如, fmt.Fprintf 的文档注释如下:

```go
// Fprintf formats according to a format specifier and writes to w.
// It returns the number of bytes written and any write error encountered.
func Fprintf(w io.Writer, format string, a ...interface{}) (int, error)
```

Fprintf 的格式化细节在 fmt 包自身的文档注释中进行解释. 包声明的前面的文档注释被认为是整个包的文档注释. 尽管它可以出现在任何文件中, 但是必须只有一个. 比较长的包注释可以使用一个单独的注释文件, fmt 的注释超过 300 行, 文件名通常叫 doc.go.  
简明是文档一个不可替代的优点. 事实上, Go 在文档保持简练和简单方面的惯例和其他所有的东西一样, 因为文档像代码一样也需要维护. 许多声明可以在一个通顺的句子中解释清楚, 并且如果这个行为非常明确, 就不需要注释.  
go doc 工具输出在命令行上指定的内容的声明和整个文档注释, 这也许是一个包:

```go
 go doc time
package time // import "time"
Package time provides functionality for measuring and displaying time.
const Nanosecond Duration = 1 ...
func After(d Duration) <-chan Time
func Sleep(d Duration)
func Since(t Time) Duration
func Now() Time
type Duration int64
type Time struct { ... }
...many more...
```

或者是一个包成员:

```go
 go doc time.Since
func Since(t Time) Duration
Since returns the time elapsed since t.
It is shorthand for time.Now().Sub(t).
```

或者是一个方法:

```go
 go doc time.Duration.Seconds
func (d Duration) Seconds() float64
Seconds returns the duration as a floating-point number of seconds.
```

该工具不需要完整导入路径或者正确的标识符. 这个命令输出来自 encoding/json 包的 (\*json.Decoder).Decode 的文档:

```go
 go doc json.decode
func (dec *Decoder) Decode(v interface{}) error
Decode reads the next JSON-encoded value from its input and stores
it in the value pointed to by v.
```

第二个工具名字叫 godoc, 它提供互相链接的 HTML 页面服务, 进而提供不少于 go doc 命令的信息. 在 <https://golong.org/pkg> 的 godoc 服务器覆盖了标准库.
如果想浏览自己的包, 你可以在你的工作空间中运行一个 godoc 实例. 在执行下面的命令后, 在浏览器中访问 <http://localhost:8000/pkg>

```go
 godoc -http :8000
```

加上 -analysis=type 和 -analysis=pointer 标记使文档内容丰富, 同时提供源代码的高级静态分析结果.

##### 10.7.5 内部包

这是用包来封装 Go 程序最重要的机制. 没有导出的标识符只能在同一个包内访问, 导出的标识符可以在世界的任何地方访问.  
go build 工具会特殊对待导入路径中包含路径片段 internal 的情况. 这些包叫内部包. 内部包只能被另一个包导入, 这个包位于以 internal 目录的父目录为根目录的树中. 例如, 给定下面的包, net/http/internal/chunked 可以从 net/http/httputil 或 net/http 导入, 但是不能从 net/url 导入. 然而, net/url 可以导入 net/http/httputil.

```go
net/http
net/http/internal/chunked
net/http/httputil
net/url
```

##### 10.7.6 包的查询

go list 工具上报包的可用信息. 通过最简单的形式, go list 判断一个包是否存在于工作工作空间中, 如果存在输出它的导入路径:

```go
 go list github.com/go-sql-driver/mysql
github.com/go-sql-driver/mysql
```

go list 命令的参数可以包含 " ... " 通配符, 它用来匹配包导入路径中的任意子串. 可以使用它枚举一个 Go 工作空间中所用的包.

> go list ...
> archive/tar
> archive/zip
> bufio
> bytes
> cmd/addr2line
> cmd/api
> ...many more...

或者一个指定的子树中的所有包:

> go list gopl.io/ch3/...
> gopl.io/ch3/basename1
> gopl.io/ch3/basename2
> gopl.io/ch3/comma
> gopl.io/ch3/mandelbrot
> gopl.io/ch3/netflag
> gopl.io/ch3/printints
> gopl.io/ch3/surface

或者一个具体主题:

> go list ...xml...
> encoding/xml
> gopl.io/ch7/xmlselect

go list 命令获取每一个包的完整元数据, 而不仅仅是导入路径, 并且提供各种对于用户或者其他工具可访问的格式. -json 标记使 go list 以 JSON 格式输出每一个包的完整记录:

```go
 go list -json hash
{
"Dir": "/home/gopher/go/src/hash",
"ImportPath": "hash",
"Name": "hash",
"Doc": "Package hash provides interfaces for hash functions.",
"Target": "/home/gopher/go/pkg/darwin_amd64/hash.a",
"Goroot": true,
"Standard": true,
"Root": "/home/gopher/go",
"GoFiles": [
"hash.go"
],
"Imports": [
"io"
],
"Deps": [
"errors",
"io",
"runtime",
"sync",
"sync/atomic",
"unsafe"
]
}
```

-f 标记可以让用户通过 text/template 包提供的模板语言来定制输出格式. 这个命令输出 strconv 包的依赖过渡关系记录, 记录之间由空格分割:

```go
 go list -f '{{join .Deps " "}}' strconv
errors math runtime unicode/utf8 unsafe
```

这条命令输出标准库的 compress 子树中每个包的直接导入记录:

```go
 go list -f '{{.ImportPath}} -> {{join .Imports " "}}' compress/...
compress/bzip2 -> bufio io sort
compress/flate -> bufio fmt io math sort strconv
compress/gzip -> bufio compress/flate errors fmt hash hash/crc32 io time
compress/lzw -> bufio errors fmt io
compress/zlib -> bufio compress/flate errors fmt hash hash/adler32 io
```

go list 对于一次性的交付查询和构建, 测试脚本都非常有用. 更多的参数以及它们的含义等信息, 可以通过执行 go help list 来获取.

### 第 11 章 测试

_测试是 自动化测试的简称, 即编写简单的程序来确保程序 (产品代码) 在该测试中针对特定输入产生预期的输出._

#### 11.1 go test 工具

go test 子命令是 Go 语言包测试驱动程序, 这些包根据某些约定组织在一起. 在一个包目录中, 以 \_test.go 结尾的文件不是 go build 命令编译的目标, 而是 go test 编译的目标.  
在 _\_test.go 文件中, 三种函数需要特殊对待, 即功能测试函数, 基准测试函数和示例函数. 功能测试函数是以 Test 前缀命名的函数, 用来检测一些程序逻辑的正确性, go test 运行测试函数, 并且报告结果是 PASS 还是 FAIL. 基准测试函数的名称以 Benchmark 开头, 用来测试某些操作的性能, go test 汇报操作的平均执行时间. 示例函数的名称, 以 Example 开头, 用来提供机器检查过文档.  
go test 工具扫描_\_test.go 文件来找特殊函数, 并生成一个临时的 main 包来调用它们, 然后编译和运行, 并汇报结果, 最后清空临时文件.

#### 11.2 Test 函数

每一个测试文件必须导入 testing 包. 这些函数的签名如下:

```go
func TestName(t *testing.T) {
// ...
}
```

功能测试函数必须以 Test 开头, 可选的后缀名称必须以大写字母开头:

```go
func TestSin(t *testing.T) { /* ... */ }
func TestCos(t *testing.T) { /* ... */ }
func TestLog(t *testing.T) { /* ... */ }
```

参数 t 提供了汇报测试失败和日志记录的功能. 定义一个函数 IsPalindrome, 用来判断是否回文字符串.

```go
// Package word provides utilities for word games.
package word
// IsPalindrome reports whether s reads the same forward and backward.
// (Our first attempt.)
func IsPalindrome(s string) bool {
for i := range s {
if s[i] != s[len(s)-1-i] {
return false
}
}
return true
}
```

在同一个目录中, 文件 word_test.go 包含了两个功能测试函数 TestPalindrome 和 TestNonPalindrome. 两个函数都检查 isPalindrome 是否针对单个输入参数给出了正确的结果.

```go
package word
import "testing"
func TestPalindrome(t *testing.T) {
    if !IsPalindrome("detartrated") {
        t.Error(`IsPalindrome("detartrated") = false`)
    }
    if !IsPalindrome("kayak") {
        t.Error(`IsPalindrome("kayak") = false`)
    }
}
func TestNonPalindrome(t *testing.T) {
    if IsPalindrome("palindrome") {
        t.Error(`IsPalindrome("palindrome") = true`)
    }
}
```

go test (或者 go build) 命令在不指定包参数的情况下, 以当前目录所在的包为参数. 可以用下面的命令来编译和运行测试:

```go
 cd GOPATH/src/gopl.io/ch11/word1
 go test
ok gopl.io/ch11/word1 0.008s
```

程序存在 bug:

```go
func TestFrenchPalindrome(t *testing.T) {
    if !IsPalindrome("été") {
        t.Error(`IsPalindrome("été") = false`)
    }
}
func TestCanalPalindrome(t *testing.T) {
    input := "A man, a plan, a canal: Panama"
    if !IsPalindrome(input) {
        t.Errorf(`IsPalindrome(%q) = false`, input)
    }
}
```

添加这两个测试之后, go test 命令失败了, 给出如下错误信息:

```go
 go test
--- FAIL: TestFrenchPalindrome (0.00s)
word_test.go:28: IsPalindrome("été") = false
--- FAIL: TestCanalPalindrome (0.00s)
word_test.go:35: IsPalindrome("A man, a plan, a canal: Panama") = false
FAIL
FAIL gopl.io/ch11/word1 0.014s
```

比较号的实践是先写触发错误和用户 bug 报告里面的一致. 只有这个时候, 我们才确信我们修复的内容是针对这个出现的问题的.  
另外, 运行 go test 比手动测试 bug 报告中的内容快的多, 测试可以让我们顺序地检查内容. 如果一个测试套件 (test suite) 里面有很多测试用例, 我们可以选择性地测试用例来加加测试过程.  
选项 -v 可以输出包中每个测试用例地名称和执行时间:

```go
 go test -v
=== RUN TestPalindrome
--- PASS: TestPalindrome (0.00s)
=== RUN TestNonPalindrome
--- PASS: TestNonPalindrome (0.00s)
=== RUN TestFrenchPalindrome
--- FAIL: TestFrenchPalindrome (0.00s)
word_test.go:28: IsPalindrome("été") = false
=== RUN TestCanalPalindrome
--- FAIL: TestCanalPalindrome (0.00s)
word_test.go:35: IsPalindrome("A man, a plan, a canal: Panama") = false
FAIL
exit status 1
FAIL gopl.io/ch11/word1 0.017s
```

选项 -run 地参数是一个正则表达式, 它可以使得 go test 只运行那些测试函数名称匹配给定模式地函数:

```go
 go test -v -run="French|Canal"
=== RUN TestFrenchPalindrome
--- FAIL: TestFrenchPalindrome (0.00s)
word_test.go:28: IsPalindrome("été") = false
=== RUN TestCanalPalindrome
--- FAIL: TestCanalPalindrome (0.00s)
word_test.go:35: IsPalindrome("A man, a plan, a canal: Panama") = false
FAIL
exit status 1
FAIL gopl.io/ch11/word1 0.014s
```

Fix bug 后, 重写了这个函数:

```go

// Package word provides utilities for word games.
package word
import "unicode"
// IsPalindrome reports whether s reads the same forward and backward.
// Letter case is ignored, as are non-letters.
func IsPalindrome(s string) bool {
    var letters []rune
    for _, r := range s {
        if unicode.IsLetter(r) {
            letters = append(letters, unicode.ToLower(r))
        }
    }
    for i := range letters {
        if letters[i] != letters[len(letters)-1-i] {
            return false
        }
    }
    return true
}
```

重写测试用例:

```go
func TestIsPalindrome(t *testing.T) {
    var tests = []struct {
        input string
        want  bool
    }{
        {"", true},
        {"a", true},
        {"aa", true},
        {"ab", false},
        {"kayak", true},
        {"detartrated", true},
        {"A man, a plan, a canal: Panama", true},
        {"Evil I did dwell; lewd did I live.", true},
        {"Able was I ere I saw Elba", true},
        {"été", true},
        {"Et se resservir, ivresse reste.", true},
        {"palindrome", false}, // non-palindrome
        {"desserts", false},   // semi-palindrome
    }
    for _, test := range tests {
        if got := IsPalindrome(test.input); got != test.want {
            t.Errorf("IsPalindrome(%q) = %v", test.input, got)
        }
    }
}
```

当前调用 t.Errorf 输出地失败地测试用例信息没有包含整个跟踪栈信息, 也不会导致程序宕机或者终止执行, 这和很多其他语言地测试框架断言不同. 测试用例彼此是独立地. 如果测试表中一个条目造成测试失败, 那么其他条目仍然会继续测试.  
如果我们真的需要终止一个测试函数, 比如由于初始化代码失败或者避免已有地错误产生令人困惑地输出, 我们可以使用 t.Fatal 或者 t.Fatalf 函数来终止测试. 这些函数地调用必须和 Test 函数在同一个 goroutine 中, 而不是在测试创建其他 goroutine 中.  
测试错误信息一般格式是 "f(x)=y, want z", 这里 f(x) 表示需要执行地操作和它地输入, y 是实际地输出结果, z 是期望得到地结果. 出于方便, 对于 f(x) 我们会使用 Go 的语法, 比如在上面回文的例子中, 我们使用 Go 的格式化来显示较长的输入, 避免重复输入. 在基于表的测试中, 输出 x 是 很重要的, 因为一条断言语句会在不同的输入情况下执行多次. 错误消息要避免样板文字和冗余信息. 在测试一个布尔函数的时候, 比如上面的 IsPalindrome, 省略 "want z" 部分, 因为它没有给出有用信息. 如果 x, y, z 都比较长, 可以输出准确代表各部分的概要信息.

##### 11.2.1 随机测试

基于表的测试方便针对精心选择的输入检测函数是否工作正常, 以测试逻辑上引人关注的用例. 另外一种方式是随机测试, 通过构建随机输入来扩展测试覆盖范围.  
如果给出的输入是随机的, 我们这么知道函数输出什么内容呢&#63; 这里有两种策略. 一种方式是额外写一个函数, 这个函数使用低效但是清晰的算法, 然后检查这两种实现的输出是否一致. 另外一种方式是构建符合某种模式的输入, 这样我们可以知道它们对应的输出是什么.  
下面的例子使用第二种方式, randomPalindrome 函数产生一系列的回文字符串, 这些输出在建构的时候就确定是回文字符串了.

```go

import "math/rand"
// randomPalindrome returns a palindrome whose length and contents
// are derived from the pseudo-random number generator rng.
func randomPalindrome(rng *rand.Rand) string {
    n := rng.Intn(25) // random length up to 24
    runes := make([]rune, n)
    for i := 0; i < (n+1)/2; i++ {
        r := rune(rng.Intn(0x1000)) // random rune up to '\u0999'
        runes[i] = r
        runes[n-1-i] = r
    }
    return string(runes)
}
func TestRandomPalindromes(t *testing.T) {
    // Initialize a pseudo-random number generator.
    seed := time.Now().UTC().UnixNano()
    t.Logf("Random seed: %d", seed)
    rng := rand.New(rand.NewSource(seed))
    for i := 0; i < 1000; i++ {
        p := randomPalindrome(rng)
        if !IsPalindrome(p) {
            t.Errorf("IsPalindrome(%q) = false", p)
        }
    }
}

```

由于随机测试的不确定性, 在遇到测试用例失败的情况下, 一定要记录足够的信息以便于重现这个问题. 在该例子中, 函数 IsPalindrome 的输入 p 告诉我们需要知道的所有信息, 但是对于那些拥有更复杂输入的函数来说, 记录伪随机数生成器的种子会比转储整个输入数据结构要简单的多. 有了随机数种子, 我们可以简单的修改测试代码来准确的重现错误.  
通过使用当前时间作为伪随机的种子源, 在测试的整个生命周期中, 每次运行的时候都会得到新的输入.

##### 11.2.2 测试命令

go test 工具对测试代码库包很有用, 但是也可以将它用于测试命令. 包名 main 一般会产生可执行文件, 但是也可以当做库来导入. 为 2.3.2 节的 echo 程序写一个测试. 把程序分成两个函数, echo 执行逻辑, 而 main 用来读取和解析命令行参数以及报告 echo 函数可能返回的错误.

```go

// Echo prints its command-line arguments.
package main
import (
"flag"
"fmt"
"io"
"os"
"strings"
)

var (
    n = flag.Bool("n", false, "omit trailing newline")
    s = flag.String("s", " ", "separator")
)
var out io.Writer = os.Stdout // modified during testing
func main() {
    flag.Parse()
    if err := echo(!*n, *s, flag.Args()); err != nil {
        fmt.Fprintf(os.Stderr, "echo: %v\n", err)
        os.Exit(1)
    }
}
func echo(newline bool, sep string, args []string) error {
    fmt.Fprint(out, strings.Join(args, sep))
    if newline {
        fmt.Fprintln(out)
    }
    return nil
}
```

在测试中, 我们会通过不同的参数和开关来调用 echo, 以检查它在每种测试用例下得到正确的输出, 所以我们还为 echo 函数添加了参数以避免依赖全局变量. 也就是说, 我们还引入了另外一个全局变量 out, 该变量是 io.Writer 类型, 所有的结果都将输出到这里. 通过将 echo 输出到这个变量而不是直接输出到 os.Stdout, 测试用例还可以用其他的替代 Writer 实现来记录写入的内容以便于后面检查. 下面是测试代码 (在文件 echo_test.go 中):

```go

package main
import (
"bytes"
"fmt"
"testing"
)
func TestEcho(t *testing.T) {
    var tests = []struct {
        newline bool
        sep     string
        args    []string
        want    string
    }{
        {true, "", []string{}, "\n"},
        {false, "", []string{}, ""},
        {true, "\t", []string{"one", "two", "three"}, "one\ttwo\tthree\n"},
        {true, ",", []string{"a", "b", "c"}, "a,b,c\n"},
        {false, ":", []string{"1", "2", "3"}, "1:2:3"},
    }
    for _, test := range tests {
        descr := fmt.Sprintf("echo(%v, %q, %q)",
            test.newline, test.sep, test.args)
        out = new(bytes.Buffer) // captured output
        if err := echo(test.newline, test.sep, test.args); err != nil {
            t.Errorf("%s failed: %v", descr, err)
            continue
        }
        got := out.(*bytes.Buffer).String()
        if got != test.want {
            t.Errorf("%s = %q, want %q", descr, got, test.want)
        }
    }
}
```

注意, 测试代码和产品代码在一个包里面. 尽管包的名称叫作 main, 并且里面定义了一个 main 函数, 但是在测试过程中, 这个包当作库来测试, 并且将函数 TestEcho 递送到测试驱动程序, 而 main 函数则被忽略了.  
通过表来组织测试用例, 我们可以很容易地添加测试用例. 下面添加一行到测试用例表中, 来看看测试失败地时候发生了什么.

```go
{true, ",", []string{"a", "b", "c"}, "a b c\n"}, // NOTE: wrong expectation!
```

运行 go test 输出:

```go
 go test gopl.io/ch11/echo
--- FAIL: TestEcho (0.00s)
echo_test.go:31: echo(true, ",", ["a" "b" "c"]) = "a,b,c", want "a b c\n"
FAIL
FAIL gopl.io/ch11/echo 0.006s
```
