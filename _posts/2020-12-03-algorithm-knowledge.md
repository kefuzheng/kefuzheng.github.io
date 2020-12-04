---
title: 算法基础知识
key: 2020-12-03
tags: algorithm
---

### 1. 计算机底层存储数据的基本原理
内存条是一个非常精密的部件，包含了上亿个电子元器件，它们很小，达到了纳米级别。这些元器件，实际上就是电路；电路的电压会变化，要么是 0V，要么是 5V，只有这两种电压。5V 是通电，用1来表示，0V 是断电，用0来表示。所以，**一个元器件有2种状态，0 或者 1**。我们通过电路来控制这些元器件的通断电，会得到很多0、1的组合。例如，8个元器件有 2^8=256 种不同的组合，16个元器件有 2^16=65536 种不同的组合。虽然一个元器件只能表示2个数值，但是多个结合起来就可以表示很多数值了。   
一般情况下我们不一个一个的使用元器件，而是**将8个元器件看做一个单位**，即使表示很小的数，例如 1，也需要8个，也就是 00000001。**1个元器件称为1比特（Bit）或1位，8个元器件称为1字节（Byte）**。
- 1Byte = 8 Bit
- 1KB = 1024Byte = 2^10Byte
- 1MB = 1024KB = 2^20Byte
- 1GB = 1024MB = 2^30Byte
- 1TB = 1024GB = 2^40Byte
- 1PB = 1024TB = 2^50Byte
- 1EB = 1024PB = 2^60Byte

