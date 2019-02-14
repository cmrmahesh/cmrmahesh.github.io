---
published: true
---
## Hello

This page is for testing github pages and will be used for only that purpose.

'''c++
#include <iostream> 
using namespace std; 

class B 
{ 
public:	 
	B(const char* str = "\0") //default constructor 
	{ 
		cout << "Constructor called" << endl; 
	}	 
	
	B(const B &b) //copy constructor 
	{ 
		cout << "Copy constructor called" << endl; 
	} 
}; 

int main() 
{ 
	B ob = "copy me"; 
	return 0; 
} 
'''
