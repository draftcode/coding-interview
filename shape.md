# 図形操作のクラスを作る

次の2つの操作が行える三角形、四角形、円のクラスを作ってください。

* `move(dx, dy)`
* `area()`

# Pythonによる回答

```python
import math

class Shape(object):
    def __init__(self, x, y):
        self._x = x
        self._y = y

    def move(dx, dy):
        self._x += dx
        self._y += dy

    def area(self):
        raise NotImplementedError

class Triangle(Shape):
    """A triangle which is made by the points (x, y), (x + w, y) (x, y + h)"""

    def __init__(self, x, y, w, h):
        super(Triangle, self).__init__(x, y)
        self._w = w
        self._h = h

    def area(self):
        return (self._w * self._h) / 2

class Rectangle(Shape):
    def __init__(self, x, y, w, h):
        super(Rectangle, self).__init__(x, y)
        self._w = w
        self._h = h

    def area(self):
        return self._w * self._h

class Circle(Shape):
    def __init__(self, x, y, r):
        super(Circle, self).__init__(x, y)
        self._r = r

    def area(self):
        return self._r * self._r * math.pi
```

# Cによる回答

```c
#include <stdio.h>
#include <stdlib.h>

#pragma pack(8)

struct shape_vtbl;
typedef struct {
  double x, y;
  struct shape_vtbl *shape_vtbl;
} SHAPE;

struct shape_vtbl {
  void (*move)(void *self, double dx, double dy);
  double (*area)(void *self);
};

void SHAPE_move(void *_self, double dx, double dy) {
  SHAPE *self = _self;
  self->x += dx;
  self->x += dy;
}

void *init_SHAPE(void *_self, double x, double y) {
  SHAPE *self = _self;
  if (!self) return self;
  self->shape_vtbl = malloc(sizeof(struct shape_vtbl));
  if (!self->shape_vtbl) return NULL;

  self->x = x;
  self->y = y;
  self->shape_vtbl->move = SHAPE_move;
  self->shape_vtbl->area = NULL;

  return self;
}

typedef struct {
  SHAPE parent;
  double h, w;
} TRIANGLE;

double TRIANGLE_area(void *_self) {
  TRIANGLE *self = _self;
  return self->h * self->w / 2;
}

void *init_TRIANGLE(void *_self, double x, double y, double h, double w) {
  TRIANGLE *self = (TRIANGLE *)init_SHAPE(_self, x, y);
  if (!self) return self;

  self->h = h;
  self->w = w;
  self->parent.shape_vtbl->area = TRIANGLE_area;

  return self;
}

typedef struct {
  SHAPE parent;
  double h, w;
} RECTANGLE;

double RECTANGLE_area(void *_self) {
  RECTANGLE *self = _self;
  return self->h * self->w;
}

void *init_RECTANGLE(void *_self, double x, double y, double h, double w) {
  RECTANGLE *self = init_SHAPE(_self, x, y);
  if (!self) return self;

  self->h = h;
  self->w = w;
  self->parent.shape_vtbl->area = RECTANGLE_area;

  return self;
}

typedef struct {
  SHAPE parent;
  double r;
} CIRCLE;

double CIRCLE_area(void *_self) {
  CIRCLE *self = _self;
  return self->r * self->r * 3.14;
}

void *init_CIRCLE(void *_self, double x, double y, double r) {
  CIRCLE *self = init_SHAPE(_self, x, y);
  if (!self) return self;

  self->r = r;
  self->parent.shape_vtbl->area = CIRCLE_area;

  return self;
}

int main(void) {
  SHAPE *triangle = malloc(sizeof(TRIANGLE));
  if (!init_TRIANGLE(triangle, 0.0, 0.0, 1.0, 1.0)) abort();

  SHAPE *rect = malloc(sizeof(RECTANGLE));
  if (!init_RECTANGLE(rect, 0.0, 0.0, 1.0, 1.0)) abort();

  SHAPE *circle = malloc(sizeof(CIRCLE));
  if (!init_CIRCLE(circle, 0.0, 0.0, 1.0)) abort();

  printf("TRIANGLE.size() = %f\n", triangle->shape_vtbl->area(rect));
  printf("RECT.size() = %f\n", rect->shape_vtbl->area(rect));
  printf("CIRCLE.size() = %f\n", circle->shape_vtbl->area(rect));
}
```
