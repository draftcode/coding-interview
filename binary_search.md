# 二分探索

ソートされた数列と数が与えられるので、数列の中に数が入っているかどうかを判定す
る関数を書いてください。また、計算量を見積もってください。

# Pythonによる回答

```python
def has(sorted_list, number):
    # O(n*log(n)) setの構築に時間がかかる。
    return number in set(sorted_list)

def has(sorted_list, number):
    # O(log(n))
    l = 0
    r = len(sorted_list)
    while l < r:
        mid = int((l + r) / 2)
        if sorted_list[mid] == number:
            return True
        elif sorted_list[mid] < number:
            l = mid + 1
        else:
            r = mid
    return False
```

# Cによる回答

```c
#include <stdio.h>

char binary_search(const int *sorted_array, size_t array_size, int number) {
  int r = 0, l = array_size;
  while (r < l) {
    int mid = (r+l)/2;
    if (sorted_array[mid] == number) {
      return 1;
    } else if (sorted_array[mid] < number) {
      r = mid + 1;
    } else {
      l = mid;
    }
  }
  return 0;
}

int main(void) {
  int sorted_array[] = { 1, 3, 5, 7, 9 };
  for (int i = 0; i < 11; i++) {
    printf("%d => %s\n", i, (binary_search(sorted_array, 5, i) ? "TRUE" : "FALSE"));
  }
}
```
