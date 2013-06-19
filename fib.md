# フィボナッチ数の計算

N番目のフィボナッチ数を計算する関数を、再帰とループ、両方で実装してください。
また、それを最適化してください。

例:
```
fib(0) # => 0
fib(1) # => 1
fib(2) # => 1
fib(3) # => 2
```

# Cによる回答

```C
#include <stdio.h>

int fib_rec(int n) {
  if (n == 0 || n == 1) {
    return n;
  } else {
    return fib_rec(n-1) + fib_rec(n-2);
  }
}

int fib_loop(int n) {
  int prev = 0, curr = 1, next;
  if (n == 0) return 0;
  while (n != 1) {
    next = curr + prev;
    prev = curr;
    curr = next;
    n--;
  }
  return curr;
}

int fib_rec_opt1(int n) {
  static char has_fib_cache[16];
  static int fib_cache[16];

  if (!has_fib_cache[n]) {
    has_fib_cache[n] = 1;
    if (n == 0 || n == 1) {
      fib_cache[n] = n;
    } else {
      fib_cache[n] = fib_rec_opt1(n-1) + fib_rec_opt1(n-2);
    }
  }
  return fib_cache[n];
}

int fib_rec_opt2_sub(int n, int k, int fib_k, int fib_k_1) {
  if (k == n) {
    return fib_k;
  } else {
    return fib_rec_opt2_sub(n, k+1, fib_k + fib_k_1, fib_k);
  }
}

int fib_rec_opt2(int n) {
  if (n == 0 || n == 1) return n;
  return fib_rec_opt2_sub(n, 1, 1, 0);
}

int main(void) {
  for (int i = 0; i < 10; i++) {
    printf("fib(%d): %2d %2d %2d %2d\n",
        i, fib_rec(i), fib_loop(i), fib_rec_opt1(i), fib_rec_opt2(i));
  }
  return 0;
}
```
