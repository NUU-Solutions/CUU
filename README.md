#  CUU

## C++ Serialization. **C**lean. **U**niversal. **U**sable.
* Serialization of classes **with a single line of code**
* Composite and polymorphic class support
* Full STL support
* Supported formats: Text, JSON, XML, as well as Binary
* Easy to extend and customize

## Requirements
* Just **#include "cuu.h"** 
* Header-only library, no dependencies
* C++20

## Motivation
A project required saving and reloading relatively large custom data 
* in binary format (for performance reasons) 
* in human readable form (for occasional easy inspection)

I spent weeks with boost.serialization, cereal and libs11n. But, for various reasons, they just did not cut it. 
So, I finally decided to roll my own, making use of crucial C++11/C++17/C++20 features. This is the result. 

**The focus is on simplicity and usability!**

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
cuu::SaveToFile<Text>( "Customer.cuu", MainCustomer );
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
cuu::LoadFromFile<Text>( "Customer.cuu", MainCustomer );
```

## Name-value pairs vs. simple list
You always have both options. `CUU( Order, ID, Total );` as seen above, 
or `CUU_LIST( Order, ID, Total );` resulting in just `[5001, 1.99]`
Containers are always treated as lists.

## Member access
Simple class members are handled as one-liners as seen above. 
For private members, access must be granted by putting `CUU_ACCESS` in the class
Alternatively, get and set member-functions provided by your class can be used.

## Inheritance
Easily include inherited members.
This will inlcude all members provided for `Shape` in the cuu-ing of `Circle`:
```
CUU_REGISTER_INHERITANCE( Circle, Shape );
```

## Polymorphic types
Easily serialize polymorphic pointers (std::unique_ptr, std::shared_ptr and raw pointers).
The following line will register Shape pointers for polymorphic cuu-ing:
```
CUU_REGISTER_POLYMORPHISM( Shape, Circle, Line, Triangle, Polygon );
```

## STL Support
All Standard Library containers and data types are supported out of the box. 
The following will just automatically work as expected:
```
std::vector<Customer> CustomerList; 
std::pair<int,Customer> IDCustomerPair; 
std::optional<Customer> OptionalCustomer; 
cuu::SaveToFile<Text>( "CustomerList.cuu", CustomerList, IDCustomerPair, OptionalCustomer );
```

## Customizable/Extendable
You can specify per class whether your prefer linebreaks or inline for the output.
You can also specify a floating-point format or print integers in hexadecimal or any base.
You can even define a custom overall format with different delimiters, etc., if it's not too crazy.
Looking at the supplied code for STL containers, pointers, and data types, you should be able
to easily write similar code for your own custom containers and non-standard data types.

