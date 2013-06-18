# データの集計

単純なスペース区切りで"文字列 数値"というデータが与えられるので、文字列ごとに
数値の合計を出し、数値が大きい順に出力してください。

注) 文字列の大きさは事前に定められた大きさに収まると考えて良いです。

入力例:
```
Apple 3
Banana 5
Melon 2
Apple 8
Melon 4
```

出力例:
```
Apple 11
Melon 6
Banana 5
```

# Cによる回答

```C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct _node {
  struct _node *next;
  char *tag;
  int value;
  unsigned char output;
} NODE;

NODE *find(NODE *node, const char *tag) {
  if (!node) return NULL;
  if (strcmp(tag, node->tag) == 0) {
    return node;
  } else {
    return find(node->next, tag);
  }
}

char *strdup(const char *str) {
  char *ret = malloc(strlen(str)+1);
  strcpy(ret, str);
  return ret;
}

NODE *prepend(NODE *node, char *tag, int value) {
  NODE *ret = malloc(sizeof(NODE));
  ret->next = node;
  ret->tag = tag;
  ret->value = value;
  ret->output = 0;
  return ret;
}

void aggregate(void) {
  char str[16];
  int num, node_count = 0;
  NODE *node = NULL, *p, *q;

  while (scanf("%s %d", str, &num) == 2) {
    p = find(node, str);
    if (p) {
      p->value += num;
    } else {
      node = prepend(node, strdup(str), num);
      node_count++;
    }
  }

  for (;;) {
    p = node, q = NULL;
    while (p) {
      if (!p->output) {
        if (!q || q->value < p->value) {
          q = p;
        }
      }
      p = p->next;
    }

    if (q) {
      printf("%s %d\n", q->tag, q->value);
      q->output = 1;
    } else {
      break;
    }
  }
}

int main(void) {
  aggregate();
  return 0;
}
```

# Pythonによる回答

```Python
def aggregate(f):
    d = dict()
    for line in f:
        tag, value_s = line.strip().split(' ')
        value = int(value_s)
        if tag in d:
            d[tag] += value
        else:
            d[tag] = value
    for tag, value in reversed(sorted(d.items(), key=lambda x: x[1])):
        print("{} {}".format(tag, value))
```
