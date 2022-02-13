## c++ effective
+ C++为一个联邦语言，C基础语言、面向对象C、模板、STL  
+ 尽量以const、enum、inline替换#define  
    + #define后的宏，gdb调试时，不能获取该宏信息  
    + 对于单纯常量，最好以const对象或enums替换#define  
    + 对于形似函数的宏，最好改用inline函数替换  
+ 尽可能使用const  
    + const能够帮编译器甄别更多的错误  
    + 成员函数后面加了const，表示不能更改类中的成员  
    + 如果加了const，又非要更改类中的成员，可以把成员定义为mutable …  
    + **`const_case<>`** 将变量的const的属性去除  
+ 确定对象被使用前已先被初始化，构造函数初始化列表初始效率较构造函数中赋值效率更高  
+ 编译器可以暗自为class创建默认构造函数、拷贝构造函数、重载赋值操作符、析构函数  
+ 若不想使用编译器自动生成的函数，就该明确拒绝=delete  
+ 如果class带有任何虚函数，析构函数就应被设置为虚析构函数，多态的基类应该声明一个虚函数  
+ 如果一个被析构函数调用的函数可能会抛出异常，析构函数应该捕捉任何异常，然后吞下它们或结束程序  
+ 绝不在构造和析构过程中调用virtual函数  
+ 使类中赋值重载operater=返回return *this  
+ 在operator=中处理"自我赋值"  
+ 复制对象应确保复制对象内的所有成员变量以及所有基类成分都被复制  
+ 用智能指针auto_ptr和shared_ptr管理非数组对象，因为智能指针释放是采用delete而不是delete[]，其中auto_ptr对象进行拷贝或赋值时，被拷贝或赋值的对象会变为nullptr  
+ 在资源管理类中小心copying行为  
+ 在资源管理类中提供对原始资源的访问  
+ 成对使用new和delete时要采取相同形式，如果在new中使用[]，必须在相应的delete表达式中也使用[]  
+ 以独立语句将newed对象置入智能指针  
    + **`process(shared_ptr<Obj>(new obj), priority());`** 这种写法有问题，不能明确 **`new obj,priority(),shared_ptr<Obj>()`**，这三个步骤谁先执行，如果 **`priority()`** 执行在中间并抛出异常，new出来的结果将不能被释放  
+ 让接口容易被正确使用，不易被误用  
+ 设计class犹如设计type  
+ 让函数传常引用值替换传递值，内置类型、STL的迭代器和函数对象可以考虑传值  
+ 必须返回对象时，别妄想返回其reference  
+ 将成员变量声明为private  
+ 构造函数如果不声明为explicit，编译器可以进行隐式类型构造转换  
+ 对于操作符重载函数既可以是成员函数，也可以是非成员函数  
+ 考虑写出一个不抛异常的swap函数  
```cpp
//对swap函数进行特化的方式，
namespace std{
template<>
void swap<Widget>(Widget& a, Widget& b){}
}
```
+ 尽可能延后变量定义式的出现时间  
+ 尽量少做转型动作  
```cpp
const_cast<T>(expression);// 将常量性转除
dynamic_cast<T>(expression);// 父类转换为子类
reinterpret_cast<T>(expression);// 指针转换
static_cast<T>(expression);// 将non-const转换为const，将void*转为type指针等
```
+ 避免返回handles指向对象内部成分  
+ 为"异常安全"而努力是值得的  
+ 循环和递归函数不应该声明为inline，virtual函数不应该被声明为inline，通过函数指针调用的内联函数不会内联成功，构造函数和析构函数不应该声明为inline  
+ 将文件间的编译依赖关系降至最低，通过handle类和抽象类定义来实现，handle类，将多个成员变量对象，由一个指针包装，并提前声明包装的成员对象类，这样就可以不用在.h文件中加头文件了；抽象类，则由纯虚函数实现  
+ 明确public继承创造出的是is a的关系  
+ 避免遮掩继承而来的名称，父类某个函数有重载函数，子类重写了这个函数，子类对象调用不了父类的重载函数，可以在子类使用 **`using Base::func;`** 解决该问题  
+ 区分接口继承和实现继承  
    + 纯虚函数只具体指定接口继承（纯虚函数也可以对其进行实现，但是要求子类必须重写该函数）  
    + 虚函数具体指定接口继承及缺省实现继承  
    + 普通成员函数具体指定接口继承以及强制性实现继承（普通函数，所有子类都不应该去重写）  
+ 考虑virtual函数以外的其他选择  
+ 绝不重新定义继承而来的non-virtual函数  
+ 绝不重新定义继承而来的函数默认参数值（因为函数可能是虚函数）  
+ 类shape中定义枚举值 **`enum color{Red, Green, Blue};`** 可以通过域的方式访问 **`shape::Red`**，枚举相当于多个typedef  
+ has a表示有一个对象，而is a表示的是一个  
+ 谨慎使用private继承，当子类需要访问基类的保护成员或者需要重新定义继承而来的virtual函数时，可以private继承，类似于has a  
+ 谨慎使用多重继承，多重继承的基类最好不要带成员变量  
+ 了解隐式接口和编译器多态，对template参数而言，接口是隐式的  
+ 了解typename的双重意义  
    + 声明template参数时，前缀关键字class和typename可互换  
    + 使用typename标识嵌套从属类型名称，在继承基类时以及构造函数成员初始化时可以不用加typename  
+ 子类继承了模板基类，要使用模板基类里的函数，需要使用this->调用，或者用 **`using base<T>::func;`** 进行声明再调用  
+ 将与参数无关的代码抽离templates，因非类型模板参数而造成的代码膨胀，往往可以通过函数参数或成员变量替换template参数  
+ 模板类中成员函数返回模板类时，不用声明成 **`Test<T> func()`**，声明成 **`Test func()`** 即可（只针对成员函数的定义或实现直接写在类里面）；但如果成员函数也是模板函数，则需加上 **`template <U> Test<U> func();`**  
+ STL共有5种迭代器，input迭代器、output迭代器、前向迭代器、双向迭代器、随机访问迭代器  
+ new的形式必须和delete的形式配对，new异常时，系统才能自动去调用对应的delete  