### 2. 算法的复杂度
算法复杂度分为时间复杂度和空间复杂度。其作用： 时间复杂度是指执行算法所需要的计算工作量；而空间复杂度是指执行这个算法所需要的内存空间
##### 1. 时间复杂度
```java
int aFunc(int n) {
    for(int i = 0; i<n; i++) {         // 需要执行 (n + 1) 次
        printf("Hello, World!\n");      // 需要执行 n 次
    }
    return 0;       // 需要执行 1 次
}
```
这个方法需要 f(n) = (n + 1 + n + 1) = 2n + 2 次运算。    
算法的时间复杂度，用来度量算法的运行时间，记作: T(n) = O(f(n))。它表示随着 输入大小n 的增大，算法执行需要的时间的增长速度可以用 f(n) 来描述。   
如果一个算法的执行次数是 T(n)，那么只保留最高次项，同时忽略最高项的系数后得到函数 f(n)，此时算法的时间复杂度就是 O(f(n))。为了方便描述，下文称此为 大O推导法。   
O(1)常数阶 < O(logn)对数阶 < O(n)线性阶 < O(n^2)平方阶 < O(n^3)(立方阶) < O(2^n) (指数阶)   
时间复杂度分析的基本策略是：从内向外分析，从最深层开始分析。如果遇到函数调用，要深入函数进行分析。
1. 多个循环的时间复杂度：   
```java
for(int i = 0 ; i < n ; i++)           // n+1
{ 
    for(int j = 0 ; j < m ; j++)       // n*(m+1)
    {
        num++;                         // n*m
    }
}
```
f(n) = (n+1)+n*(m+1)+n*m = 2*n*m+2*n+1   
当 n、m 都无限大时，我们完全就可以认为 n==m。在此基础上，2*n*m+2*n+1 又可以简化为 2*n^2+2*n+1，这就是此段程序在最坏情况下的运行时间，也就是此段程序的频度。再以无限大的思想来简化 2*n2+2*n+1。当 n 无限大的：首先，常数 1 是可以忽略不计的；其次，对于指数级的 2*n^2 来说，是否在其基础上加 2*n，并无关紧要；甚至于，对于是否给 n^2 乘 2，也可以忽略。因此，最终频度 2*n^2+2*n+1 可以简化为 n^2 。   
事实上，对于一个算法（或者一段程序）来说，其最简频度往往就是最深层次的循环结构中某一条语句的执行次数   
2. 对于顺序执行的语句或者算法，总的时间复杂度等于其中最大的时间复杂度。
```java
void aFunc(int n) {
    // 第一部分时间复杂度为 O(n^2)
    for(int i = 0; i < n; i++) {
        for(int j = 0; j < n; j++) {
            printf("Hello, World!\n");
        }
    }
    // 第二部分时间复杂度为 O(n)
    for(int j = 0; j < n; j++) {
        printf("Hello, World!\n");
    }
}
```
此时时间复杂度为 max(O(n^2), O(n))，即 O(n^2)。
3. 对于条件判断语句，总的时间复杂度等于其中**时间复杂度最大的路径**的时间复杂度。
```java
void aFunc(int n) {
    if (n >= 0) {
        // 第一条路径时间复杂度为 O(n^2)
        for(int i = 0; i < n; i++) {
            for(int j = 0; j < n; j++) {
                printf("输入数据大于等于零\n");
            }
        }
    } else {
        // 第二条路径时间复杂度为 O(n)
        for(int j = 0; j < n; j++) {
            printf("输入数据小于零\n");
        }
    }
}
```
此时时间复杂度为 max(O(n^2), O(n))，即 O(n^2)。
4. 基础题
```java
void aFunc(int n) {
    for (int i = 0; i < n; i++) {
        for (int j = i; j < n; j++) {
            printf("Hello World\n");
        }
    }
}
```
当 i = 0 时，内循环执行 n 次运算，当 i = 1 时，内循环执行 n - 1 次运算……当 i = n - 1 时，内循环执行 1 次运算。所以，执行次数 T(n) = n + (n - 1) + (n - 2)……+ 1 = n(n + 1) / 2 = n^2 / 2 + n / 2。   
根据上文说的 大O推导法 可以知道，此时时间复杂度为 O(n^2)。
5. 进阶题
```java
void aFunc(int n) {
    for (int i = 2; i < n; i++) {
        i *= 2;
        printf("%i\n", i);
    }
}
```
假设循环次数为 t，则循环条件满足 2^t < n。  
可以得出，执行次数t = log(2)(n)，即 T(n) = log(2)(n)，可见时间复杂度为 O(log(2)(n))，即 O(log n)。
6. 二分法的时间复杂度和空间复杂度
```c++
// 非递归
template<typename T>
T* BinarySearch(T* array,int number,const T& data)
{
   assert(number>=0);
   int left = 0;
   int right = number-1;
   while (right >= left)
   {
      int mid = (left&right) + ((left^right)>>1);
      if (array[mid] > data)
      {
            right = mid - 1;
      }
      else if (array[mid] < data)
      {
            left = mid + 1;
      }
      else
      {
            return (array + mid);
      }
   }
   return NULL;
}

// 递归
template<typename T>
T* BinarySearch(T* left,T* right,const T& data)
{
   assert(left);
   assert(right);
   if (right >=left)
   {
      T* mid =left+(right-left)/2;
      if (*mid == data)
         return mid;
      else
         return *mid > data ? BinarySearch(left, mid - 1, data) : BinarySearch(mid + 1, right, data);
   }
   else
   {
      return NULL;
   }
}
```
在长度为n的表中查找，第一次循环，n在n/2的范围查找；第二次，在n/2/2的范围内查找；第x次循环，在n/(2^x)的范围内查找。假设最坏的情况下，循环x次之后找到，则：n/(2^x)=1， x=log2n，循环的次数基本是log2N，所以时间复杂度是O(log2N)   
7. 斐波那契数列
```java
long aFunc(int n) {
    if (n <= 1) {
        return 1;
    } else {
        return aFunc(n - 1) + aFunc(n - 2);
    }
}
```
显然运行次数，T(0) = T(1) = 1，同时 T(n) = T(n - 1) + T(n - 2) + 1，这里的 1 是其中的加法算一次执行。显然 T(n) = T(n - 1) + T(n - 2) 是一个斐波那契数列，通过归纳证明法可以证明，当 n >= 1 时 T(n) < (5/3)^n，同时当 n > 4 时 T(n) >= (3/2)^n。所以该方法的时间复杂度可以表示为 O((5/3)^n)，简化后为 O(2^n) 
##### 2. 空间复杂度
空间复杂度是对一个算法在运行过程中临时占用存储空间大小的量度，所谓的临时占用存储空间指的就是代码中「辅助变量所占用的空间」，它包括为参数表中「形参变量」分配的存储空间和为在函数体中定义的「局部变量」分配的存储空间两个部分。我们用 S(n)=O(f(n))来定义，其中n为问题的规模(或大小)。通常来说，只要算法不涉及到动态分配的空间，以及递归、栈所需的空间，空间复杂度通常为0(1)。一个一维数组a[n]，空间复杂度O(n)，二维数组为O(n^2)。   
如果算法执行所需要的临时空间不随着某个变量n的大小而变化，即此算法空间复杂度为一个常量，可表示为 O(1)。如下代码中的 i、j、t 所分配的空间都不随着处理数据量变化，因此它的空间复杂度为O(1)。
```python
i = 0
j = 1
t = i + j
```
这段代码中，第一行定义了一个列表，这个列表的长度随着n的规模不同，会不一样，这里空间复杂度为O(n)。
```python
def fun(n):
    temp = []
    for i in range(n):
        temp.append(i)
```
### 3. 链表（Linked List）
**链表的定义**：链表是一种物理存储单元上非连续、非顺序的存储结构，数据元素的逻辑顺序是通过链表中的指针连接次序实现的。   
每一个链表都包含多个节点，节点又包含两个部分，一个是数据域（储存节点含有的信息），一个是引用域（储存下一个节点或者上一个节点的地址）。   
**链表的特点**：获取数据麻烦，需要遍历查找，比数组慢；方便插入、删除   
**链表的实现原理**：创建一个节点类，其中节点类包含两个部分，第一个是数据域（你到时候要往节点里面储存的信息），第二个是引用域（相当于指针，单向链表有一个指针，指向下一个节点；双向链表有两个指针，分别指向下一个和上一个节点）。创建一个链表类，其中链表类包含三个属性：头结点、尾节点和大小，方法包含添加、删除、插入等等方法。
##### 1. 单向链表（Single-Linked List）
![单链表](https://github.com/kefuzheng/kefuzheng.github.io/raw/master/assets/images/single_linked_list.PNG)
单链表是链表中结构最简单的。一个单链表的节点(Node)分为两个部分，第一个部分(data)保存或者显示关于节点的信息，另一个部分存储下一个节点的地址。最后一个节点存储地址的部分指向空值。  
单向链表只可向一个方向遍历，一般查找一个节点的时候需要从第一个节点开始每次访问下一个节点，一直访问到需要的位置。而插入一个节点，对于单向链表，我们只提供在链表头插入，只需要将当前插入的节点设置为头节点，next指向原头节点即可。删除一个节点，我们将该节点的上一个节点的next指向该节点的下一个节点。   
```java
public class SingleLinkedList {
   private Node head;//头节点

   public SingleLinkedList(){
      head = null;
   }

   //链表的每个节点类
   private class Node{
      private Object data;//每个节点的数据
      private Node next;//每个节点指向下一个节点的连接

      public Node(Object data){
         this.data = data;
      }
   }

   //在链表头添加元素
   public Object addHead(Object obj){
      Node newHead = new Node(obj);
      if(size == 0){
         head = newHead;
      }else{
         newHead.next = head;
         head = newHead;
      }
      return obj;
   }

   //在链表头删除元素
   public Object deleteHead(){
      Object obj = head.data;
      head = head.next;
      return obj;
   }

    // 链表长度
    public int length() {
        int length = 0;
        Node tmp = head;
        while (tmp != null) {
            length++;
            tmp = tmp.next;
        }
        return length;
    }

   //查找指定元素，找到了返回节点Node，找不到返回null
   public Node find(Object obj){
      Node current = head;
      int tempSize = length();
      while(tempSize > 0){
         if(obj.equals(current.data)){
               return current;
         }else{
               current = current.next;
         }
         tempSize--;
      }
      return null;
   }

   //删除指定的元素，删除成功返回true
   public boolean delete(Object value){
      if(size == 0){
         return false;
      }
      Node current = head;
      Node previous = head;
      while(current.data != value){
         if(current.next == null){
               return false;
         }else{
               previous = current;
               current = current.next;
         }
      }
      //如果删除的节点是第一个节点
      if(current == head){
         head = current.next;
      }else{//删除的节点不是第一个节点
         previous.next = current.next;
      }
      return true;
   }

   //判断链表是否为空
   public boolean isEmpty(){
      return (length() == 0);
   }

   //显示节点信息
   public void display(){
      if(length() >0){
         Node node = head;
         int tempSize = length();
         if(tempSize == 1){//当前链表只有一个节点
            System.out.println("["+node.data+"]");
            return;
         }
         while(tempSize>0){
            if(node.equals(head)){
               System.out.print("["+node.data+"->");
            }else if(node.next == null){
               System.out.print(node.data+"]");
            }else{
               System.out.print(node.data+"->");
            }
            node = node.next;
            tempSize--;
         }
         System.out.println();
      }else{//如果链表一个节点都没有，直接打印[]
         System.out.println("[]");
      }
   }


   /** 链表反转
    * 
    * @param head
    * @return
    */
   public Node ReverseIteratively(Node head) {
      Node tmpReversedHead = head;
      Node tmpNode = head;
      Node tmpPrev = null;
      while (tmpNode != null) {
         Node tmpNext = tmpNode.next;
         if (tmpNext == null) {
            tmpReversedHead = tmpNode;
         }
         tmpNode.next = tmpPrev;
         tmpPrev = tmpNode;
         tmpNode = tmpNext;
      }
      head = tmpReversedHead;
      return head;
   }


   /**
    * 查找单链表的中间节点
    * 采用快慢指针的方式查找单链表的中间节点，快指针一次走两步，慢指针一次走一步，
    * 当快指针走完时，慢指针刚好到达中间节点。
    * @param head
    * @return
    */
   public Node SearchMid(Node head) {
      Node p = head, q = head;
      while (p != null && p.next != null && p.next.next != null) {
         p = p.next.next;
         q = q.next;
      }
      System.out.println("Mid:" + q.data);
      return q;
   }

   /**
    * 查找倒数 第k个元素
    * 采用两个指针P1,P2，P1先前移K步，然后P1、P2同时移动，
    * 当p1移动到尾部时，P2所指位置的元素即倒数第k个元素 。
    * @param head
    * @param k
    * @return
    */
   public Node findElem(Node head, int k) {
      if (k < 1 || k > this.length()) {
         return null;
      }
      Node p1 = head;
      Node p2 = head;
      for (int i = 0; i < k; i++)// 前移k步
         p1 = p1.next;
      while (p1 != null) {
         p1 = p1.next;
         p2 = p2.next;
      }
      return p2;
   }

   /**
    * 排序
    * 
    * @return
    */
   public Node orderList() {
      Node nextNode = null;
      int tmp = 0;
      Node curNode = head;
      while (curNode.next != null) {
         nextNode = curNode.next;
         while (nextNode != null) {
            if (curNode.data > nextNode.data) {
               tmp = curNode.data;
                  curNode.data = nextNode.data;
                  nextNode.data = tmp;
               }
               nextNode = nextNode.next;
         }
         curNode = curNode.next;
      }
      return head;
   }

   /**
    * 删除重复节点
    */
   public void deleteDuplecate(Node head) {
      Node p = head;
      while (p != null) {
         Node q = p;
         while (q.next != null) {
               if (p.data == q.next.data) {
                  q.next = q.next.next;
               } else
                  q = q.next;
         }
         p = p.next;
      }
   }

   /**
    * 判断链表是否有环，单向链表有环时，尾节点相同
    * 
    * @param head
    * @return
    */
   public boolean IsLoop(Node head) {
      Node fast = head, slow = head;
      if (fast == null) {
         return false;
      }
      while (fast != null && fast.next != null) {
         fast = fast.next.next;
         slow = slow.next;
         if (fast == slow) {
               System.out.println("该链表有环");
               return true;
         }
      }
      return !(fast == null || fast.next == null);
   }
}
```
##### 2. 双端链表
对于单项链表，我们如果想在尾部添加一个节点，那么必须从头部一直遍历到尾部，找到尾节点，然后在尾节点后面插入一个节点。这样操作很麻烦，如果我们在设计链表的时候多个对尾节点的引用，那么会简单很多。
![双端链表](https://github.com/kefuzheng/kefuzheng.github.io/raw/master/assets/images/double_linked_list.PNG)
```java
public class DoublePointLinkedList {
   private Node head;//头节点
   private Node tail;//尾节点
   private int size;//节点的个数

   private class Node{
      private Object data;
      private Node next;

      public Node(Object data){
         this.data = data;
      }
   }

   public DoublePointLinkedList(){
      size = 0;
      head = null;
      tail = null;
   }

   //链表头新增节点
   public void addHead(Object data){
      Node node = new Node(data);
      if(size == 0){//如果链表为空，那么头节点和尾节点都是该新增节点
         head = node;
         tail = node;
         size++;
      }else{
         node.next = head;
         head = node;
         size++;
      }
   }

   //链表尾新增节点
   public void addTail(Object data){
      Node node = new Node(data);
      if(size == 0){//如果链表为空，那么头节点和尾节点都是该新增节点
         head = node;
         tail = node;
         size++;
      }else{
         tail.next = node;
         tail = node;
         size++;
      }
   }

   //删除头部节点，成功返回true，失败返回false
   public boolean deleteHead(){
      if(size == 0){//当前链表节点数为0
         return false;
      }
      if(head.next == null){//当前链表节点数为1
         head = null;
         tail = null;
      }else{
         head = head.next;
      }
      size--;
      return true;
   }
   //判断是否为空
   public boolean isEmpty(){
      return (size ==0);
   }
   //获得链表的节点个数
   public int getSize(){
      return size;
   }

   //显示节点信息
   public void display(){
      if(size >0){
         Node node = head;
         int tempSize = size;
         if(tempSize == 1){//当前链表只有一个节点
               System.out.println("["+node.data+"]");
               return;
         }
         while(tempSize>0){
            if(node.equals(head)){
               System.out.print("["+node.data+"->");
            }else if(node.next == null){
               System.out.print(node.data+"]");
            }else{
               System.out.print(node.data+"->");
            }
            node = node.next;
            tempSize--;
         }
         System.out.println();
      }else{//如果链表一个节点都没有，直接打印[]
         System.out.println("[]");
      }
   }
}
```
##### 3. 双向链表
![双向链表](https://github.com/kefuzheng/kefuzheng.github.io/raw/master/assets/images/double_linked_list.PNG)
```java
public class TwoWayLinkedList {
   private Node head;//表示链表头
   private Node tail;//表示链表尾
   private int size;//表示链表的节点个数

   private class Node{
      private Object data;
      private Node next;
      private Node prev;

      public Node(Object data){
         this.data = data;
      }
   }

   public TwoWayLinkedList(){
      size = 0;
      head = null;
      tail = null;
   }

   //在链表头增加节点
   public void addHead(Object value){
      Node newNode = new Node(value);
      if(size == 0){
         head = newNode;
         tail = newNode;
         size++;
      }else{
         head.prev = newNode;
         newNode.next = head;
         head = newNode;
         size++;
      }
   }

   //在链表尾增加节点
   public void addTail(Object value){
      Node newNode = new Node(value);
      if(size == 0){
         head = newNode;
         tail = newNode;
         size++;
      }else{
         newNode.prev = tail;
         tail.next = newNode;
         tail = newNode;
         size++;
      }
   }

   //删除链表头
   public Node deleteHead(){
      Node temp = head;
      if(size != 0){
         head = head.next;
         head.prev = null;
         size--;
      }
      return temp;
   }

   //删除链表尾
   public Node deleteTail(){
      Node temp = tail;
      if(size != 0){
         tail = tail.prev;
         tail.next = null;
         size--;
      }
      return temp;
   }

   //获得链表的节点个数
   public int getSize(){
      return size;
   }
   //判断链表是否为空
   public boolean isEmpty(){
      return (size == 0);
   }

   //显示节点信息
   public void display(){
      if(size >0){
         Node node = head;
         int tempSize = size;
         if(tempSize == 1){//当前链表只有一个节点
            System.out.println("["+node.data+"]");
            return;
         }
         while(tempSize>0){
            if(node.equals(head)){
               System.out.print("["+node.data+"->");
            }else if(node.next == null){
               System.out.print(node.data+"]");
            }else{
               System.out.print(node.data+"->");
            }
            node = node.next;
            tempSize--;
         }
         System.out.println();
      }else{//如果链表一个节点都没有，直接打印[]
         System.out.println("[]");
      }
   }
}
```

----

[整数、浮点数在计算机中的存储](https://www.cnblogs.com/mukekeheart/p/10517298.html)  
[时间复杂度&空间复杂度分析](https://blog.csdn.net/LF_2016/article/details/52453212)   
[十分钟搞定时间复杂度](https://www.jianshu.com/p/f4cca5ce055a)    
[Java数据结构和算法（七）——链表](https://www.cnblogs.com/ysocean/p/7928988.html)   
[链表的原理及java实现](https://blog.csdn.net/jianyuerensheng/article/details/51200274)   

