---
title: "C Function Tip."
categories:
  - C 
tags:
  - tips
---

List of useful function in C Language.

```C
#include <stdio.h>

//assumes little endian
void printBits(size_t const size, void const * const ptr) {
    unsigned char *b = (unsigned char *) ptr;
    unsigned char byte;
    int i, j;

    for (i = size - 1; i >= 0; i--) {
        for (j = 7; j >= 0; j--) {
            byte = (b[i] >> j) & 1;
            printf("%u", byte);
        }
    }
    puts("");
}

int main() {
    int n, k, data, i, remain_bits;

    printf("Nhap n: ");
    scanf("%d", &n);
    printf("Nhap k (k <= %d): ", n);
    scanf("%d: ", &k);
    printf("n: %d, k: %d\n", n, k);

    /*
     k = 1 -> data = 1
     k = 2 -> data = 11
     k = 3 -> data = 111
     */
    i = 0;
    data = 0;
    while (i < k) {
        data = data | (1 << i);
        i++;
    }
    //printf("data: ");
    //printBits(sizeof(data), &data);

    /*
     n = 4
     k = 2
     => remain_bits = 4 - 2 = 2 (+ 1)
     */
    remain_bits = n - k + 1;
    for (i = 0; i < remain_bits; i++) {
        int result = (data << i) | 0x0;
        printBits(sizeof(result), &result);
    }

    return 0;
}
```
