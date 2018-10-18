#  CUU

## C++ Serialization. Simplified.
* Serialize classes with **a single line of code**
* STL support
* Binary and Text formats (JSON, XML, CUU, ...)
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
Customer MainCustomer{"Carl Cuttler", true, {1,2,3}};
SaveToFile<CUU>( "Customer.cuu", MainCustomer );
```
resulting in the CUU file
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
Save to binary, JSON, or XML if you want to.
