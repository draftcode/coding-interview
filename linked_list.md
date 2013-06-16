# 単方向連結リストの実装

単方向の連結リストを実装してください。実装するのは、リストに値を追加する関数の
みでよいです。追加する値はどこに追加されても構いません。

# Cによる回答

```C
#include <stdlib.h>
#include <stdio.h>

typedef struct _node {
  struct _node* next;
  int value;
} NODE;

NODE *prepend(NODE* node, int value) {
  NODE *ret = malloc(sizeof(NODE));
  ret->next = node;
  ret->value = value;
  return ret;
}

int main(void) {
  NODE *node = NULL, *p;
  node = prepend(node, 1);
  node = prepend(node, 2);
  node = prepend(node, 3);

  p = node;
  while (p) {
    printf("%d\n", p->value);
    p = p->next;
  }
}
```
