# memory model

```cpp
#include<cstdlib>
#include<iostream>
using namespace std;

int globalx = 10;

int main(){
    static int staticx = 3;
    int localx = 5;
    int *px = (int*) malloc (sizeof(int));

    cout << "&globalx="
}

```