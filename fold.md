# 数値列の畳み込み

数値の列と初期値が与えられるので、関数をとってそれを用いて畳み込みを行うような
関数を実装してください。

例:
```Scheme
(foldl (lambda (r x) (+ r x)) 0 '(1 2 3 4)) # => 10
```

# Cによる回答

```C
#include <stdio.h>

int foldl(const int *array, size_t sz, int init, int (*f)(int, int)) {
  size_t i;
  int r = init;
  for (i = 0; i < sz; i++) {
    r = f(r, array[i]);
  }
  return r;
}

int plus(int a, int b) {
  return a + b;
}

int main(void) {
  int array[] = {1, 2, 3, 4};
  printf("%d\n", foldl(array, sizeof(array)/sizeof(int), 0, plus));
  return 0;
}
```
