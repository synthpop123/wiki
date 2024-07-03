# C/C++ | Q & A

Q：这段代码有什么问题？

```c++
void func(void **p) {
    p = (void **)malloc(512);
}

int main()
{
    void **p;
    func(p);
    strcpy((char *)p, "Hello");
    printf("%s", p);
    free((char*) p);
    return(0);
}
```

A：在这段代码中，存在几个问题：

1. 在 `func` 函数中，虽然函数参数 `p` 是一个指向指针的指针，但是在函数内部，你将传入的指针 `p` 直接赋值为一个新分配的内存块的地址，而没有修改原始指针指向的位置。这意味着在函数调用结束后，原始指针 `p` 并没有指向新分配的内存块，导致内存泄漏。

2. 在 `main` 函数中，没有初始化指针 `p`，这意味着 `p` 包含一个随机的内存地址，这会导致未定义的行为。

3. 在 `main` 函数中，你尝试对未初始化的指针 `p` 进行 `strcpy` 操作，这会导致未定义的行为，因为你没有为 `p` 分配内存，所以无法将字符串 "Hello" 复制到该地址。

4. 在 `main` 函数中，你在使用 `free` 函数释放内存时，应该使用 `free(p)` 而不是 `free((char*) p)`，因为 `p` 是 `void **` 类型的指针，不需要强制转换为 `char *` 类型。

下面是修正后的代码示例：

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

void func(void **p) {
    *p = malloc(512);
}

int main() {
    void *p;
    func(&p);
    strcpy((char *)p, "Hello");
    printf("%s", (char *)p);
    free(p);
    return 0;
}
```

在修正后的代码中，通过传递指向指针的指针，我们可以在 `func` 函数中分配内存并将新的内存块的地址存储在原始指针 `p` 所指向的位置。然后在 `main` 函数中，我们可以正确地使用 `p` 指向的内存块。

---
