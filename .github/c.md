# C++學習過程

:::spoiler 目錄
[TOC]
:::

## 命令列介面

### (Macro)巨集變數

* 參數 -D [巨集名稱]
範例:
`gcc main.c -o exe -D巨集名`
可用於debug

* 參數 -c 只編譯不連結
可以將各檔案分別編譯，最後在連結一起執行
可用make進階完成。

* 參數 -g
可以使用GDB除錯

### GDB (GNU Debugger)

編譯時加上 -g
使用gdb 載入執行檔

當成是執行發生異常或是終止，作業系統會記錄當時記憶體狀態，存成一個 **(core dump)** ，可以使用GDB還原當時狀態。

### Make and Makefile

* 常見錯誤，command 需以 \<tab> 字符為開始

* Makefile 要素和結構：

變數（Variables）：
可以在 Makefile 中定義變數，用於存儲項目中的路徑、編譯器的選項、檔案名稱等。這樣可以使 Makefile 更具彈性和可讀性。

``` makefile
CC = gcc             # variables
CFLAGS = -Wall -O2   # variables
```

目標（Targets）：
目標是指定要建置的項目。每個目標都有一個相應的規則，描述了如何編譯和連結該目標。

``` makefile
# targets : Dependency
main: main.o utils.o
    $(CC) $(CFLAGS) -o main main.o utils.o

main.o: main.c
    $(CC) $(CFLAGS) -c main.c

utils.o: utils.c
    $(CC) $(CFLAGS) -c utils.c
```

在上面的示例中，main 是目標，它依賴於 main.o 和 utils.o。每個目標都有一個相應的規則，指定了如何編譯和連結目標。

依賴關係（Dependencies）：
指定了目標所依賴的檔案或其他目標。如果依賴的檔案有更新，則相應的規則將被執行。上面例中，main 目標依賴於 main.o 和 utils.o。

規則（Rules）：
規則描述了如何從依賴項目建置目標。它包含一個目標、依賴項目和相應的指令。規則由一個或多個命令組成，這些命令以 Tab 字元開頭。以下是一個規則的示例：

``` makefile
main.o: main.c
    $(CC) $(CFLAGS) -c main.c #rules
```

在這個示例中，main.o 是目標，它依賴於 main.c。規則中的命令 $CC $CFLAGS -c main.c 用於編譯 main.c。

以上是 Makefile 的基本結構和要素。你可以根據自己的項目需求添加更多的目標、規則和變數其他要素，例如：

預設目標（Default Target）：
Makefile 可以指定一個預設目標，當你執行 make 命令時，該目標將被建置。以下是一個定義預設目標的示例：

``` makefile
#all is Default Target
all: main 

main: main.o utils.o
    $(CC) $(CFLAGS) -o main main.o utils.o

main.o: main.c
    $(CC) $(CFLAGS) -c main.c

utils.o: utils.c
    $(CC) $(CFLAGS) -c utils.c
```

隱含規則（Implicit Rules）：
Make 工具提供了一些隱含的規則，用於根據檔案的擴展名自動推斷編譯和連結的命令。
例如：如果你的目標是 main.o，而你在 Makefile 中並沒有明確定義規則，Make 工具將使用預設的隱含規則進行編譯。這使得 Makefile 更加簡潔，因為你不需要為每個檔案都定義規則。

清理目標（Clean Target）：在 Makefile 中，通常會定義一個清理目標，用於刪除編譯生成的檔案。這可以是臨時檔案、目標檔案或執行檔等。以下是一個定義清理目標的示例：

``` makefile
clean:
    rm -f main *.o
```

在這個示例中，clean 是清理目標，執行 make clean 命令將刪除 main 及其相關的 .o 檔案。

這些是 Makefile 的基本要素和結構，它們可以根據項目的需求進行擴展和調整。Makefile 提供了一種靈活且強大的方法來定義編譯和建置的規則，並自動化專案的建置過程。

## Version Control Git

## Functions

### iostream

`endl`將緩衝區的資料輸出。

### 檔案流 write_file

