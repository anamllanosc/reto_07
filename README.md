# reto_07
```python
class MenuItem:
    def __init__(self,name:str,price:int):
        self.name=name #Nombre del producto
        self.price=price #Precio del producto

class Beverage(MenuItem): #Clase derivada de MenuItem (Bebidas)
    def __init__(self,name,price,temperature:str):
        super().__init__(name,price)# Hereda el inicializador de la super clase MenuItem
        self.temperature=temperature# Temperatura -> Atributo de unico de la clase Beverage

class Appetizer(MenuItem): #Clase derivada de MenuItem (Entradas)
    def __init__(self,name,price,pieces:int): 
        super().__init__(name,price) # Hereda el inicializador de la super clase MenuItem
        self.pieces=pieces# Numero de piezas -> Atributo de unico de la clase Appetizer

class MainCourse(MenuItem): #Clase derivada de MenuItem (Plato principal)
    def __init__(self,name,price,accompainments):
        super().__init__(name,price) # Hereda el inicializador de la super clase MenuItem
        self.accompainments=accompainments # Acompañamientos -> Atributo de unico de la clase MainCourse


class Order:
    def __init__(self):

        self.order_list=[] #Lista vacia para guardar los productos ingresados por el usuario

    def add_item(self): #metodo para crear la orden
        
        while True:
            item= str(input("Ingrese el producto que desea agregar a su pedido o (fin) para terminar: "))
            if item.lower()=="fin":
                break #El pedido terminara de hacerse cuando el usuario escriba "fin"
            elif item.lower() in menu_items:
                self.order_list.append(menu_items[item.lower()]) #Los productos de agregan a la lista "order_list unicamente si hacen parte de las claves del diccionario "menu_items"
                                                                      #A la lista se agraga el nombre objeto que corresponidia a la clave ingresada por el usiario
            else:
                print(f"El producto '{item}' no esta en el menu. Ingrese nuevamente su pedido: ")
                True #En caso de que el producto ingresado no corresponda a ninguna de las claves del diccionario, se guardara lo 
                     #anteriormente ingresado y se volvera a hacer el pedido.
        print("\n")
        print (f"-> Order: {[item.name for item in self.order_list]}")#-> Order: (lista con los nombres de los objetos)
    
    

    def total_bill(self): #metodo para calcular el valor total a pagar por el pedido

        self.bill=0 #se inicializa la la cuenta en 0
        for product in self: #por cada producto en la lista de la orden del usuario
                self.bill+=product.price #se accede al atributo "price" de cada uno de los objetos y se acumulan a 0.
        
        print(f"->TOTAL A PAGAR = ${self.bill}") #"->TOTAL A PAGAR = $suma de los precios"

    def discounts(self): #metodo para hacer descuentos al pedido
        beverage_count=sum(isinstance(item, Beverage) for item in self.order_list)
        # Suma 1 al contador de bebidas si el elemento en la orden es una instancia de la clase Beverage
        appetizer_count=sum(isinstance(item, Appetizer) for item in self.order_list)
        # Suma 1 al contador de bebidas si el elemento en la orden es una instancia de la clase Appetizer
        main_course_count=sum(isinstance(item, MainCourse) for item in self.order_list)
        # Suma 1 al contador de bebidas si el elemento en la orden es una instancia de la clase MainCourse
        
        discount=0
        # Aplica descuentos basados en la cantidad y tipo de productos

        if   beverage_count== appetizer_count== main_course_count and 3>=beverage_count >=1:
            
            #si se piden entre uno y tres productos de cada categoria: descuento del 5%
            discount=(self.bill*0.05)#valor a restar del total
            final_bill=self.bill-discount#total con el descuento aplicado
            print(f"Descuento a aplicar: -{discount}$")#imprime el valor a restar del total
            print(f"TOTAL A PAGAR (Descuento aplicado)=> {final_bill}$") #imprime el total con el descuento aplicado
            

        elif beverage_count== appetizer_count== main_course_count and beverage_count >=4:

            #si se piden 4 o mas productos cateroria: descuento del 10%
            discount=(self.bill*0.10)
            final_bill=self.bill-discount
            print(f"Descuento a aplicar: -{discount}$")
            print(f"TOTAL A PAGAR (Descuento aplicado)=>  {final_bill}$") 



        elif beverage_count==main_course_count and beverage_count==4:

            #si se piden 4 bebidas y cuatro platos principales: descuento del 8%
            discount=(self.bill*0.08)
            final_bill=self.bill-discount
            print(f"Descuento a aplicar: -{discount}$")
            print(f"TOTAL A PAGAR (Descuento aplicado)=> {final_bill}$")

        else: 
            print(f"Descuento a aplicar: 0$")# En caso de que no se cumpla con ninguno de los requisitos para descuento
                                             #se imprimira que el "Descuento a aplicar es igual a 0$""

    def __iter__(self):
        return OrderIterator(self.order_list)

class OrderIterator:
    def __init__(self, order_list):
        self.order_list = order_list
        self.index = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self.index < len(self.order_list):
            item = self.order_list[self.index]
            self.index += 1
            return item
        else:
            raise StopIteration
    

#Objetos de la clase Beverages
coffe=Beverage("Coffe",3000,"Hot")
chocolate=Beverage("Chocolate",3500,"Hot")
lemonade=Beverage("Lemonade",2500,"Cold")
water=Beverage("Water",2000,"Hot")

#Objetos de la clase Appetizer
spicy_shrimp=Appetizer("Spicy Shrimp",20000,"6")
mini_quiches=Appetizer("Mini Quiches",18000,"4")
fried_platains=Appetizer("Fried Platains",15000,"8")
capresse_salad=Appetizer("Capresse Salad",15000,"1")

#Objetos de la clase Main Course
grilled_salmon=MainCourse("Grilled Salmon",38000,"Accompainments= Mashed Potatoes and Mango Salad")
BBQ_ribs=MainCourse("Bbq Ribs",34000,"Accompainments= Fries")
stuffed_chicken=MainCourse("Stuffed Chicken",30000,"Accompainments= Parsley Rice")
vegetarian_curry=MainCourse("Vegetarian Curry",30000,"Accompainments= Pita Bread")

#Objeto de la clase orden 
order = Order()

#Diccionario con todos los productos del menu
menu_items={"coffe":coffe,"chocolate":chocolate,"lemonade":lemonade,"water":water,
            "spicy shrimp":spicy_shrimp,"mini quiches":mini_quiches,"fried platains":fried_platains,"capresse salad":capresse_salad,
            "grilled salmon":grilled_salmon,"bbq ribs":BBQ_ribs,"stuffed chicken":stuffed_chicken,"vegetarian curry":vegetarian_curry}


#Impresion del menu
print ("\nMENU:\n")
for item in menu_items:
    print(f"-{menu_items[item].name}")

#Lista de descuentos
print("\n")
print("DESCUENTOS:\n-Entre uno y tres productos de cada categoria: 5% de descuento\n-Desde cuatro productos de cada categoria: 10% de descuento\n-Cuatro bebidas y cuatro platos principales:8% de descuento")
print("\n")
    
order.add_item()# Llama al método add_item() de la clase Order para agregar productos a la orden.
order.total_bill()# Calcula el total de la factura llamando el método total_bill() de la clase Order.
order.discounts()#Calcula los descuentos llamando al método discounts() de la clase Order.
```
