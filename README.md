#  CUU

## C++ Serialization. Simplified.
* Serialize classes with **a single line of code**
* Composition works out of the box
* STL support
* Binary and Text formats (CUU, JSON, XML, ...)
* Header-only library
* Easy to extend

## Requirements
* C++17
* Works with MSVC for now (making this potable should not be a big issue, please contact me)

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
Equip them with CUU funtionality (one line per class)
```
#include "cuu.h"
CUU( Order, ID, Total );
CUU( Customer, Name, VIP, OrderHistory );
```
Create a customer and save it (one line of code)
```
MainCustomer = Customer( "Carl C. Cuttler", true, { {5001, 1.99}, {5002, 2.79}, {5003, 3.69} } );
SaveToFile<CUU>( "Customer.cuu", MainCustomer );
```
resulting in the file "Customer.cuu":
```
{
    MainCustomer: 
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
}
```
and load it again later by just
```
Customer MainCustomer;
LoadFromFile<CUU>( "Customer.cuu", MainCustomer );
```
The same goes for JSON, XML or binary formats

## Motivation
A recent project of mine required saving relatively large files 
* in binary format (performance reasons) 
* in human readable form (for easy inspection)

After spending weeks with boost.serialization, cereal and libs11n, which just did not cut it,
I finally decided to roll it on my own.
The 
