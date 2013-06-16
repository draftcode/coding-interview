# 単方向連結リストの`delete_if`

[単方向連結リスト](linked_list.md)に指定の条件に一致する要素を取り除く
`delete_if`の実装を追加してください。

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

NODE *delete_if(NODE* node, int value) {
  if (!node) return NULL;
  if (node->value == value) {
    NODE* next = node->next;
    free(node);
    return delete_if(next, value);
  } else {
    node->next = delete_if(node->next, value);
    return node;
  }
}

int main(void) {
  NODE *node = NULL, *p;
  node = prepend(node, 1);
  node = prepend(node, 2);
  node = prepend(node, 1);

  node = delete_if(node, 1);
  p = node;
  while (p) {
    printf("%d\n", p->value);
    p = p->next;
  }
}
```
