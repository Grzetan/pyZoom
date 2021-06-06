# Introduction

pyZoom is an extension to pygame that allows you to zoom into drawn figures without quality loss.
It can be very useful for example when you want to play around with fractals or allow player of your 2D game zoom into the screen

# Installation

### Windows
```
pip install pyZoom 
```

### Linux
```
pip3 install pyZoom
```

# Requirements

- pygame

# How it works

PyZoom sees canvas not as pixel grid but as coordinates system.

When using pyZoom you can declare key point's of figures by using floats.

That means you can for example declare the center of a circle at (x=20.34, y=189.798).

It also allows you to draw very small objects like a line with starting point (0,0) and ending point (0.2, 0.2).
Of course, you will not see it when you are zoomed out all the way but when you start to zoom in this tiny line will slowly get bigger.

# Usage
First we need to import our dependencies.

```python
import pygame
from pyZoom import PyZoom
```

In this tutorial I will be using object-oriented programming (OOP).

The first step to using pyZoom is declaring an instance of PyZoom object.
It should be done in the constructor.
Make sure to pass width and height to pyZoom constructor.

```python
class Window:
    def __init__(self):
        pygame.init()
        self.WIDTH, self.HEIGHT = 1000, 800
        self.WIN = pygame.display.set_mode((self.WIDTH, self.HEIGHT))
        self.CLOCK = pygame.time.Clock()
        self.FPS = 30
        self.run = True
        self.pyzoom = PyZoom(self.WIDTH, self.HEIGHT)
        self.pyzoom.set_background((255, 0, 0))
        self.loop()
```

You can also set background color. It's black by default.

Then we write basic loop method

```python
def loop(self):
    while self.run:
        self.refresh_window()
        self.events()
        self.CLOCK.tick(self.FPS)
    pygame.quit()
    sys.exit()
```

'events' method to close our window:

```python
def events(self):
    for e in pygame.event.get():
        if e.type == pygame.QUIT:
            self.run = False
```

And last but not least, refresh_window method:

```python
def refresh_window(self):
    self.WIN.fill(0)
    #Draw shapes
    
    #End of draw shapes section
    self.pyzoom.render(self.WIN, (100, 100))
    pygame.display.update()
```
We will get back to drawing shapes later in this tutorial.

Let me explain this line of code

```python
self.pyzoom.render(self.WIN, (0, 0))
```

This line adds generated surface to main window.
The second parameter are coordinates, so the main window knows where to blit generated surface.

There is an alternative way to blit generated image:

```python
self.WIN.blit(self.pyzoom.generate_surface(),(0,0))
```

But this way is not recommended because it only works when we are blitting generated surface at (0,0) coordinates.

So I recommend you to always use first method, that way pyZoom knows where it's canvas is located, and it allows you to blit generated image at any coordinates on the screen.

## Let's take a look on how to draw shapes with pyZoom.

To draw any of shapes listed bellow ad corresponding code to draw shapes section in refresh_window.

- ### Line

```python
self.pyzoom.draw_line(color, x1, y1, x2, y2, width)
```

Input data:

    color = color of the line (RGB).

    x1 = x coordinate of starting point

    y1 = y coordinate of starting point

    x2 = x coordinate of ending point

    y2 = y coordinate of ending point

    width = width of the line (default = 1)
    
Example:
```python
self.pyzoom.draw_line((255, 255, 255), 0, 0, 200, 200)
```

- ### Rectangle

```python
self.pyzoom.draw_rect(color, x, y, w, h, width)
```

Input data:

    color = color of the line (RGB).

    x1 = x coordinate of starting point

    y1 = y coordinate of starting point

    w = Width of the rectangle

    h = Height of the rectangle

    width = width of the border (default = 0). If the width=0, rectangle will be filled.
    
Example:
```python
self.pyzoom.draw_rect((255, 255, 255), 100, 100, 200.123, 200,5.567)
```

- ### Circle

```python
self.pyzoom.draw_circle(color, x, y, r, width)
```

Input data:

    color = color of the line (RGB).

    x1 = x coordinate of the center point

    y1 = y coordinate of the center point

    r = radius of the circle

    width = width of the border (default = 0). If the width=0, circle will be filled.
    
Example:
```python
self.pyzoom.draw_circle((255, 255, 255), 100.5, 100.5, 50.9087,90)
```

- ### Ellipse

```python
self.pyzoom.draw_ellipse(color, rect, width)
```

Input data:

    color = color of the line (RGB).
    
    rect = Tuple with 4 values. (x1,y1,r1,r2).
        x = x coordinate of the center point
        y = y coordinate of the center point
        r1 = width of the ellipse
        r2 = height of the ellipse

    width = width of the border (default = 0). If the width=0, ellipse will be filled.
    
Example:
```python
self.pyzoom.draw_ellipse((255, 255, 255), (100, 400.5, 80.123, 20),0)
```

- ### Polygon

```python
self.pyzoom.draw_polygon(color, points, width)
```

Input data:

    color = color of the line (RGB).
    
    points = array with tuples of 2. First value in each tuple is a x coordinate of the point. Second values is the y coordinate

    width = width of the border (default = 0). If the width=0, ellipse will be filled.
    
Example:
```python
self.pyzoom.draw_polygon((255, 255, 255), [(0.653, 789.234), (100,100), (345, 890.2)],0)
```

Great se we finished writing our class. Don't forget to make an instance of Window class.

```python
win = Window()
```

Thanks for using pyZoom <3