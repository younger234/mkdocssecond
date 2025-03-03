


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

# STL

!!! note "what is STL"
    standard


## container

- sequential
  array(static),vector(dynamic)
  deque(double-ended queue)
  forward
- associative
  set
  map
  multiset,multimap
- unordered associative
  hashed by keys
  unordered_set,unordered_map
- adaptors
  stack,

### using the vector

```cpp
#include<iostream>
#include<vector>
using namespace std;

int main(){
    vector<int> evens{2,4,6,8};
    evens.push_back(20);
    evens.push_back(22);
    evens.insert(evens.begin()+4,5,10);

    for(int i = 0;i<evens.size();++i)
        cout << evens[i] << ' ';
    cout << endl;

    for(vector<int>::iterator it = evens.begin();it<evens.end();it++)//if list,we can't use < iterator,we can use != iterator
        cout << *it << ' ';
    cout << endl;

    for(int e:evens)//迭代器的作用
        cout<< e << ' ';
}
```

### using the list

### using the map
- collection of key-value pairs
- lookup by key, and retrieve a value

```cpp
#include<iostream>
#include<map>
#include<string>
using namespace std;

itn main(){
    map<string,int> price;
    price["apple"] = 3;
    price["orange"] = 5;
    price["banana"] = 1;

    for(const auto& pair: price)
    cout<<"{"<<pair.first<<":"<<pair.second<<"}";//直接用pair代替

    string item;
    int total = 0;
    while(cin>> item){
        total += price[item];
    }
    for(const auto&)
}

```

```cpp
int main(){
    map<string,int> word_map;
    for(const auto& w:{"we","are","not","humans","we","are","robots","!!"})
        ++word_map[w];
    for(const auto& [word,count] : word_map)
        cout<<count<<"occurrences of word"<<word<<endl;
}

```

```cpp
bool is_balanced(const string &s){
    stack<char> st;
    for(char c:s){
        if(c=='('||c=='{'c=='['){
            st.push(c);
        }
        else if(c==')'||c=='}'||c==']'){
            if (st.empty())
                return false;
            char top= st.top();
            st.pop();
            if((c==')'&&top !='(')||(c==']'&&top!='[')||(c=='}'&&top!='{')
                return false;
        }
    }
}

int main(){
    string test1 = "a(b{c[d]e}f)g";
    string test2 = "x(y{z[]})";

    cout << "test 1: "<<(is_balanced(test1))?"Balaced":"Unbalanced"<<endl;
    cout << "test 2: "<<(is_balanced(test2))?"Balaced":"Unbalanced"<<endl;
}
```

```cpp
class Stack
{
public:
    virtual int & top()=0;
    virtual bool empty() const = 0;
    virtual size_t size() const = 0;
    virtual void push(const T& value) = 0;
    virtual void pop()=0;
}

template<typename T>//template<typename T,typename Container = std::deque<T>>
class c_stack : public Stack<T>
{
public:
    T& top() override{return c.back();}
    bool empty() const override{return c.empty();}
    size_t size() const override{return c.size();}
    void push(const T&value) override{c.push_back(value);}
    virtual void pop() override{c.pop_back();}
}

```

## Algorithms
- works on a range defined as[first,last).
- for_each,find,···

```cpp
int main(){
    vector<int> v = {1,2,3,5};
    vector<int> u;

    reverse(v.begin(),v.end());
    copy(v.begin(),v.end(),back_inserter(u));
    copy(u.begin(),u.end(),ostream_iterator<int>(cout,", "));

    list<int> l;
    copy(v.begin(),v.end(),front_inserter(l));
    copy(l.begin(),l.end(),ostream_iterator<int>(cout,", "));
}
```