#  CUU

## C++ Serialization. Simplified.
* Serialize classes with **a single line of code**
* STL support
* Binary and Text formats (CUU, JSON, XML, ...)
* Easy to extend

## Example
Consider a simple customer class
```
class Customer
{
public:
    std::string         Name;
    bool                VIP = false;
    std::vector<int>    PurchaseHistory;
};
```
Equip it with CUU funtionality
```
#include "cuu.h"
CUU( Customer, Name, VIP, PurchaseHistory );
```
Create a customer and save it
```
Customer MainCustomer{ "Carl Cuttler", true, {1,2,3} };
SaveToFile<CUU>( "Customer.cuu", MainCustomer );
```
resulting in the file "Customer.cuu":
```
{
    MainCustomer: 
    {
        Name: "Carl Cuttler", 
        VIP: true, 
        PurchaseHistory: 
        [
            1, 
            2, 
            3
        ]
    }
}
```
and load it again later
```
Customer MainCustomer;
LoadFromFile<CUU>( "Customer.cuu", MainCustomer );
```
