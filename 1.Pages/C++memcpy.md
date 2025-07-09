---
Type:
aliases: 
tags: 
modifiedDate: 2025/06/17, 22:30:28
---

# C++memcpy

```cpp
void *memcpy(void *dest, const void *src, size_t n);
```

## 功能

从src的开始位置拷贝n个字节的数据到dest。如果dest存在数据，将会被覆盖。memcpy函数的返回值是dest的指针。

## 实现

按照CPU 字大小(64 位 CPU通常为 8B)进行拷贝（这个版本没有考虑内存重叠的情况）

- 即先按 8B 拷贝, 可以使用 `int64_t` 或者 ` long long ``
- 剩下的按 1B 拷贝, `char`

```cpp
void * Memcpy1(void *dst, const void *src, size_t num)
{
 int nchunks = num/sizeof(dst);   /*按CPU位宽拷贝*/

 cout<<"sizeof(dst)是："<<sizeof(dst)<<endl;

 int slice =   num%sizeof(dst);   /*剩余的按字节拷贝*/
 
 int64_t * s = (int64_t *)src; //8B
 int64_t * d = (int64_t*)dst;
 
 while(nchunks--)
     *d++ = *s++;
     
 while (slice--)
     *((char *)d++) =*((char *)s++); // 1B
     
 return dst;
}
```
