#  CUU

## C++ Serialization. **C**lean. **U**sable. **U**niversal.
* Serialization of classes *with a single line of code*
* Composite and polymorphic class support
* STL support
* Binary and Text formats (CUU, JSON, XML, ...)
* Easy to extend and customize

## Requirements
* Just *#include "cuu.h"* (header-only lib, no dependencies)
* C++17
* Targeting Visual Studio 2017 and 2019 (Portability to GCC/Clang should be straightforward)

## Motivation
A recent project required saving and reloading relatively large custom data 
* in binary format (for performance reasons) 
* in human readable form (for occasional easy inspection)

I spent weeks with boost.serialization, cereal and libs11n. But, for various reasons, they just did not cut it. 
So, I finally decided to roll my own, making use of crucial C++11/C++17 features. This is the result. 

**The focus is on simplicity, usability and more simplicity!**

## Formats
Supported formats out of the box are: Text, Binary, JSON and XML. 
You can define your own custom format, if it is not too crazy.

## Example
Consider the following Order and Customer classes
```
class Order
{
public:
    int     ID;
    double  Total;
};

class Customer
{
public:
    std::string         Name;
    bool                VIP;
    std::vector<Order>  OrderHistory;
};
```
Equip them with **CUU functionality**
```
#include "cuu.h"
CUU( Order, ID, Total );
CUU( Customer, Name, VIP, OrderHistory );
```
Create a customer and save it...
```
MainCustomer = Customer( "Carl C. Cuttler", true, { {5001, 1.99}, {5002, 2.79}, {5003, 3.69} } );
SaveToFile<CUU>( "Customer.cuu", MainCustomer );
```
resulting in the file "Customer.cuu":
```
{
    Name: "Carl C. Cuttler", 
    VIP: true, 
    OrderHistory: 
    [
        {
            ID: 5001, 
            Total: 1.99
        }, 
        {
            ID: 5002, 
            Total: 2.79
        }, 
        {
            ID: 5003, 
            Total: 3.69
        }
    ]  
}
```
...to be loaded again later by
```
Customer MainCustomer;
LoadFromFile<CUU>( "Customer.cuu", MainCustomer );
```

## STL Support
All Standard Library containers and most data types are supported out of the box. The following will just work as expected
```
std::vector<Customer> CustomerList; 
SaveToFile<CUU>( "CustomerList.cuu", CustomerList );
```

## Polymorphic types
You can easily serialize polymorphic pointers, e.g. contained in a list.
Again, a single line of code is enough to register a base class (Shape) and its derived classes (Circle, Line, Triangle, Polygon):
```
CUU_REGISTER_POLYMORPHISM( Shape, Circle, Line, Triangle, Polygon );
```

## Easily Extendable
Simple classes (POD) are handled as one-liners as seen above. 
Looking at the code for supported types like std::vector or std::pair, you should be able to easily extend functionality to your more complex custom types. Custom split Save/Load functions as well as Get/Set methods are available.
