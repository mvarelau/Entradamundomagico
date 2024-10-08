```python
import numpy as np
import math
class Point:
    definition: str = "Entidad geometrica abstracta que representa una ubicación en un espacio."
    def __init__(self, x=0, y=0):
        self.x = x
        self.y = y

    def compute_distance(self,point_1,point_2):
        distance= (((point_1.x-point_2.x)**2)+((point_1.y - point_2.y)**2))**0,5
        return distance 


class Line:
    def __init__(self, lenght:float, slope:float, start:Point, end:Point ):
       self.lenght= lenght 
       self.slope = slope
       self.start = start # As the left point of the line
       self.end = end
    
       

class Shape:
    def __init__(self,is_regular:bool,vertices:list,edges:list,inner_angles:list):
        self.is_regular = is_regular
        self.point = Point()
        self.line = Line()
        self.vertices=vertices
        self.edges = edges
        self.inner_angles = inner_angles

    
    def compute_area(self):
        pass

    def compute_perimeter(self,edges)->float:
        for e in edges:
            perimeter= e+e
        return perimeter

    

class Triangle(Shape):
    def __init__(self,is_regular:bool,vertices:list,edges:list,inner_angles:list,cant_vertices:int=3):
        super().__init__(is_regular,vertices,edges,inner_angles)
        self.cant_vertices=cant_vertices
    
    def compute_area(self):
        pass
    
    def compute_inner_angles(self, edges)->float:
        if len(edges) == 3:
            #I use the cosine teorem to calcole each angle
            Angle_A=math.degrees(np.arccos((((edges[1])**2)+(edges[2]**2)-(edges[0]**2))/(2*edges[1]*edges[2])))
            Angle_B=math.degrees(np.arccos((((edges[0])**2)+(edges[2]**2)-(edges[1]**2))/(2*edges[0]*edges[2])))
            Angle_C= math.degrees(np.arccos((((edges[0])**2)+(edges[1]**2)-(edges[2]**2))/(2*edges[0]*edges[1])))
        inner_angles=[Angle_A,Angle_B,Angle_C]
        return inner_angles
    
    def compute_perimeter(self, edges) -> float:
        return super().compute_perimeter(edges)

class Isosceles(Triangle):
    def __init__(self,is_regular:bool,vertices:list,edges:list,inner_angles:list,cant_vertices:int=3):
        super().__init__(is_regular,vertices,edges,inner_angles,cant_vertices)
        len(edges)==len(set(edges))+1 # function set retorns the list without repetitions
    
    def compute_area(self,edges):
        dup = [x for i, x in enumerate(edges) if i != edges.index(x)]#this is to identify the corner repeted
        r_corners=dup[0]#this is the sice of the repeted corners
        edges.remove(r_corners)
        edges.remove(r_corners)#now edeges just has one numbes, the corner that doesn´t repeat
        s_corner=edges[0]
        h=((r_corners**2)-((s_corner**2)/4))**0.5
        area=(h*s_corner)/2
        return area
    
class Equilateral(Triangle):
    def __init__(self,is_regular:bool,vertices:list,edges:list,inner_angles:list,cant_vertices:int=3):
        super().__init__(is_regular,vertices,edges,inner_angles,cant_vertices)
        len(edges)==len(set(edges))+2

    def compute_area(self,edges):
        l=edges[0]
        area=(l**2)*((3**0.5)/4)
        return area
    
class Scalene(Triangle):
    def __init__(self,is_regular:bool,vertices:list,edges:list,inner_angles:list,cant_vertices:int=3):
        super().__init__(is_regular,vertices,edges,inner_angles,cant_vertices)
        len(edges)==len(set(edges))

    def compute_area(self,edges):
        #Herón formula
        s= (Shape.compute_perimeter(edges=edges))/2
        area=(s*(s-edges[0])*(s-edges[1])*(s-edges[2]))**0.5
        return area
    
class TriRectangle(Triangle):
    def __init__(self,is_regular:bool,vertices:list,edges:list,inner_angles:list,cant_vertices:int=3):
        super().__init__(is_regular,vertices,edges,inner_angles,cant_vertices)

    def compute_area(self,edges):
        inner_angles=Triangle.compute_inner_angles(Triangle,edges=edges)
        for a in inner_angles:
            if a ==90:
                if inner_angles.index(a)==0: #with this if cicle y can find wich edges use as hight and base
                    h= edges[1]
                    b=edges[2]

                elif inner_angles.index(a)==1:
                    h=edges[0]
                    b=edges[2]

                else:
                    h=edges[0]
                    b=edges[1]
        area= (h*b)/2
        return area 
        
#Desde acá me toca arreglar
class Rectangle(Shape):
    def __init__(self, width:float, height:float, point):
        self.width = width
        self.height = height
        self.point = point
 
    def Compute_Area(self,width, height):
        print("The perimeter of the rectangle is " + str(width*height))

    def Compute_Perimeter(self,widht, height):
        print("The perimeter of the rectangle is " + str(2*widht + 2*height))

    def Compute_Interfencfe_point(self, point: Point) -> bool:
        if (self.point.x <= point.x <= self.point.x + self.width) and (self.point.y <= point.y <= self.point.y + self.height):
            return True
        else:
            return False    


class Square(Rectangle): # we use the inheritance to bring the methods and attributes of the class Rectangle
    def __init__(self,width:float, height:float):
        super().__init__(width,height)    
        self.width = width
        self.height = height
        width==height

    def Compute_Area(self, width, height):
        return super().Compute_Area(width, height)
    
    def Compute_Perimeter(self, widht, height):
        return super().Compute_Perimeter(widht, height)
    
    def Compute_Interfencfe_point(self, point: Point) -> bool:
        return super().Compute_Interfencfe_point(point)
class MenuItem:
    def __init__(self, name: str, price: float, description: str):
        self.name = name
        self.price = price
        self.description = description

class Beverage(MenuItem):
    def __init__(self, name: str, price: float, description: str, kind: str):
        super().__init__(name, price, description)
        self.kind = kind

class BreakFast(MenuItem):
    def __init__(self, name: str, price: float, description: str, flavor: str):
        super().__init__(name, price, description)
        self.flavor = flavor

class Lunch(MenuItem):
    def __init__(self, name: str, price: float, description: str, protein: str):
        super().__init__(name, price, description)
        self.protein = protein

class Order:
    def __init__(self):
        self.order_list = []

    def add_item(self, item: MenuItem):
        self.order_list.append(item)

    def total_amount(self):
        total = sum(item.price for item in self.order_list)
        return total

    def apply_discounts(self):
        total_amount = self.total_amount()
        if len(self.order_list) >= 4:
            total_amount -= total_amount * 0.10  # 10% de descuento si hay 4 o más ítems
        return total_amount

    def print_receipt(self):
        print("Receipt:")
        for item in self.order_list:
            print(f"{item.name}: ${item.price:.2f} - {item.description}")
        print(f"Total Amount: ${self.total_amount():.2f}")
        print(f"Total after discounts: ${self.apply_discounts():.2f}")

if __name__ == "__main__":
    # Creando ítems de ejemplo
    strawberry = Beverage(name="Strawberry Juice", price=5, description="Fresh strawberry juice", kind="Cold")
    coffee = Beverage(name="Coffee", price=4, description="Hot brewed coffee", kind="Hot")
    pancakes = BreakFast(name="Nutella Pancakes", price=15, description="Pancakes with Nutella", flavor="Sweet")
    eggs = BreakFast(name="Eggs", price=13, description="Scrambled eggs", flavor="Salty")
    ham_creps = Lunch(name="Ham Creps", price=13, description="Ham crepes with cheese", protein="Ham")

    # Creando una orden
    order = Order()
    order.add_item(strawberry)
    order.add_item(coffee)
    order.add_item(pancakes)
    order.add_item(eggs)
    order.add_item(ham_creps)

    # Imprimiendo el recibo
    order.print_receipt()

```
# Developed restaurant
```python
class MenuItem:
    def __init__(self, name: str, price: float, description: str):
        self.name = name
        self.price = price
        self.description = description

class Beverage(MenuItem):
    def __init__(self, name: str, price: float, description: str, kind: str):
        super().__init__(name, price, description)
        self.kind = kind

class BreakFast(MenuItem):
    def __init__(self, name: str, price: float, description: str, flavor: str):
        super().__init__(name, price, description)
        self.flavor = flavor

class Lunch(MenuItem):
    def __init__(self, name: str, price: float, description: str, protein: str):
        super().__init__(name, price, description)
        self.protein = protein

class Order:
    def __init__(self):
        self.order_list = []

    def add_item(self, item: MenuItem):
        self.order_list.append(item)

    def total_amount(self):
        total = sum(item.price for item in self.order_list)
        return total

    def apply_discounts(self):
        total_amount = self.total_amount()
        if len(self.order_list) >= 4:
            total_amount -= total_amount * 0.10  # 10% de descuento si hay 4 o más ítems
        return total_amount

    def print_receipt(self):
        print("Receipt:")
        for item in self.order_list:
            print(f"{item.name}: ${item.price:.2f} - {item.description}")
        print(f"Total Amount: ${self.total_amount():.2f}")
        print(f"Total after discounts: ${self.apply_discounts():.2f}")

if __name__ == "__main__":
    # Creando ítems de ejemplo
    strawberry = Beverage(name="Strawberry Juice", price=5, description="Fresh strawberry juice", kind="Cold")
    coffee = Beverage(name="Coffee", price=4, description="Hot brewed coffee", kind="Hot")
    pancakes = BreakFast(name="Nutella Pancakes", price=15, description="Pancakes with Nutella", flavor="Sweet")
    eggs = BreakFast(name="Eggs", price=13, description="Scrambled eggs", flavor="Salty")
    ham_creps = Lunch(name="Ham Creps", price=13, description="Ham crepes with cheese", protein="Ham")

    # Creando una orden
    order = Order()
    order.add_item(strawberry)
    order.add_item(coffee)
    order.add_item(pancakes)
    order.add_item(eggs)
    order.add_item(ham_creps)

    # Imprimiendo el recibo
    order.print_receipt()
```


   



