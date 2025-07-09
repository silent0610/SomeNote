---
Type: 
aliases:
  - C++11内存对齐
tags: 
modifiedDate: 2025/06/17, 21:23:02
---

# C++11内存对齐

- [1.概念](https://zhuanlan.zhihu.com/p/622252108/edit#1%E6%A6%82%E5%BF%B5)
- [2.默认内存对齐](https://zhuanlan.zhihu.com/p/622252108/edit#2%E9%BB%98%E8%AE%A4%E5%86%85%E5%AD%98%E5%AF%B9%E9%BD%90)
- [2.1.基本类型](https://zhuanlan.zhihu.com/p/622252108/edit#21%E5%9F%BA%E6%9C%AC%E7%B1%BB%E5%9E%8B)
	- [2.2.结构体类型](https://zhuanlan.zhihu.com/p/622252108/edit#22%E7%BB%93%E6%9E%84%E4%BD%93%E7%B1%BB%E5%9E%8B)
- [3.显式内存对齐](https://zhuanlan.zhihu.com/p/622252108/edit#3%E6%98%BE%E5%BC%8F%E5%86%85%E5%AD%98%E5%AF%B9%E9%BD%90)
- [3.1.pragma pack](https://zhuanlan.zhihu.com/p/622252108/edit#31pragma-pack)
	- [3.2.alignas](https://zhuanlan.zhihu.com/p/622252108/edit#32alignas)
	- [3.3.alignof](https://zhuanlan.zhihu.com/p/622252108/edit#33alignof)
	- [3.4.std::aligned\_storage](https://zhuanlan.zhihu.com/p/622252108/edit#34stdaligned_storage)
	- [3.5.std::aligned\_union](https://zhuanlan.zhihu.com/p/622252108/edit#35stdaligned_union)
- [4.内存对齐的应用](https://zhuanlan.zhihu.com/p/622252108/edit#4%E5%86%85%E5%AD%98%E5%AF%B9%E9%BD%90%E7%9A%84%E5%BA%94%E7%94%A8)
- [4.1.提高程序性能](https://zhuanlan.zhihu.com/p/622252108/edit#41%E6%8F%90%E9%AB%98%E7%A8%8B%E5%BA%8F%E6%80%A7%E8%83%BD)
	- [4.2.平台移植](https://zhuanlan.zhihu.com/p/622252108/edit#42%E5%B9%B3%E5%8F%B0%E7%A7%BB%E6%A4%8D)
	- [4.3.SIMD 与内存对齐](https://zhuanlan.zhihu.com/p/622252108/edit#43simd-%E4%B8%8E%E5%86%85%E5%AD%98%E5%AF%B9%E9%BD%90)
- [5.总结](https://zhuanlan.zhihu.com/p/622252108/edit#5%E6%80%BB%E7%BB%93)

## 1. 概念

C++ 中的内存对齐是指，编译器在将变量分配到内存中时按照一定规则进行调整，使得变量在内存中的地址满足一定的要求。具体来说，内存对齐是将变量的起始地址调整为其自身大小或者某个值的倍数，这个值称为“ [对齐系数](https://zhida.zhihu.com/search?content_id=226409532&content_type=Article&match_order=1&q=%E5%AF%B9%E9%BD%90%E7%B3%BB%E6%95%B0&zhida_source=entity) ”。在介绍内存对齐前补充几个概念——对齐系数，为便于理解，个人把它分为 2 类：

**[绝对对齐系数](https://zhida.zhihu.com/search?content_id=226409532&content_type=Article&match_order=1&q=%E7%BB%9D%E5%AF%B9%E5%AF%B9%E9%BD%90%E7%B3%BB%E6%95%B0&zhida_source=entity)** ：每种类型数据**在内存中的首地址必须是绝对对齐系数内存系数的整数倍**。在默认情况下，*绝对对齐系数等于该类型数据的默认对齐系数*。

**[相对对齐系数](https://zhida.zhihu.com/search?content_id=226409532&content_type=Article&match_order=1&q=%E7%9B%B8%E5%AF%B9%E5%AF%B9%E9%BD%90%E7%B3%BB%E6%95%B0&zhida_source=entity)** ：结构体内的数据相对于结构体起始地址的偏移量，单位为字节，结构体成员**在结构体中的偏移量必须是该成员相对对齐系数的整数倍**。在默认情况下，*相对对齐系数等于该类型数据的默认对齐系数*。

## 2. 默认内存对齐

默认情况下，绝对对齐系数和相对对齐系数等于默认对齐系数。 
- **默认对齐系数** ：是数据的固有属性。
- 基本数据类型: 该数据类型占用字节大小；
- 结构体类型: 结构体成员中**占用空间最大的基本数据类型占用字节大小**。

### 2.1. 基本类型

对于基本数据类型，一个变量在内存中的地址必须是其自身大小（以字节为单位）的整数倍。比如 `int` 类型的变量大小为 4，也就是说它的绝对对齐系数为 4，因此它的地址必须是 4 的倍数。

### 2.2. 结构体类型

对于结构体类型，
- 第一，结构体的起始地址必须是结构体成员中占用空间最大的基本数据类型的整数倍，且结构体大小也必须是其整数倍；
- 第二，结构体内的成员，在结构体中的偏移量必须是该成员相对对齐系数的整数倍。举个例子：

```c
struct MyStruct {
    char c;
    int i;
    short s;
};
```

对于该结构体， `char` 类型占 1 个字节， `int` 类型占 4 个字节， `short` 类型占 2 个字节，由于 `int` 类型的大小最大，因此该结构体的默认对齐系数是 4，推导出绝对对齐系数为 4，该结构体起始地址必须为 4 的倍数； `c` 在相对对齐系数为 1，因此偏移为 0， `i` 的相对对齐系数为 4，因此偏移为 4， `s` 相对对齐系数为 2，因此偏移为 8；该结构体的总大小为绝对对齐系数 4 的整数倍，因此 12 个字节。

## 3. 显式内存对齐

在 C++ 中，也可以使用关键字来显式地指定对齐系数。 [C++11](https://zhida.zhihu.com/search?content_id=226409532&content_type=Article&match_order=1&q=C%2B%2B11&zhida_source=entity) 以前关于显式指定内存对齐没有标准规定，为了满足对”显式指定内存对齐“的需求，编译器厂家引入了各自的关键字，包括 `__attribute__((aligned))` 、 `#pragma pack` 等；C++11 以后，显式指定内存对齐的语法被纳入标准，新增了 `alignas` 、 `alignof` 等关键字用于内存对齐。接下来对这些关键字进行介绍。

### 3.1. Pragma Pack

`#pragma pack` 虽然不是官方标准，但现代编译器基本都对该关键字予以了支持。 `#pragma pack(N)` ， `N` 必须为 2 的正数幂，有两个作用：

- 当 `N` 大于该结构体的默认对齐系数时候，结构体绝对对齐系数被设置为 `N` ，当 `N` 小于该结构体的默认对齐系数时候，结构体绝对对齐系数维持为默认对齐系数。
- 当 `N` 小于该结构体的默认对齐系数时候，结构体内所有成员的相对对齐系数被设置为 `N` ，当 `N` 大于该结构体的默认对齐系数时候，结构体内成员的相对对齐系数维持为默认对齐系数。

例如当 `N` 小于结构体默认对齐系数的时候：

```cpp
#pragma pack(1)
//首地址为4的倍数
struct MyStruct {
    char c;//相对偏移：0
    int i;//相对偏移：1
    short s;//相对偏移：5
};
#pragma pack()
```

再例如，当 `N` 小于结构体默认对齐系数的时候：

```cpp
#pragma pack(8)
//首地址为8的倍数
struct MyStruct {
    char c;//相对偏移：0
    int i;//相对偏移：4
    short s;//相对偏移：8
};
#pragma pack()
```

### 3.2. Alignas

用于设置指定对齐系数。 `alignas(N)` 表示把绝对对齐系数设置为 `N` 字节， `N` 必须是 2 的正数幂。注意和 `#pragma pack` 不同的是，该关键字不能指定结构体的相对对齐系数。

- 当 `N` 大于该结构体的默认对齐系数时候，结构体绝对对齐系数被设置为 `N` ，当 `N` 小于该结构体的默认对齐系数时候，结构体绝对对齐系数维持为默认对齐系数。

例如当 `N` 小于结构体默认对齐系数的时候：

```cpp
//首地址为4的倍数
struct alignas(1) MyStruct {
    char c;//相对偏移：0
    int i;//相对偏移：4
    short s;//相对偏移：8
};
```

再例如，当 `N` 小于结构体默认对齐系数的时候：

```cpp
//首地址为8的倍数
struct alignas(8) MyStruct {
    char c;//相对偏移：0
    int i;//相对偏移：4
    short s;//相对偏移：8
};
```

### 3.3. Alignof

`alignof` 操作符：用于获取指定类型的对齐系数，返回一个 `size_t` 类型的值。它的使用方式如下：

```cpp
#include <iostream>
#include <type_traits>

int main() {
  std::cout << alignof(int) << '\n';     //输出：4
  std::cout << alignof(double) << '\n';  //输出：8
  std::cout << alignof(char[7]) << '\n'; //输出：1
  return 0;
}
```

### 3.4. Std:: aligned\_storage

`std::aligned_storage` 是 C++11 标准库中的一个模板类，用于创建固定大小和对齐要求的未初始化内存，缓冲区适合存储任何类型。它有两个参数：

```cpp
template <std::size_t Len, std::size_t Align = alignof(std::max_align_t)>
struct aligned_storage {/**/};
```

第一个参数表示内存块的大小，以字节为单位；第二个参数表示对齐系数，大小要求必须是 2 的正数幂。例如，以下代码创建了一块大小为 16 字节、对齐系数为 8 字节的内存：

```cpp
#include <type_traits>
// 创建一个指定大小和对齐要求的 std::aligned_storage 类型
using my_storage_type = std::aligned_storage<16, 8>::type;
int main() 
{
    // 使用 my_storage_type 类型创建一个未初始化的原始内存缓冲区
    my_storage_type buffer;
    // 可以使用placement new将任何类型放入这个缓冲区，需要满足对齐要求
    int* p_int = new (&buffer) int(42);
    // 记得在程序结束前手动释放资源
    p_int->~int();
    return 0;
}
```

### 3.5. Std:: aligned\_union

`std::aligned_union` 用于创建一个可以存储给定类型中最大的那个类型对象的内存，并且满足对齐要求。它有两个参数：

```cpp
template <std::size_t Len, class... Types>
struct aligned_union {/**/};
```

其中，第一个参数 `Len` 表示要创建的内存块的大小（以字节为单位），第二个参数 `Types` 是一系列可以被存储在这块内存块的类型，当创建内存块时， `std::aligned_union` 会找到这些类型中最大的那个类型，并把该类型的默认对齐系数设置为这块内存的绝对对齐系数。例如：

```cpp
std::aligned_union<sizeof(double), int, double>::type data;
```

在这个例子中，我们指定了内存块的大小为 `sizeof(double)` 字节，可存储的类型为 `int` 和 `double` ，对齐系数为 8（即 `double` 的大小）。需要注意的是，如果指定的 `Len` 小于所选的类型中最大的那个类型的大小，则内存块大小为最大的那个类型的大小。例如，在以下代码中：

```cpp
std::aligned_union<4, int, double>::type data;
```

由于 4 字节无法容纳 `double` 类型，所以 `std::aligned_union` 会将内存块的大小调整为 8（即 `double` 的大小），并确保对齐要求为 8。

## 4. 内存对齐的应用

内存对齐在计算机领域的应用非常广泛。以下是一些常见的应用场景：

### 4.1. 提高程序性能

内存对齐的主要目的是优化 CPU 对内存的读写操作。现代计算机通常使用“字（word）”作为内部数据交换的基本单位，字是 CPU 在一次内存读写操作中能处理的最大数据块。变量占据内存大小等于字长，如果其首地址在字长的整数倍处，CPU 只需要读内存一次；如果其首地址不在字长整数倍处，CPU 则需要读地址两次，影响程序性能。

![](https://pic3.zhimg.com/v2-e8091f4a803e5575cc25b7e99a6b4aa6_1440w.png)

取址次数

### 4.2. 平台移植

不同平台对内存对齐的要求也有所不同，比如说：在 C 语言标准中，并没有明确规定 `short` 数据类型的长度应该是多少。通常情况下， `short` 被定义为一个整数类型，其长度通常为 2 个字节（16 位），用于表示相对较小的整数值。然而，在某些平台上， `short` 的长度可能会不同。例如，在一些嵌入式系统中，由于存储空间有限，可能会将 `short` 定义为 1 个字节或者其他长度，以节省空间和提高性能。

如果没有考虑到这一点，在移植过程中可能会出现问题，例如结构体大小不一致等。使用内存对齐可以保证数据在内存中按照一定的规则存储，提高代码的可移植性。

### 4.3. SIMD 与内存对齐

在 SIMD 中，内存对齐需要被特别注意和优化，因为 SIMD 操作通常会同时处理多个数据元素。如果数据在内存中没有按照合适的方式进行对齐，那么这些数据很可能无法被同时加载到 SIMD 寄存器中，从而导致额外的内存访问、数据拷贝或者其他操作，进而降低程序的性能。

下面以 x86 平台下的 [SSE](https://zhida.zhihu.com/search?content_id=226409532&content_type=Article&match_order=1&q=SSE&zhida_source=entity) （Streaming SIMD Extensions）指令为例来说明内存对齐在 SIMD 中的应用。该架构使用 128 位的向量寄存器，数据总线长度位 128 位，每次能读取 16 字节。假设有以下的代码：

```cpp
alignas(16) float A[4];
alignas(16) float B[4];
alignas(16) float C[4];

for (int i = 0; i < 4; i++) {
    C[i] = A[i] + B[i];
}
```

在这段代码中，由于浮点数默认是按照 4 字节对齐的，数组的首地址是 4 的倍数，因此如果数组没有进行显式对齐，那么每次计算 4 个浮点数可能就需要进行两次内存读写。然而，由于数组已经按照 16 字节对齐，每次计算 4 个浮点数就只需要进行一次内存读写。

## 问题

1. 内存浪费：填充字节没有任何意义，但是占据了内存空间
2. 跨平台兼容性：不同硬件平台和编译器，可能会存在不同的内存对齐规则，如果按照指针+偏移量的模式来访问内存，可能会出现一定的问题；某些平台可能无法访问未对齐的数据，而另一些平台则允许访问未对齐的数据。
