# 文字列のSqueeze

文字列中の中で指定された文字が連続している部分を一つの文字に置き換えるsqueeze
を実装してください。

例: `'aaabbbaaa'.squeeze('a') # => 'abbba'`

# Cによる回答

```C
#include <stdio.h>

void str_squeeze(char *target, char c) {
  int i = 0, j = 0;

  while (target[j] != '\0') {
    target[i] = target[j];
    j++;
    if (target[i] == c) {
      while (target[j] != '\0' && target[j] == target[i]) {
        j++;
      }
    }
    i++;
  }
  target[i] = '\0';
}

int main(void) {
  char target[] = "aaabbbaaa";
  str_squeeze(target, 'a');
  printf("%s\n", target);
}
```

# JavaScriptによる回答

```JavaScript
var squeeze = function(str,ch) {
  var r = new RegExp(ch+"+","g");
  return str.replace(r,ch);
}
```

# sedによる回答

```Shell
echo $1 | sed -e "s/$2\+/$2/g"
```

# Rubyによる回答

```Ruby
class String
  def squeeze(c)
    s = ""
    now = false
    0.upto(self.size-1) do |i|
      if self[i] == c
        if now
          next
        else
          s << self[i]
          now = true
        end
      else
        s << self[i]
        now = false
      end
    end
    return s
  end

  def squeeze2(c)
    self.gsub(/#{c}+/, c)
  end
end
```
