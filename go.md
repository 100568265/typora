# const和iota

iota只能配合const括号一起使用

```go
const(
	BEIJING = iota		//iota=0
    SHANGHAI			//iota=1
    SHENZHEN			//iota=2
)

const(
	a,b = iota+1,iota+2
    c,d
    e,f
)
```





# 多返回值

```go
//	函数名		形参			返回值类型
func foo2(a string, b int) (int, int) {
	fmt.Println("a = ", a)
	fmt.Println("b = ", b)
	return 666,777
}
```

```go
func foo2(a string, b int) (int, int) {
	fmt.Println("a = ", a)
	fmt.Println("b = ", b)
	return 666, 777
}

func main() {
	ret1, ret2 := foo2("aaa", 555)
	fmt.Println("ret1 = ", ret1, "ret2 = ", ret2)
}
```



返回多个返回值，有形参名称的：

```go
func foo3(a string, b int) (r1 int, r2 int) {
	fmt.Println("a = ", a)
	fmt.Println("b = ", b)

	//给有名称的返回值变量赋值
	r1 = 1000
	r2 = 2000
	return
}
```





# 包

```go
package main

import(
	"GolangStudy/5-init/lib1"
    "GolangStudy/5-init/lib1"
)


func main(){
    lib1.Lib1Test()
    lib2.Lib2Test()
}
```



```go
import . "fmt"
//通过点，将当前fmt包中的全部方法，导入到当前本包的作用域中，fmt中所有方法可以不用fmt.API来调用
```







# defer语句

在函数开始后，结束前执行的操作

```go
func retrurnAndDefer int{
    defer deferFunc()
    return returnFunc()
}
```

**总结：**return之后的语句先执行，defer后的语句后执行









# **静态数组array**

```go
//固定长度的数组
var myArray1 [10]int

myArray2 := [10]int{1,2,3,4}
```



数组作为参数传入，类型必须匹配。

例如：不能把一个长度为10的数组传入下面的形参中

```go
//数组作为参数
func printArray(myArray [4]int){
    for index,value := range myArray{
        fmt.Println("index = ", index, "value = ", value)
    }
}
```

总结：这种静态数组的传入参数是**值传递**







# **动态数组slice**

声明slice的四种方式：

```go
//1.声明slice1是一个切片，并初始化，默认值是1,2,3，长度len是3
slice1 := []int{1,2,3}

//2.声明slice1是一个切片，但是并没有给slice分配空间
var slice1 []int

//3.给slice开辟三个容量
var slice1[] int = make([]int,3)//开辟三个空间，默认值0

//4.声明slice是一个切片
slice1 := make([]int,3)
```



**判断一个slice是否为0**

```go
if slice1 == nil{
    fmt.Println("slice是一个空切片")
}else{
    fmt.Println("slice1是有空间的")
}
```





**切片容量的追加**