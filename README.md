# CHALLENGE_05
1. Create a package with all code of class Shape, this exersice should be conducted in two ways:
A unique module inside of package Shape
Individual modules that import Shape in inheritates from it.
-
The first thing that I did was create a folder with a package named shape_pc then I added a file "_ _init_ _.py inside the package. This file indicates to python to treat directories as modules.
```
SHAPEPCK/
├── shape_pc/
│   ├── __init__.py
│   ├── shape_module.py
└── main.py
```
as ypu can tell there is a single module inside the package, this unique module has the classes, Point, Line and Shape.
```python
import math

class Point:
    def __init__(self, x:int, y:int):
        self._x = x
        self._y = y
        
    def get_x(self):
        return self._x
    
    def get_y(self):
        return self._y
    
    def compute_distance(self, other_point):
        return ((self._x - other_point.get_x())**2 + (self._y - other_point.get_y())**2)**0.5

class Line:
    def __init__(self, point1:Point, point2:Point):
        self._length = point1.compute_distance(point2)
        self._start = point1
        self._end = point2
    
    def get_start(self):
        return self._start
    
    def get_end(self):
        return self._end
    
    def get_length(self):
        return self._length    
    
class Shape:
    def __init__(self, vertices):
        self._vertices = vertices
        self._edges = [Line(vertices[i], vertices[(i+1)%len(vertices)]) for i in range(len(vertices))] 
        
    def inner_angles(self):
        self._inner_angles = []
        for i in range(len(self._vertices)):
            prev_point = self._vertices[i-1]
            current_point = self._vertices[i]
            next_point = self._vertices[(i+1)%len(self._vertices)]
            a = prev_point.compute_distance(current_point)
            b = current_point.compute_distance(next_point)
            c = next_point.compute_distance(prev_point)
            angle_radians = math.acos((a**2 + b**2 - c**2)/(2*a*b))
            angle_degrees = round(math.degrees(angle_radians))
            self._inner_angles.append(angle_degrees)
        return self._inner_angles
                
    def is_regular(self):
        first_angle = self._inner_angles[0]
        for angle in self._inner_angles:
            if angle != first_angle:
                return False
        return True        
    
    def get_edges(self):
        return self._edges
    
    def get_vertices(self):
        return self._vertices
    
    def get_inner_angles(self):
        return self._inner_angles
    
    def compute_area(self):
        pass
    
    def compute_perimeter(self):
        pass
    
    def compute_inner_angles(self):
        pass
```
then, out of the package, i started to create different modules for each figure, I thought that i could create a new package and put inside of it all the figures, but the instruction is clear when it says just one package, however here is a representation of what i did: 
```
├── shape_pc/
│   ├── __init__.py
│   ├── shape_module.py
└── main.py
├── equilateral_triangle.py
├── isosceles_triangle.py
├── rectangle_triangle.py
├── rectangle.py
├── scalene_triangle.py
├── square.py
└── triangle.py
```
now i will show you eache module and how i did the imports
### rectangle.py
```python
from shape_pc.shape_module import Shape
class Rectangle(Shape):
    def __init__(self, vertices):
        super().__init__(vertices)
        
    def compute_area(self):
        print(self._edges[0].get_length())
        return self._edges[0].get_length() * self._edges[1].get_length()
    
    def compute_perimeter(self):
        return 2*self._edges[0].get_length() + 2*self._edges[1].get_length()
    
    def compute_inner_angles(self):
        return self._inner_angles
```
In this case, the Shape class defined in the shape_module module within the shape_pc package is being accessed.
### module square.py
```python
from rectangle import Rectangle
class Square(Rectangle):
    def __init__(self, vertices):
        super().__init__(vertices)
    
    def compute_area(self):
        return self._edges[0].get_length()**2
    
    def compute_perimeter(self):
        return 4*self._edges[0].get_length()
    
    def compute_inner_angles(self):
        return self._inner_angles
```
### module triangle.py
```python
from shape_pc.shape_module import Shape
class Triangle(Shape):
    def __init__(self, vertices):
        super().__init__(vertices)
        
    def compute_area(self):
        s = self.compute_perimeter()/2
        a = self._edges[0].get_length()
        b = self._edges[1].get_length()
        c = self._edges[2].get_length()
        return (s*(s-a)*(s-b)*(s-c))**0.5
    
    def compute_perimeter(self):
        pass
```
### equilateral_triangle.py
```python
from triangle import Triangle
class EquilateralTriangle(Triangle):
    def __init__(self, vertices):
        super().__init__(vertices)
    
    def compute_area(self):
        return (3**0.5/4) * self._edges[0].get_length()**2 #Deducción a partir de partir el triángulo en dos triángulos rectángulos de 30° y 60°
    
    def compute_perimeter(self):
        return 3*self._edges[0].get_length()
    
    def compute_inner_angles(self):
        return self._inner_angles
```
### isosceles_triangle.py
```python
from triangle import Triangle
class IsoscelesTriangle(Triangle):
    def __init__(self, vertices):
        super().__init__(vertices)
        
    def compute_perimeter(self):
        return self._edges[0].get_length() + self._edges[1].get_length() + self._edges[2].get_length()
    
    def compute_area(self):
        return Triangle.compute_area(self)
    
    def compute_inner_angles(self):
        return self._inner_angles
```
### scalene_triangle.py
```python
from triangle import Triangle
class ScaleneTriangle(Triangle):
    def __init__(self, vertices):
        super().__init__(vertices)
        
    def compute_perimeter(self):
        return self._edges[0].get_length() + self._edges[1].get_length() + self._edges[2].get_length()
    
    def compute_area(self):
        return Triangle.compute_area(self)
    
    def compute_inner_angles(self):
        return self._inner_angles
```
### rectangle_triangle.py
```python
from triangle import Triangle
class RectangleTriangle(Triangle):
    def __init__(self, vertices):
        super().__init__(vertices)
        
    def compute_perimeter(self):
        return self._edges[0].get_length() + self._edges[1].get_length() + self._edges[2].get_length()
    
    def compute_area(self):
        return Triangle.compute_area(self)
    
    def compute_inner_angles(self):
        return self._inner_angles
```
## Main
Finally i created the file named "main" that is the main script that runs in the project. This allows the script to behave as the entry point of the program, that is, the place where code execution begins.
```python
from shape_pc.shape_module import Point 
from isosceles_triangle import IsoscelesTriangle 
from equilateral_triangle import EquilateralTriangle
from scalene_triangle import ScaleneTriangle
from rectangle_triangle import RectangleTriangle
from rectangle import Rectangle
from square import Square


def main(): 
    p1 = Point(0, 0)
    p2 = Point(2, 5)
    p3 = Point(4, 0)

    isosceles = IsoscelesTriangle([p1, p2, p3])

    print("ISOSCELES TRIANGLE")
    print("Area:", isosceles.compute_area())
    print("Perimeter:", isosceles.compute_perimeter())
    print("Inner angles:", isosceles.compute_inner_angles())
    print("\n")

    r1 = Point(0, 0)
    r2 = Point(1, 0)
    r3 = Point(0.5, 0.8660254037844386)  # This creates an equilateral triangle with side length 1

    equilateral = EquilateralTriangle([r1, r2, r3])

    print("EQUILATERAL TRIANGLE")
    print("Area:", equilateral.compute_area())
    print("Perimeter:", equilateral.compute_perimeter())
    print("Inner angles:", equilateral.compute_inner_angles())
    print("\n")

    m1 = Point(2, 0)
    m2 = Point(3, 5)
    m3 = Point(6, 0)

    scalene = ScaleneTriangle([m1, m2, m3])

    print("SCALENE TRIANGLE")
    print("Area:", scalene.compute_area())
    print("Perimeter:", scalene.compute_perimeter())
    print("Inner angles:", scalene.compute_inner_angles())
    print("\n")

    f1 = Point(0, 0)
    f2 = Point(6, 4)
    f3 = Point(6, 0)

    tri_rectangle = RectangleTriangle([f1, f2, f3])

    print("RECTANGLE TRIANGLE")
    print("Area:", tri_rectangle.compute_area())
    print("Perimeter:", tri_rectangle.compute_perimeter())
    print("Inner angles:", tri_rectangle.compute_inner_angles())
    print("\n")

    b1 = Point(0, 0)
    b2 = Point(6, 2)
    b3 = Point(6, 0)
    b4 = Point(0, 2)

    rectangle = Rectangle([b1, b2, b3, b4])

    print("RECTANGLE")
    print("Area:", rectangle.compute_area())
    print("Perimeter:", rectangle.compute_perimeter())
    print("Inner angles:", rectangle.compute_inner_angles())
    print("\n")

    g1 = Point(0, 0)
    g2 = Point(0, 4)
    g3 = Point(4, 4)
    g4 = Point(4, 0)

    square = Square([g1, g2, g3, g4])

    print("SQUARE")
    print("Area:", square.compute_area())
    print("Perimeter:", square.compute_perimeter())
    print("Inner angles:", square.compute_inner_angles())

if __name__ == "__main__":
  main()  
```
### Explanation 
1. from shape_pc.shape_module import Point: This line is importing the Point class from the shape_module module in the shape_pc package.

2. from isosceles_triangle import IsoscelesTriangle: This line is importing the IsoscelesTriangle class from the isosceles_triangle module.

3. from equilateral_triangle import EquilateralTriangle: This line is importing the EquilateralTriangle class from the equilateral_triangle module.

4. from scalene_triangle import ScaleneTriangle: This line is importing the ScaleneTriangle class from the scalene_triangle module.

5. from rectangle_triangle import RectangleTriangle: This line is importing the RectangleTriangle class from the rectangle_triangle module.

6. from rectangle import Rectangle: This line is importing the Rectangle class from the rectangle module.

7. from square import Square: This line is importing the Square class from the square module.

These classes are used in the rest of the code in main.py. By importing them at the top of the file, they become available for use throughout the rest of this file.
