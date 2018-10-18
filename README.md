#  CUU

## C++ Serialization. Simplified.
* Serialization of classes **with a single line of code**
* Composite and polymorphic class support
* STL support
* Binary and Text formats (CUU, JSON, XML, ...)
* Easy to extend and customize

## Requirements
* Just **#include "cuu.h"** (header-only lib, no dependencies)
* C++17
* Targeting Visual Studio 2017 (MSVC 19.15) for now. 
* Portability should be straightforward. Please contact me, if you can port this to GCC/Clang.

## Motivation
A recent project required saving and reloading relatively large custom data 
* in binary format (for performance reasons) 
* in human readable form (for easy inspection)

I spent weeks with boost.serialization, cereal and libs11n. But, for various reasons, they just did not cut it. So, I finally decided to roll my own, making use of crucial C++11/C++17 features. This is the result. 

**The focus is on simplicity, usability and more simplicity!**

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
Equip them with CUU funtionality 
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
The same goes for JSON, XML and binary formats. Here is the XML version:
```
SaveToFile<XML>( "Customer.xml", MainCustomer );
```
```
<?xml version="1.0"?>
<root>
    <Name>Carl C. Cuttler</Name>
    <VIP>true</VIP>
    <OrderHistory>
        <Order>
            <ID>5001</ID>
            <Total>1.99</Total>
        </Order>
        <Order>
            <ID>5002</ID>
            <Total>2.79</Total>
        </Order>
        <Order>
            <ID>5003</ID>
            <Total>3.69</Total>
        </Order>
    </OrderHistory>
</root>
```