* 標頭檔`#include<fstream>`
  * ifstream
  * ostream

類似於標準輸出輸入(建立連結)
i 讀檔案=把檔案資料寫到程式(input)
o寫檔案=把程式輸出寫到檔案中(output)

使用方法 先建一個物件

``` cpp
ifstream 物件名
物件名.函式;
```

### Vector(dynamic array)

* 結構
  ![結構](https://i.imgur.com/yB89eK5.png)
  vector類似於array，不同於array，vector可以動態的增加長度。
* 初始化

  ``` cpp
  #include <vector>
  vector <資料型態> vectorName;
  ```
  
* 特點
  * vector 的優點
    宣告時可以不用確定大小
    節省空間
    支持隨機訪問[i]
  * vector 的缺點
    進行插入刪除時效率低
    只能在末端進行 pop 和 push

* 使用常用功能
`vectorName.push_back`：把元素加到尾巴，必要時會進行記憶體配置
`vectorName.pop_back`：移除尾巴的元素
`vectorName.insert`：插入元素
`vectorName.erase`：移除某個位置元素, 也可以移除某一段範圍的元素
`vectorName.clear`：清空容器裡所有元素
`vectorName.resize()`:重新給定大小，擴充或減少

:::warning

* 三種遍歷技巧比較

  * 第一種寫法：`for (int i = 0; i < vec.size(); i++)`

    這種寫法使用了索引變量 i，從 0 開始逐一遍歷 vector 容器中的元素，
    直到 i 達到vector 的大小為止。這種寫法相對簡單，容易理解和掌握，
    但需要注意的是，當 vector 容器中的元素數量很大時，
    遍歷過程中需要不斷計算 vector 的大小，對性能有一定影響。

  * 第二種寫法：`for (vector<int>::iterator it = vec.begin(); it != vec.end(); ++it)`

    這種寫法使用了迭代器，從 vector 容器的開始位置（begin）開始遍歷每個元素，
    直到結束位置（end）為止。這種寫法相對於第一種寫法，
    不需要額外計算 vector 的大小，對性能影響較小，
    同時也可以使用迭代器提供的其他方法對元素進行操作。
    但需要注意的是，當 vector 容器內存不連續時，
    迭代器的運行速度可能會慢於索引方式。

  * 第三種寫法：`for (auto &v : vec)`

    ``` cpp
    for (auto &v : vec) //將vec位址傳入，會改vec值
    for (auto v : vec)  //將ve值傳入，不會更改vec值  
    ```

    這種寫法使用了 C++11 引入的 foreach 循環方式，
    也被稱為範圍遍歷（range-based for loop）。
    這種寫法可以自動推導 vector 元素的數據類型，並且更簡潔，可讀性更高。
    同時，這種寫法也可以避免在遍歷過程中產生迭代器失效的問題，因為它不涉及迭代器的使用。

  三種寫法的優劣取決於具體情況和需求。
  一般來說，如果要對 vector 容器內的元素進行修改操作，
  應使用第二種寫法；如果需要較簡潔的代碼和更好的可讀性，應使用第三種寫法；如果只是簡單遍歷 vector 容器，應使用第一種寫法。
  
> [name=ChatGPT]
:::
我沒有看很懂
:::warning

* `first` 和 `begin`、`back` 和 `end` 是 C++ 中常用的 STL 容器中的一些成員函數或迭代器，它們在使用上有以下幾點區別：

  * `first` 和 `begin`
    `first` 和 `begin` 都是指向 STL 容器中第一個元素的迭代器，但是它們的用法不同：

    `first` 通常用於表示 STL 的 pair 類型中的第一個元素，它是 pair 的一個公開成員變量，可以直接使用。
    例如，對於一個 pair 對象 p，我們可以使用 `p.first` 獲取其第一個元素。

    `begin` 則是 STL 容器中的一個成員函數，它返回指向容器中第一個元素的迭代器。
    例如，對於一個 vector 容器 v，我們可以使用 `v.begin()` 獲取其指向第一個元素的迭代器。
  * `back` 和 `end`
    `back` 和 `end` 都是指向 STL 容器中最後一個元素的迭代器，但是它們的用法不同：

    `back` 是 STL 容器的一個成員函數，它返回最後一個元素的引用。
    例如，對於一個`vector` 容器 v，我們可以使用 `v.back()` 獲取其最後一個元素的值。
    `end` 則是指向 STL 容器中結束位置（即最後一個元素的下一個位置）的迭代器。
    例如，對於一個 vector 容器 v，我們可以使用 `v.end()` 獲取其指向結束位置的迭代器。

* 總體來說， `first`  和  `back`  主要用於  `pair` 和容器的讀取操作，而  `begin`  和  `end`  則主要用於迭代容器中的元素。

> [name=ChatGPT]
:::

![:初始化](https://i.imgur.com/uuAUdwa.png)

什麼是inline function
看PPT比較差異優劣

### `pair`

由兩個型別組成的容器

```cpp
#include <utility>
pair <type1,type2> pairName;
pairName.first; //回傳type1值
pairNamr.second;
```

### map

* `map`是一個一對一(one-to-one)的容器，其中
  * `map.first`是key，key是唯一值，一個key對應到一個元素。
  * `map.second`是key對應到元素。
  * `map`會自動將資料由小到大排列，
* (延伸)`map`內部資料結構是red-black tree
:::info

* 元素存取
  `operator[]`：存取指定的[i]元素的資料

* 迭代器
  `begin()`：回傳指向map頭部元素的迭代器
  `end()`：回傳指向map末尾的迭代器

  若要由大到小使用，可以使用反向迭代器。
  `rbegin()`：回傳一個指向map尾部的反向迭代器
  `rend()`：回傳一個指向map頭部的反向迭代器

* 容量
  `empty()`：檢查容器是否為空，空則回傳true
  `size()`：回傳元素數量
  `max_size()`：回傳可以容納的最大元素個數

* 修改器
  `clear()`：刪除所有元素
  `insert()`：插入元素
  `erase()`：刪除一個元素
  `swap()`：交換兩個map

* 查找
  `count()`：回傳指定元素出現的次數
  `find()`：查找一個元素
:::
:::warning

* 初始化
  初始化也有不同的方式
  * 直接宣告
    `map<keyType, valueType> mapName;`
  * 使用pointer的方式宣告
  
    ```cpp
    typedef map<keyType, valueType> TEST_MAP;
    TEST_MAP* mapTest = new TEST_MAP();
    //TODO ....
    delete mapTest;
    ```

  * 直接宣告可以在宣告的時候一併將資料定義進去

* 插入資料
  出入資料有兩種方式
  * insert

    ```cpp
    mapName.insert(pair<keyType,valueType>(key,value));
    ```

  * array

    ```cpp
    mapName[key] = value;
    ```

  * 彼此的差異是什麼
    使用`insert`如果key值已經存在`map`中，將會失效。
    如果使用`[]`的方式的話，將會覆蓋原本資料。

* 如何遍歷`map`?
  我們可以使用`for`來遍歷`map`
  
* 讀取資料：
  讀取資料時，我們不知道`map`中是否有東西，貿然讀取是一個危險的行為，會發生不可知的錯誤。
  所以讀取資料要使用`find`
  
* 清除資料
  `erase`有三種overload的寫法
  
  ```cpp
  //迭代器刪除
  iter = mapStudent.find("r123");
  mapStudent.erase(iter);

  //用關鍵字刪除
  int n = mapStudent.erase("r123");//如果刪除了會返回1，否則返回0

  //用迭代器範圍刪除 : 把整個map清空
  mapStudent.erase(mapStudent.begin(), mapStudent.end());
  //等同於mapStudent.clear()
  ```
  
:::
  
### unordered map

### set_intersection()

### unordered_set

### partial_sort()

### Basic Operator Overloading

::: info

* Binary / Unary operators
* Member Functions
:::

#### Binary Operator

* To work with **our type** operation
  寫一個新的二元運算子來執行符合我們需求的運算，
  (e.g. +, -, = , %, ==, /, \*)皆可以overloading
  有 **一個** 輸入參數[(member function)](#member-function)以及 **兩個** 輸入參數的方法。
  
  我們將Operator Overloading看做成function的呼叫，
  當我們呼叫這個"function"時，會有我們自訂的type回傳。
  所以我們會先完成一個`type`傳入我們需要使用到的參數，在內完成新`type`定義；
  當然也會有非自訂的type回傳，是需求完成operation overloafing type。
  
* 我們先看兩個輸入參數的方式，後面會提到一個輸入參數
  ![兩個輸入參數](https://hackmd.io/_uploads/B1U9cpVDn.png)

)
  這裡略過定義`class Money`
  這段程式碼想要完成有不同單位的金額相加
  
* anonymous object (匿名物件)
    也稱作臨時物件（Temporary Object）或無名物件（Unnamed Object）
    在表達式中做某些操作被使用，在後續不會被使用，使用完後即被銷毀
    `retuen Money(finalDollars, finalCents);`
    `Money(finalDollars, finalCents);`即為anonymous object。

* const and non-const

  ```cpp
  // const
  const Money operator +(const Money& amount1, const Money& amount2);
  // non-const
  Money operator +(const Money& amount1, const Money& amount2);
  ```
  
  當我們定義時使用的是non-const
  在我們調用時，(m1, m2 are Money objects)
  
  ```cpp
  (m1 + m2).output();   // 合法，沒有問題     因為output沒有修改Money物件狀態
  (m1 + m2).input();    // 合法，但是有問題   因為input修改了Money物件狀態
  ```
  
  如果使用non-const的方式，compiler會將其視為合法，但執行的時候會發生問題，
  **為了避免這種狀況，我們使用`const`**，如果嘗試對其進行修改，compiler會報錯。
  
  > C++ 提供了使用者較為寬鬆的寫法，編寫時需要注意更多事情，這是C++常讓人詬病的地方
  > [name=教授]
  
#### Unary Operator

* 一元運算子，所以輸入的參數也只有一個
* "-" operator is overloaded twice!
  * For two operands/arguments (binary)
  * For one operand/argument (unary)
  * Definitions must exist for both

#### member function
  
在進入member operator之前，我們先了解member function

* Member Function
  下例中，展示member function的結構
  
  ```cpp
  class MyClass {
  public:
    // 成員函數的聲明
    returnType functionName(parameterList...) {
    // 函數體
    }
    .
    .
    .
  };
  ```

  成員函數當然不只一個

* 現在來看到一個輸入參數 Member Function
  ::: warning
  Notice only ONE argument
  只有一個輸入參數
  :::

  先前的Operator都是兩個參數，可以看做
  
  ```cpp
  Vector v1(2, 3);
  Vector v2(4, 5);
  Vector sum = v1 + v2;  // 等同於 operator+(v1, v2)
  ```

  現在只有一個輸入參數，會使用到類似member function的方式
  
  * 首先

    ```cpp
    class Vector {
    private:
        int x, y;

    public:
        Vector(int x, int y) : x(x), y(y) {}

        // 重載加法運算符
        Vector operator+(const Vector& other) const {
        int newX = x + other.x;
        int newY = y + other.y;
        return Vector(newX, newY);
        }
    };
    ```

  * 接下來使用時
  
    ```cpp
    Vector v1(2, 3);
    Vector v2(4, 5);
    Vector sum = v1 + v2;  // 等同於 v1.operator+(v2)
    ```

> 良好的編寫程式規則，當你不需要更改其值，就使用`const`，
> 在閱讀code的時候也可以讓別人知道，你是不希望別人更改的
> [name=教授]

:::success

* Object-Oriented-Programming，維持物件導向(OOP)的精神
  * Principles suggest member operators
  * Many agree, to maintain "spirit" of OOP
* Member operators more efficient，為了效率
  * No need to call accessor & mutator functions
* At least one significant disadvantage，仍然有缺點
  * (Later in chapter…)
:::

:::info
短路求值（Short-Circuit Evaluation）是一種在布爾運算中的求值策略。當使用邏輯運算符`&&`（邏輯與）和`||`（邏輯或）時，短路求值可以提高運算效率並避免不必要的計算。

短路求值的原則如下：

1.對於`&&`運算符：

如果第一個運算數（左邊）為 false，則整個表達式結果一定為 false，後面的運算數（右邊）不會被評估。
只有當第一個運算數為 true 時，才會評估並計算後面的運算數。

2.對於`||`運算符：

如果第一個運算數為 true，則整個表達式結果一定為 true，後面的運算數不會被評估。
只有當第一個運算數為 false 時，才會評估並計算後面的運算數。

Complete Evaluation 相對於 Short-Circuit Evaluation
:::

* Generally should not overload these operators

### Friends and Automatic Type Conversion

:::info

* Friend functions, friend classes
* Constructors for automatic type conversion
:::

* 更有效率
* Has direct access to `private` members
  Just as member functions do
  But not a member function
* Friend function

  ```cpp
  class MyClass {
  private:
      int privateData;

  public:
      MyClass(int data) : privateData(data) {}

      // 在前面加上friend，讓friendFunction可被訪問
      friend void friendFunction(const MyClass& obj);
  };

  // 實作friendFunction
  void friendFunction(const MyClass& obj) {
      // 可以訪問 MyClass 的私有成員
      std::cout << "Private data: " << obj.privateData << std::endl;
  }

  int main() {
      MyClass obj(42);
      friendFunction(obj); // 呼叫friend function
      return 0;
  }
  ```

* Friend class

  ```cpp
  class FriendClass {
  public:
      void friendClassFunction(const MyClass& obj);
  };

  class MyClass {
  private:
      nt privateData;

  public:
      MyClass(int data) : privateData(data) {}

      // FriendClass is friend of MyClass.
      friend class FriendClass;
  };

  void FriendClass::friendClassFunction(const MyClass& obj) {
      // 可以訪問 MyClass 的私有成員
      std::cout << "Private data: " << obj.privateData << std::endl;
  }

  int main() {
      MyClass obj(42);
      FriendClass fc;
      fc.friendClassFunction(obj); // 呼叫friend class的函數
      return 0;
  }
  ```

:::success

* Friends not pure? ( friends 違反OOP精神，破壞封裝性)
  * "Spirit" of OOP dictates all operators and functions be member functions
  * Many believe friends violate basic OOP principles
* Advantageous?
  * For operators: very!
  * Allows automatic type conversion
  * Still encapsulates: friend is in class definition
  * Improves efficiency
:::

### References and More Overloading

:::info

* << and >>
* Operators: = , [], ++, --
:::

:::warning

* Call-by-reference
  * 將原始數據傳進去做更改
* Call-by-value
  * 將原始數據**複製**進去，不會對原始數據更改
:::

* Overloading >> and <<
  * Enables:
    `cout << myObject;`
    `cin >> myObject;`
  * 取代:
   `myObject.output(); …`

  > `cout`是`ostream`的一個對象
  
  其實就是對`<iostream>`新增一些功能，寫一個自己的`cout`。

### String

:::info

* An Array Type for Strings
  * C-Strings
  * Standard Class string
* Character Manipulation Tools
  * Character I/O
  * get, put member functions
  * putback, peek, ignore
* Standard Class string
  * String processing
:::

#### C-String

* c-string is array
* 句尾'/0'
  Called "null character"

:::warning

* Only difference from standard array:
  * Must contain null character
:::

* Must use library function for **assignment**:
  `char aString[10];`
  * `strcpy(aString, "Hello");`
    * Sets value of aString equal to "Hello"
  * `aString = "Hello"; // ILLEGAL!`

* Also cannot use operator ==
  * Must use library function again:
  
  ```cpp
  if (strcmp(aString, anotherString))
  cout << "Strings NOT same.";
  else
  cout << "Strings are same.";
  ```

* `strlen()`
  Not including null

* `strcat()`
  字串的串接，注意空間的使用。
  
* output
  `cout << news << " Wow.\n";`
  – Where news is a c-string variable
  * Possible because `<<` operator is overloaded for c-strings!

* input 沒有像是output一樣簡單
  * Whitespace is "delimiter"
    * Tab, space, line breaks are "skipped"
    * Input reading "stops" at delimiter
  * Use `getline()`, a predefined member function
    將會以 line breaks 為分割

* More Member Functions
  * `putback()`: 讀取一個`char`放入下一次輸入流
    * `cin.putback(lastChar);`
  * `peek()`: Returns next char, but leaves it there
    * `peekChar = cin.peek();`
  * `ignore()`
    * `cin.ignore(1000, "\n");`
      Skips at most 1000 characters until "\n"

#### Standard Class string / String class

* Can assign, compare, add:

  ```cpp
  string s1, s2, s3;
  s3 = s1 + s2; //Concatenation
  s3 = "Hello Mom!" //Assignment
  ```

##### `find_first_not_of(str)` 返回找到的第一個符合的位置

`str.find_first_not_of();`

##### `substr()` 子字串

``` cpp
str.substr(原字串第一位置); //提取子字串指定第一位置到最後位置
str.substr(原字串第一位置,最後位置); //提取子字串指定第一位置到指定最後位置
```

##### String Stream(sstream) 字串流

* istringstream
* ostringstream
* stringstream

配合getline可以分個字串

``` cpp
#include <sstream>
istringstream issName(stringName);
string buffer;
while(getline(iss, buffer, '/')) //未設定預設為' '
{
    cout<<buffer<<endl;
}
```

##### Other

* `stof`, `stod`, `stoi`, `stol` to convert a string to a
  `float`, `double`, `int`, or `long`
* `to_string`

### Pointers

:::info

* Pointer variables
* Memory management
:::
:::danger
* 指標是變量內存變數的位址
:::

* Pointers are "typed"
  * Can store pointer in variable
  * Not int, double, etc.
    Instead: **A POINTER to int, double, etc.!**
* new
  `p1 = new int;` (new)給定p1一個新的`int`的空間
  :::info
  • Creates new dynamic variable
  • Returns pointer to the new variable
  * If type is class type:
    – Constructor is called for new object
    – Can invoke different constructor with
    initializer arguments:
  
    ```cpp
    MyClass *mcPtr;
    mcPtr = new MyClass(32.0, 17);
    ```

  * Can still initialize non-class types:

    ```cpp
    int *n;
    n = new int(17); //Initializes *n to 17
    ```

  :::
* `new`有可能失敗
  * Heap
    Also called "freestore

  Future "new" operations will **fail if freestoreis "full"**
* nullptr
  * NULL is actually the number 0 and can lead to ambiguity

    ```cpp
    void func(int *p);
    void func(int i);
    ```

  * 在C++ 11 使用nullptr，處理了NULL不知道是0還是null pointer
* delete
  * 歸還記憶體
  * 確保刪除後指向NUll
* Array pointer is **constant** pointer

### Dynamic Arrays

使用new
:::info

* Creating and using
* Pointer arithmetic
:::

### Classes, Pointers, Dynamic

:::info

* The this pointer
* Destructors, copy constructors
:::

### Inheritance(或許需要確定正確性)

protected
子帶可以直接使用但不能更改其值
privated

## 待編輯

[](https://hackmd.io/@ndhu-programming-2021/BkZukG4jK#tags-%E5%A4%A7%E4%B8%80%E7%A8%8B%E8%A8%AD-%E4%B8%8B-%E6%9D%B1%E8%8F%AF%E5%A4%A7%E5%AD%B8-%E6%9D%B1%E8%8F%AF%E5%A4%A7%E5%AD%B8%E8%B3%87%E7%AE%A1%E7%B3%BB-%E5%9F%BA%E6%9C%AC%E7%A8%8B%E5%BC%8F%E6%A6%82%E5%BF%B5-%E8%B3%87%E7%AE%A1%E7%B6%93%E9%A9%97%E5%88%86%E4%BA%AB)

一個可以被操作的實體稱為Object，Class是Object的樣子
what is printcheck();
what is defult
