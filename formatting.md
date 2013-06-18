# 文章を80桁で折り返し

与えられた文章をスペースで区切って、80桁に収まるように改行して出力してくださ
い。

# Cによる回答

```C
#include <stdio.h>

void format(const char *paragraph, int columns) {
  const char *start = paragraph;
  int curr_col = 0, i;
  while (*start != '\0') {
    i = 0;
    while (start[i] != '\0' && start[i] != ' ') {
      i++;
    }
    if (curr_col != 0 && curr_col + i > columns) {
      putchar('\n');
      curr_col = i + 1;
    } else {
      if (curr_col != 0) {
        putchar(' ');
      }
      curr_col += i + 1;
    }

    while (*start != '\0' && *start != ' ') {
      putchar(*start);
      start++;
    }

    while (*start != '\0' && *start == ' ') {
      start++;
    }
  }
}
```

# JavaScriptによる回答

```JavaScript
var format = function (str, column) {
  var strs = str.split(/( )/);
  var output = "";
  var line = strs.length > 0 ? strs.shift() : "";
  while (strs.length > 0) {
    if (line.length + strs[0].length > column) {
      output += line.replace(/ *$/, "") + "\n";
      line = "";
    }
    line += strs.shift();
    if (line === " ") {
      line = "";
    }
  }
  output += line;
  return output;
};

format("hoge fuga  hogehoge   fugafuga", 10);//"hoge fuga\nhogehoge\nfugafuga"
```
