


```cpp
#include<iostream>

int min_element(int arr[],int begin,int end){
    int min_idx = begin;
    for(int j = begin+1;i<end;i++){
        if(arr[i]<arr[min_idx]) min_idx=i;
    }
    return min_idx;
}

void swap(int &a,int &b){//引用&
    int tmp = a;
    a = b;
    b = tmp;
}

void selection_sort(int arr[],int n){
    for(int i=0;i<n-1;i++){
        int min_idx = 1;
        for(int j= i+1;j<n;j++){
            if(arr[j]<arr[min_idx]) min_idx = j;
        }
        if(min_idx!=1){
            /*int tmp = arr[min_idx];
            arr[min_idx] = arr[i];
            arr[i] = tmp;*/
            swap(arr[min_idx])
        }
    }
}

void print_array(int arr[],int n){
    for(int i = 0;i<n;i++){
        std::cout<< arr[i] <<' ';

    }
    std::cout<<std::end;
}

int main(){
    int arr[] = {64,25,12,23,11};
    int n = sizeof(arr)/sizeof(arr[0]);

    selection_sort(arr,n);
    print_array(arr,n);
}
```

!!! note  "烦恼的东西"
    如果要把int改成double,那么必须要把相应的函数copy然后再改参数int为double，很烦而且很容易出错，所以C语言出现了不方便的地方

改进
```cpp
template<typename T>//函数模版
void swap(T & a, T & b){
    //same
}//这样就解决了hhh,用函数模版，但是要注意，每个函数前的函数模版都要写！
```

```cpp
struct Student{
    int id;
    std::string name;

    bool operator<(const Student& s){
        return id < s.id;
    }
}

bool operator<(const Student& s1,const Student& s2){
    return s1.id<s2.id;
}

std::ostream& operator<<(std::ostream & out,const Student& s){
    out << "(" <<s.id<<","<<s.name<<")";
}

int main(){
    Student arr[] = 
}

```

class就是来解决这些问题的
```cpp
//inheritance class Rectangle:public Shape
class Rectangle{
private:
    double w,h;
    double area,perimeter;
public:
    Rectangle(double w,double h): w(w),h(h) {}
    void calc_area(){
        area = w * h;
    }
    void
};

//class Circle:public Shape
class Circle{
private:
    double r;
public:
    Circle(double r):r(r){}
    void cal_area(){
        area = PI * r *r;
    }
    void cal_perimeter(){
        perimeter = 2* PI *r;
    }
};

class Triangle{
private:
    double a,b,c;

public:
    Triangle(double a,double b,double c): a(a),b(b),c(c){}
    void calc_area(){
        double s (a+b+c)/2;
        area = (s*(s-a)*(s-b)*(s-c));
    }
};

int main(){
    Rectangle arr[]={Rectangle(2,3),Rectangle(5,5)};
    Circle arr2[]={Circle(3)};
    Triangle arr3[] = {Triangle(2,5,4)};
    int n = sizeof(arr)/sizeof(arr[0]);

    for (auto &r:arr)
}

```

共性抽出来，新建class shape
```cpp

class Shape

```

!!! warning
    不可写成
    ```cpp
    Shape arr[] = {Rectangle(2,3),Rectangle(5,5),Circle(3),Triangle(2,5,4)};
    ```

    ```cpp
    Shape* arr[] = {new Rectangle(2,3),new Rectangle(5,5),new Circle(3),new Triangle(2,5,4)};

    int main(){
        for(  )
    }
    ```

关于多态
```cpp
class Shape{
protected:
    double
}
```