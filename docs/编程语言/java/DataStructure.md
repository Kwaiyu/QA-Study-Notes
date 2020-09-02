## 数据结构

### 概念

数据结构表示数据在计算机中的存储和组织方式，主要描述数据元素之间的位置关系等。选择适当的数据结构可以提高计算机程序的运行效率（时间复杂度）和存储效率（空间复杂度）。

1. 逻辑结构-抽象层： 主要描述的是数据元素之间的逻辑关系
   
   - 集合结构（集）：所有元素都属于一个总体，除了同属于一个集合外没有其他关系，集合结构不强调元素之间的任何关联性。
   - 线性结构（表）：数据元素之间具有一对一的前后关系。结构中必须存在唯一的首元素和唯一的尾元素。
   - 树形结构（树）：数据元素之间一对多的关系。
   - 网状结构（图）：图状结构或网状结构，结构中的数据元素之间存在多对多的关系。
2. 物理结构-存储层：主要描述的是元素之间的位置关系
   
   - 顺序结构：顺序结构就是使用一组联系的存储单元依次存储逻辑上相邻的各个元素
     - 优点：只需要申请存放数据本身的内存空间即可，支持下标访问，也可以实现随机访问。
     - 缺点：必须静态分配连续空间，内存空间的利用率比较低。插入或删除可能需要移动大量元素，效率比较低。
   - 链式结构：链式存储结构不使用连续的存储空间存放结构元素，而是为每个元素构造一个节点。节点除了存放数据本身以外，还需要存放指向下一个节点的指针。
     - 优点：不采用连续的存储空间导致内存空间利用率比较高，克服顺序存储结构中预知元素个数的缺点 插入或删除元素时，不需要移动大量的元素。
     - 缺点：需要额外的空间来表达数据之间的逻辑关系，不支持下标和随机访问。
   - 索引结构：除建立存储节点信息外，还建立附加的索引表来标节点的地址。索引表由若干索引项组成。
     - 优点：是用节点的索引号来确定节点存储地址，检索速度快。
     - 缺点：增加了附加的索引表，会占用较多的存储空间。
   - 散列结构：由节点的关键码值决定节点的存储地址。散列技术除了可用于查找外，还可用于存储。
     - 优点：散列是数组存储方式的一种发展，采用存储数组中内容的部分元素作为映射函数的输入，映射函数的输出就是存储数据的位置，相比数组，散列的数据访问速度要高于数组。
     - 缺点：不支持排序，一般用线性表存储更多的空间，并且记录关键字不能重复。
3. 运算结构-实现层：主要描述的是如何实现数据结构
   
   - 分配资源，建立结构，释放资源
   - 插入和删除
   - 获取和遍历
   - 修改和排序
   
   每种逻辑结构采用何种物理结构来实现并没有具体的规定。当一个结构在逻辑结构中只有一种定义，而在物理结构中却有两种选择，那么这个结构就属于逻辑结构。

#### 数据结构比较

#### 数据结构的选择

#### O符号

O在算法当中表述的是时间复杂度，它在分析算法复杂性的方面非常有用。常见的有：

1. `O(1)`：最低的复杂度，无论数据量大小，耗时都不变，都可以在一次计算后获得。哈希算法就是典型的O(1)
2. `O(n)`：线性，n表示数据的量，当量增大，耗时也增大，常见有遍历算法
3. `O(n²)`：平方，表示耗时是n的平方倍，当看到循环嵌循环的时候，基本上这个算法就是平方级的，如：冒泡排序等
4. `O(log n)`：对数，通常ax=n,那么数x叫做以a为底n的对数,也就是x=logan，这里是a通常是2，如：数量增大8倍，耗时只增加了3倍，二分查找就是对数级的算法，每次剔除一半
5. `O(n log n)`：线性对数，就是n乘以log n,按照上面说的数据增大8倍，耗时就是8*3=24倍，归并排序就是线性对数级的算法

### Array

在Java中，数组是用来存放同一种数据类型的集合。

```java
//只声明了类型和长度
数据类型 [] 数组名称 = new 数据类型[数组长度];
//声明了类型，初始化赋值，大小由元素个数决定
数据类型 [] 数组名称 = {数组元素1，数组元素2，......}
```

大小固定，不能动态扩展（初始化给大了浪费，给小了不够用），插入快，删除和查找慢。

#### 示例：

```java
package com.lsaiah.datatructure;

public class Array {
    private int[] intArray;
    private int elems;
    private int length;
    public Array(int max) {
        length = max;
        intArray = new int[max];
        elems = 0;
    }
    /**
     * 添加
     *
     * @param value
     */
    public void add(int value) {
        if (elems == length) {
            System.out.println("error");
            return;
        }
        intArray[elems] = value;
        elems++;
    }
    /**
     * 查找
     * @param searchKey
     * @return
     */
    public int find(int searchKey){
        int i;
        for(i = 0; i < elems; i++){
            if(intArray[i] == searchKey)
                break;
        }
        if(i == elems){
            return -1;
        }
        return i;
    }
    /**
     * 删除
     * @param value
     * @return
     */
    public boolean delete(int value){
        int i = find(value);
        if (i == -1){
            return false;
        }
        for (int j = i; j < elems; j++) {
            //后面的数据往前移
            intArray[j] = intArray[j+1];
        }
        elems--;
        return true;
    }
    /**
     * 更新
     * @param oldValue
     * @param newValue
     * @return
     */
    public boolean update(int oldValue, int newValue){
        int i = find(oldValue);
        if (i == -1){
            return false;
        }
        intArray[i] = newValue;
        return true;
    }
    /**
     * 显示所有
     */
    public void display(){
        for (int i = 0; i < elems; i++){
            System.out.println(intArray[i] + " ");
        }
        System.out.println("\n");
    }
    /**
     * 冒泡排序
     * 每趟冒出一个最大数/最小数
     * 每次运行数量：总数量-运行的趟数（已冒出）
     */
    public void bubbleSort(){
        for (int i = 0; i < elems-1; i++){
            System.out.println("第" + (i+1) + "趟：");
            for (int j = 0; j < elems-i-1; j++) {
                if (intArray[j] > intArray[j+1]){
                    int temp = intArray[j];
                    intArray[j] = intArray[j+1];
                    intArray[j+1] = temp;
                }
                display();
            }
        }
    }
    /**
     * 选择排序
     * 每趟选择一个最大数/最小数
     * 每次运行数量：总数量-运行的趟数（已选出）
     */
    public void selectSort(){
        for (int i = 0; i < elems-1; i++){
            int min = i;
            for (int j = i+1; j < elems; j++) {
                if (intArray[j] < intArray[min]){
                    min = j;
                }
            }
            if (i != min){
                int temp = intArray[i];
                intArray[i] = intArray[min];
                intArray[min] = temp;
            }
            display();
        }
    }
    /**
     * 插入排序
     * 每趟选择一个待插入的数
     * 每次运行把待插入的数放在比它大/小后面
     */
    public void insertSort(){
        int j;
        for (int i = 0; i < elems; i++) {
            int temp = intArray[i];
            j = i;
            while (j > 0 && temp < intArray[j-1]){
                intArray[j] = intArray[j-1];
                j--;
            }
            intArray[j] = temp;
        }
        display();
    }

    public static void main(String[] args) {
        Array array = new Array(10);
        array.add(6);
        array.add(3);
        array.add(8);
        array.add(2);
        array.add(11);
        array.add(5);
        array.add(7);
        array.add(4);
        array.add(9);
        array.add(10);
//        array.bubbleSort();
//        array.selectSort();
        array.insertSort();
        array.display();
//        System.out.println(array.find(4));
//        System.out.println(array.delete(1));
//        array.display();
//        System.out.println(array.update(2,6));
//        array.display();
    }
}
```

### Stack

- 栈又称为堆栈或堆叠，栈作为一种数据结构，它按照先进后出的原则存储数据，先进入的数据被压入栈底，最后的数据在栈顶。
- Java中Stack是Vector的一个子类，只定义了默认构造函数，用来创建一个空栈。
- 栈是元素的集合，其包含了两个基本操作：push操作可用于将元素压入栈，pop操作可以将栈底元素移除。
- 遵循后进先出原则。
- 时间复杂度：索引O(n)，搜索O(n)，插入O(1)，移除O(1)

#### 示例：

```java
package com.lsaiah.datatructure;
import java.util.Arrays;

public class Stack {
    /**通常可以利用栈实现字符串逆序，还可以利用栈判断分隔符是否匹配，如<a[b{c}]>，可以正进反出，栈为空则成功。
    * 基于数组实现的顺序栈，连续存储的线性实现，需要初始化容量
    * 固定数据类型
    * private int[] array;
    * 动态数据类型
    */
    private Object[] objArray;
    private int maxSize;
    private int top;
    public Stack() {
    }
    public Stack(int maxSize) {
        if(maxSize > 0){
            objArray = new Object[maxSize];
            this.maxSize = maxSize;
            top = -1;
        }else{
            throw new RuntimeException("初始化大小错误：maxSize=" + maxSize);
        }
    }
    /**
     * 入栈
     * @param obj
     */
    public void objPush(Object obj){
        //扩容
        grow();
        //++在前表示先运算再赋值，优先级高，在后表示先赋值再运算，优先级低
        objArray[++top] = obj;
    }
    /**
     * 出栈
     * @return
     */
    public Object objPop(){
        Object obj = peekTop();
        //声明原顶栈可以回收空间(GC)
        objArray[top--] = null;
        return obj;
    }
    /**
     * 查看栈顶
     * @return
     */
    public Object peekTop(){
        if(top != -1){
            return objArray[top];
        }else{
            throw new RuntimeException("stack is null");
        }
    }
    /**
     * 动态扩容
     */
    public void grow(){
        // << 左移运算符，1表示乘以2的1次方
        if(top == maxSize-1){
            maxSize = maxSize<<1;
            objArray = Arrays.copyOf(objArray,maxSize);
        }
    }
    /**基于链式存储，不连续存储的非线性实现**/
    private class Node<Object>{
        private Object data;
        private Node next;
        public Node(Object data, Node next) {
            this.data = data;
            this.next = next;
        }
    }
    private Node nodeTop;
    private int size;
    public void nodePush(Object obj){
        //栈顶指向新元素，新元素的next指向原栈顶元素
        nodeTop = new Node(obj,nodeTop);
        size++;
    }

    public Object nodePop(){
        Node old = nodeTop;
        //声明原顶栈可以回收空间(GC)
        old.next = null;
        //栈顶指向下一个元素
        nodeTop = nodeTop.next;
        size--;
        return old.data;
    }
    @Override
    public String toString() {
        StringBuilder sb = new StringBuilder("[ ");
        for(Node<Object> node = nodeTop; node != null; node = node.next){
            sb.append(node.data.toString() + " ");
        }
        return sb.toString()+"]";
    }
    public static void main(String[] args) {
//        Stack stack = new Stack(1);
//        stack.objPush("abc");
//        stack.objPush(123);
//        stack.objPush("de");
//        stack.objPush("cd");
//        stack.objPush("er");
//        stack.objPush("hello");
//        stack.objPush(666);
//        stack.objPush(545);
//        stack.objPush("word");
//        while (stack.top != -1){
//            System.out.println(stack.objPop());
//        }
//        System.out.println(stack.peekTop());
        Stack stack = new Stack();
        stack.nodePush("111");
        stack.nodePush("222");
        stack.nodePush("aaa");
        stack.nodePush("bbb");
        System.out.println(stack);
        while (stack.size > 1)
            System.out.println(stack.nodePop());
        System.out.println(stack);
    }
}
```

### Queue

- 队列是元素的集合，包含了两个基本操作：enqueue操作可以用于将元素插入到队列中，而dequeue操作则是将元素从队列中移除。
- 遵循先入先出原则
- 时间复杂度：索引O(n)，搜索O(n)，插入O(1)，移除O(1)

#### 示例：

```java
package com.lsaiah.datatructure;
public class Queue {
    /**
     * 1.单向队列（Queue）：只能在一端插入数据，另一端删除数据。
     * 2.双向队列（Deque）：每一端都可以进行插入数据和删除数据操作。
     *  与栈不同的是，队列中的数据不总是从数组的0下标开始的
     *  选择的做法是移动队头和队尾的指针。
     *  为了避免队列不满却不能插入新的数据，我们可以让队尾指针绕回到数组开始的位置，这也称为循环队列。
     */
    // 单向循环队列，顺序存储结构实现
    private Object[] objQueue;
    //队列大小
    private int maxSize;
    //顶部
    private int top;
    //底部
    private int bottom;
    //实际元素
    private int item;
    public Queue(int size) {
        maxSize = size;
        objQueue = new Object[maxSize];
        top = 0;
        bottom = -1;
        item = 0;
    }
    /**
     * 入队
     * @param obj
     */
    public void add(Object obj){
        if(item == maxSize){
            throw new RuntimeException(obj+" add error, queue is full");
        }
        //循环队列，首尾结合，下标控制队首和队尾位置
        if(bottom == maxSize-1){
            bottom = -1;
        }
        objQueue[++bottom] = obj;
        item++;
    }
    /**
     * 出队
     * @return
     */
    public Object out(){
        if(item == 0){
            throw new RuntimeException("queue is null");
        }
        Object obj = objQueue[top];
        //声明原顶栈可以回收空间(GC)
        objQueue[top] = null;
        top++;
        //重置下标
        if(top == maxSize){
            top = 0;
        }
        item--;
        return obj;
    }
    //链式存储结构实现
    private class NodeQueue<Object>{
        private Object data;
        private NodeQueue next;
        public NodeQueue(Object data, NodeQueue next) {
            this.data = data;
            this.next = next;
        }
    }
    //队列头 出
    private NodeQueue queueTop;
    //队列尾 进
    private NodeQueue queueBottom;
    //队列大小
    private int size;
    public Queue() {
        queueTop = null;
        queueBottom = null;
        size = 0;
    }
    /**
     * 入队
     * @param obj
     */
    public void addNodeQueue(Object obj){
        if(size == 0){
            queueTop = new NodeQueue(obj,null);
            //指向同一存储地址
            queueBottom = queueTop;
        }else{
            NodeQueue<Object> nodeQueue = new NodeQueue(obj,null);
            //让尾节点的next指向新增的节点
            queueBottom.next = nodeQueue;
            //以新节点作为尾节点
            queueBottom = nodeQueue;
        }
        size ++;
    }
    /**
     * 出队
     * @return
     */
    public Object removeNodeQueue(){
        if(size == 0){
            throw new RuntimeException("queue is null");
        }
        NodeQueue nodeQueue = queueTop;
        queueTop = queueTop.next;
        //声明原队列头next可以回收空间(GC)
        nodeQueue.next = null;
        size--;
        return nodeQueue.data;
    }
    @Override
    public String toString() {
        StringBuilder sb = new StringBuilder("{ ");
        for(NodeQueue nodeQueue = queueTop ; nodeQueue != null ; nodeQueue = nodeQueue.next){
            sb.append(nodeQueue.data.toString()+" ");
        }
        return sb.toString()+"}";
    }
    public static void main(String[] args) {
        Queue queue = new Queue();
        queue.addNodeQueue("123");
        queue.addNodeQueue("abc");
        queue.addNodeQueue("ddd");
        System.out.println(queue);
        queue.removeNodeQueue();
        System.out.println(queue);
        queue.removeNodeQueue();
        queue.removeNodeQueue();
        System.out.println(queue);
    }
}
```

### Linked List

- 链表是由节点组成的线性集合，每个节点可以利用指针指向其它节点。它是一种包含了多个节点能够用于表示序列的数据结构。
- 单向链表：链表中的节点仅指向下一个节点，并且最后一个节点指向空。
- 双向链表：每个节点具有两个指针p、n 使得p指向先前节点，n指向下一节点。
- 循环链表：每个节点指向下一个节点并且最后一个节点指向第一个节点的链表。
- 时间复杂度：索引O(n)，搜索O(n)，插入O(1)，移除O(1)

#### 示例：

```java
package com.lsaiah.datatructure;
public class LinkedList {
    /**
     * 链表通常由一连串节点组成，每个节点包含任意的实例数据（data fields）和一或两个用来指向上一个/或下一个节点的位置的链接（"links"）
     */
    private Node head; //链表头
    private Node tail; //链表尾
    private int size; //节点数
    /**
     * 双端链表
     */
    public class Node{
        private Object data;
        private Node prev; //上一个
        private Node next; //下一个

        public Node(Object data) {
            this.data = data;
        }
    }
    public LinkedList() {
        this.head = null;
        this.tail = null;
        this.size = 0;
    }
    /**
     * 向链表头添加数据
     * @param object
     */
    public void addHead(Object object){
        Node node = new Node(object);
        if(size == 0){
            head = node;
            tail = node;
            size++;
        }else{
            head.prev = node;
            node.next = head;
            head = node;
            size++;
        }
    }
    /**
     * 删除头
     */
    public void deleteHead(){
        //头部指向下一个，prev值为null则说明是链表的头部
        if(size != 0){
            head.prev = null;
            head = head.next;
            size--;
        }
    }
    /**
     *向链表尾添加数据
     * @param object
     */
    public void addTail(Object object){
        Node node = new Node(object);
        if(size == 0){
            head = node;
            tail = node;
            size++;
        }else{
            node.prev = tail;
            tail.next = node;
            tail = node;
            size++;
        }
    }
    /**
     * 删除尾部
     */
    public void deleteTail(){
        //尾部指向上一个，next值为null则说明是链表的尾部
        if(size != 0){
            tail.next = null;
            tail = tail.prev;
            size--;
        }
    }
    /**
     * 显示数据
     */
    public void display(){
        Node node = head;
        while (size > 0){
            System.out.print("["+node.data+"->");
            node = node.next;
            size--;
        }
    }

    /**
     * 不调用库函数实现链表反转
     */
    public void inverse(String str){
        char[] chars = str.toCharArray();
        for (int i = 0; i < chars.length/2; i++) {
            char temp = chars[i];
            chars[i] = chars[chars.length-i-1];
            chars[chars.length-i-1] = temp;
        }
    }

    public static void main(String[] args) {
        LinkedList linkedList = new LinkedList();
        linkedList.addHead("123");
//        linkedList.addHead("abc");
//        linkedList.addHead("%$$");
//        linkedList.addTail("+_+");
//        linkedList.addTail("hello");
        linkedList.addTail("word");
        linkedList.deleteHead();
        linkedList.deleteTail();
        linkedList.display();
        linkedList.inverse("abcdefg");
    }
}
```

### Binary Tree

二叉树是由一个根节点和两颗互不相交的（分别称为左子树和右子树）组成，二叉树即是每个节点最多包含左子节点与右子节点这两个节点的树形数据结构。

- 满二叉树：树种的每个节点仅包含0或2个节点
- 完美二叉树：二叉树中的每个叶节点都拥有两个子节点，并且具有相同的高度
- 完全二叉树：除最后一层外，每一层上的结点数均达到最大值，在最后一层上只缺少右边的若干节点
- 红黑树：每个节点都带有颜色和属性，根是黑色，叶子是黑色，每个红色节点必须有两个黑色子节点，从任一节点到其每个叶子的所有简单路径都包含相同数目的黑色节点。

![满二叉树、完全二叉树、完美二叉树](https://lsaiah-1259166707.cos.ap-shanghai.myqcloud.com/图片/1094457-20170225184451991-1942362001.png)

![红黑树](https://lsaiah-1259166707.cos.ap-shanghai.myqcloud.com/图片/1355319681_6107.png)

![搜索树](https://lsaiah-1259166707.cos.ap-shanghai.myqcloud.com/%E5%9B%BE%E7%89%87/TIM%E5%9B%BE%E7%89%8720200306191657.png)

#### 树的深度优先遍历算法

深度优先搜索就是在搜索树的每一层始终只扩展一个子节点，不断地向下一层前进（到达叶子结点或受到深度限制）时，才从当前节点返回到上一级节点，沿着另一个方向继续前进。如上图所示的搜索树深度遍历的结果为ABCDEFGHK。

#### 树的广度优先遍历算法

广度优先搜索是深度越大的节点越先得到扩展，本层的节点没有搜索处理完时不能对下层节点进行处理。如上图所示的搜索树广度遍历的结果为ABECFDGHK。

```java
package com.lsaiah.datatructure;
import java.util.ArrayDeque;

public class BinaryTree {
    static class TreeNode {
        int value;
        TreeNode left;
        TreeNode right;

        public TreeNode(int value) {
            this.value = value;
        }
    }
    TreeNode root;
    public BinaryTree(int[] array) {
        root = createBinaryTree(array, 0);
    }
    //利用二叉树的数组表示法构建二叉树，并返回根节点。
    public TreeNode createBinaryTree(int[] array, int index) {
        if (index < array.length) {
            int value = array[index];
            TreeNode root = new TreeNode(value);
            root.left = createBinaryTree(array, index * 2 + 1);
            root.right = createBinaryTree(array, index * 2 + 2);
            return root;
        }
        return null;
    }
    //深度优先遍历采用栈的非递归实现
    public void depthOrderTraversal(TreeNode root) {
        if (root == null) {
            System.out.println("空树！");
            return;
        }
        ArrayDeque<TreeNode> stack = new ArrayDeque<TreeNode>();
        stack.push(root);
        while (!stack.isEmpty()) {
            TreeNode node = stack.pop();
            System.out.println(node.value + "  ");
            //先让右结点入栈，以致在pop时确保左结点先出栈
            if (node.right != null) {
                stack.push(node.right);
            }
            if (node.left != null) {
                stack.push(node.left);
            }
        }
        System.out.println("\n");
    }
    //深度优先遍历的递归实现
    public void depthOrderTraversalWithRecursion(TreeNode root) {
        if (root != null) {
            System.out.println(root.value);
        }
        depthOrderTraversal(root.left);
        depthOrderTraversal(root.right);
    }

    //广度优先遍历采用队列的非递归实现。广度优先遍历无法用递归实现
    public void breadthOrderTraversal(TreeNode root) {
        if (root == null) {
            System.out.println("空树！");
            return;
        }
        ArrayDeque<TreeNode> queue = new ArrayDeque<TreeNode>();
        queue.add(root);
        while (!queue.isEmpty()) {
            TreeNode node = queue.remove();
            System.out.println(node.value + "  ");
            if (node.left != null) {
                queue.add(node.left);
            }
            if (node.right != null) {
                queue.add(node.right);
            }
        }
        System.out.println("\n");
    }
    public static void main(String[] args) {
        int[] array = {1, 2, 3, 4, 5, 6, 7};
        BinaryTree binaryTree = new BinaryTree(array);
//        binaryTree.depthOrderTraversal(binaryTree.root);
//        binaryTree.depthOrderTraversalWithRecursion(binaryTree.root);
        binaryTree.breadthOrderTraversal(binaryTree.root);
    }
}
```

#### 二叉树先序，中序，后序遍历

先根遍历法步骤：

1. 访问根节点
2. 先序遍历左子树
3. 先序遍历右子树

如图所示搜索树先根遍历结果：ABDCEFGHK

中根遍历法步骤：

1. 中序遍历左子树
2. 访问根节点
3. 中序遍历右子树

如图所示搜索树中根遍历结果：BDCAEHGKF

后根遍历步骤：

1. 后序遍历左子树
2. 后序遍历右子树
3. 访问根节点

如图所示搜索树后根遍历结果：DCBAHKGFEA

### Heap

- 堆也被称为优先队列（队列+排序规则）
- 堆是一种特殊的基于树的满足某些特性的数据结构，整个堆中的所有父子节点的键值都会满足相同的排序条件。堆更准确地可以分为最大堆和最小堆，在最大堆中父节点的键值永远大于或者等于子节点的键值，且整个堆中最大值存储于根节点，而在最小堆中父节点的键值永远小于或者等于子节点的键值，且整个堆中最小值存储于根节点。
- 时间复杂度：访问最大值/最小值O(1)，插入O(log(n))，移除最大值/最小值O(log(n))

### Hashing

- 哈希能够将任意长度的数据映射到固定长度的数据，哈希函数返回哈希值，如果两个不同的键得到相同的哈希值，即称为碰撞。
- Hash Map: 是一种能够建立起键与值之间关系的数据结构，HashMap能够使用哈希函数将键转化为桶或者槽中的下标，从而优化对于目标值的搜索速度。
- 碰撞解决：
  - 链地址法：链地址中每个桶是相互独立的，包含一系列索引的列表，搜索操作的时间复杂度即是搜索桶的时间与遍历列表时间之和。
  - 开地址法：在开地址中当插入新值时会判断该值对应的哈希桶是否存在，如果存在则根据某种算法依次选择下一个可能的位置，直到找到一个尚未被占用的地址，所谓开地址法也是指某个元素的位置并不永远由哈希值决定。

![](https://lsaiah-1259166707.cos.ap-shanghai.myqcloud.com/图片/1659e637f1104cfa.jpg)

### Graph

图是一种数据元素之间多对多关系的数据结构，加上一组基本操作构成的抽象数据类型。

- 无向图：无向图具有对称的邻接矩阵，因此如果存在某条从节点u到节点v的边，反而从v到u的边也存在
- 有向图：有向图的邻接矩阵是非对称的，即如果存在u到v的边并不意味着一定存在从v到u的边。

![无向图](https://lsaiah-1259166707.cos.ap-shanghai.myqcloud.com/图片/1571057865-4e3ecf8192f1dde.png)

![有向图](https://lsaiah-1259166707.cos.ap-shanghai.myqcloud.com/图片/1571057866-f253884314af86b.png)

## 算法

<a href="https://www.cxyxiaowu.com/">算法讲解</a>&nbsp;&nbsp;<a href="http://www.csie.ntnu.edu.tw/~u91029/">算法笔记</a>&nbsp;&nbsp;&nbsp;&nbsp;<a href="http://uoj.ac/">在线题库</a>&nbsp;&nbsp;&nbsp;&nbsp;<a href="http://poj.org/">北大题库</a>&nbsp;&nbsp;&nbsp;&nbsp;<a href="https://projecteuler.net/">数学编程题库</a>&nbsp;&nbsp;&nbsp;&nbsp;<a href="https://vijos.org/">国内入门</a>&nbsp;&nbsp;&nbsp;&nbsp;<a href="https://www.hackerrank.com/">在线挑战</a>&nbsp;&nbsp;&nbsp;&nbsp;<a href="https://leetcode.com/">力扣</a>

### 快速排序

- 稳定：否
- 时间复杂度：最优时间O(nlog(n))，最坏时间O(n^2)，平均时间O(nlog(n))

![](https://lsaiah-1259166707.cos.ap-shanghai.myqcloud.com/图片/1659e6abe14838f2.gif)

### 归并排序

- 归并排序是典型的分治算法，它不断地将某个数组分为两个部分，分别对左子数和右子数进行排序，然后将两个数组合并为新的有序数组。
- 稳定：是
- 时间复杂度：最优时间O(nlog(n))，最坏时间O(nlog(n))，平均时间O(nlog(n))

### 桶排序

- 桶排序将数组分到有限数量的桶子里，每个桶子再个别排序（有可能再使用别的排序算法或是以递归方式继续使用桶排序进行排序）。
- 时间复杂度：最有时间Ω(n + k)，最坏时间O(n^2)，平均时间Θ(n + k)

![](https://lsaiah-1259166707.cos.ap-shanghai.myqcloud.com/图片/1659e736df1446e0.jpg)

### 基数排序

- 基数排序类似于桶排序，将数组分割到有限数目的桶中，不过在其分割之后并没有让每个桶单独地进行排序，而是直接进行了合并操作。
- 时间复杂度：最优时间Ω(nk)，最坏时间O(nk)，平均时间Θ(nk)

### 静态查找

- 顺序查找时间复杂度O(n) ，适合于存储结构为顺序存储或链表存储的线性表。它的查找过程是：从表中第一个（或最后一个）记录开始，逐个进行记录的关键字和给定值比较，若某个记录的关键字和给定值相等，则查找成功，找到所查的记录；如果直到最后一个（或第一个）记录，其关键字和给定值比较都不等时，则表中没有所查的记录，查找不成功。

```java
package com.lsaiah.datatructure;

import java.util.Scanner;
import org.junit.jupiter.api.Test;

public class Search {
    public int search(int[] arr,int num) {
        if(arr.length==0) {
            return -1;
        }
        for(int i=0;i<arr.length;i++) {
            if(arr[i]==num) {
                return i;
            }
        }
        return -1;
    }

    @Test
    public void test() {
        int[] arr={4,6,2,8,1,9,0,3};
        Scanner input=new Scanner(System.in);
        System.out.println("请输入你要查找的数:");
        int num=input.nextInt();
        int result=search(arr,num);
        if(result==-1){
            System.out.println("你输入的数不存在与数组中");
        }
        else {
            System.out.println("你输入的数字存在，在数组中的位置是第："+(result+1)+"个");
        }
    }
}
```

- 二分查找（拆半查找）属于有序查找，时间复杂度O(log(n))。前提是线性表中的记录必须是关键码有序（通常从小到大有序），线性表必须采用顺序存储。折半查找的基本思想是：在有序表中，取中间记录作为比较对象，若给定值与中间记录的关键字相等，则查找成功；若给定值小于中间记录的关键字，则在中间记录的左半区继续查找；若给定值大于中间记录的关键字，则在中间记录的右半区继续查找。不断重复上述过程，直到查找成功，或所查找区域无记录，查找失败为止。

```java
package com.lsaiah.datatructure;
import org.junit.jupiter.api.Test;

public class BinarySearch {
    //1.循环实现二分查找
    public int rank(int[] arr,int num) {
        int start=0;
        int end=arr.length;
        int mid=(start+end)/2;//中间数的下标
        while(start<=end) {//退出循环的条件 若一直没找到这个数，则会退出循环
            if(arr[mid]==num)
                return mid;
            else if(arr[mid]>num) {
                end=mid-1;
            }else {
                start=mid+1;
            }
            mid=(start+end)/2;
        }
        return -1;
    }

    //2.递归实现二分查找
    public int recursion(int[] arr,int num,int start,int end) {
        int mid=(start+end)/2;
        if(start==end&&arr[mid]!=num) {
            return -1;
        }
        if(arr[mid]==num) {
            return mid;
        }else {
            if(arr[mid]>num) {
                end=mid-1;
                return recursion(arr,num,start,end);
            }else{
                start=mid+1;
                return recursion(arr,num,start,end);
            }
        }
    }

    @Test
    public void testRank() {
        int[] arr= {2,3,6,9,13,18,20,22,24,29,30,45,67,88};
        int result=rank(arr,20);
        if(result==-1) {
            System.out.println("你要查找的数字不在该数组中");
        }else {
            System.out.println("你查找的数字在数组的第"+(result+1)+"位");
        }
    }

    @Test
    public void testRecursion() {
        int[] arr= {2,3,6,9,13,18,20,22,24,29,30,45,67,88};
        int result=recursion(arr,22,0,arr.length-1);
        if(result==-1) {
            System.out.println("你要查找的数字不在该数组中");
        }else {
            System.out.println("你查找的数字在数组的第"+(result+1)+"位");
        }
    }
}
```

### 动态查找

- 二叉树查找，时间复杂度O(log(n))，但是最坏情况会退化为O(n)。特点是表结构本身在查找过程中动态生成，对二叉查找树进行中序遍历得到有序的数列，给定关键字Key，将key和节点的key比较，如果小于就在左节点查找，如果大于就在右节点查找，如果相等返回Value。
- 差值查找是根据要查找的关键字key与查找表中最大最小记录的关键字比较后的查找方法，其核心就在于插值的计算公式。插值计算公式：mid=start+(key-a[start])/(arr[end]-arr[start])*(end-start)；

```java
package com.lsaiah.datatructure;
import org.junit.jupiter.api.Test;

public class InsertValueSearch {

    //递归实现插值查找
    public int recursionBinarySearch(int[] arr,int key,int start,int end) {
        //当查找的值小于数组最小值,或者大于数组最大值,或者start>end时查找结束
        if(key<arr[start]||key>arr[end]||start>end) {
            return -1;
        }
        if(arr[start]==key) {
            return start;
        }
        if(arr[end]==key) {
            return end;
        }
        int mid = start+(end-start)*((key-arr[start])/(arr[end]-arr[start]));
        if(arr[mid]>key) {
            //说明key在前半部分
            return recursionBinarySearch(arr,key,start,mid-1);
        }
        else if(arr[mid]<key) {
            //说明key在后半部分
            return recursionBinarySearch(arr,key,mid+1,end);
        }else {
            return mid;
        }
    }
    //非递归实现插值查找
    public int commonBinarySearch(int[] arr,int key) {
        int start=0;
        int end=arr.length-1;
        if(key<arr[start]||key>arr[end]) {
            return -1;
        }
        int mid = start+(end-start)*((key-arr[start])/(arr[end]-arr[start]));
        while(start<=end) {
            if(arr[mid]==key) {
                return mid;
            }else if(arr[mid]>key) {//说明查找的值在mid的前面
                end=mid-1;
            }else {//说明查找的值在mid的后面
                start=mid+1;
            }
            mid=start+(end-start)*((key-arr[start])/(arr[end]-arr[start]));
        }
        return -1;
    }

    //递归测试
    @Test
    public void testRecursion() {
        int[] arr= {1,2,4,5,6,7,8,9};
        int result=recursionBinarySearch(arr,1,0,arr.length-1);
        if(result==-1) {
            System.out.println("你要查找的数不在该数组中");
        }else {
            System.out.println("你要查找的数在数组的第"+(result+1)+"个位置");
        }

    }
    //非递归测试
    @Test
    public void testCommon() {
        int[] arr= {1,2,4,5,6,7,8,9};
        int result=commonBinarySearch(arr,1);
        if(result==-1) {
            System.out.println("你要查找的数不在该数组中");
        }else {
            System.out.println("你要查找的数在数组的第"+(result+1)+"个位置");
        }
    }
}
```

- 斐波那契查找是在二分查找的基础上，用斐波那契数列进行分割，在斐波那契数列上找一个略大于查找元素个数的值f(n)，将查找元素表个数扩充到f(n)，如果要补充元素用最后一个元素补充，完成后对f(n)个元素进行斐波那契分割成前面f(n-1)个元素，后面f(n-2)个元素，对要查找元素的那个部分进行递归。平均性能优于二分查找，但是如果一直在左边长半区查找则低于二分查找。

```java
package com.lsaiah.datatructure;
import java.util.Arrays;
import org.junit.jupiter.api.Test;

public class FibonacciSearch {
    private static int maxsize=20;
    //生成斐波那契数列
    public int[] fibonaqie() {
        int[] f=new int[maxsize];
        f[0]=1;
        f[1]=1;
        for(int i=2;i<maxsize;i++) {
            f[i]=f[i-1]+f[i-2];
        }
        return f;
    }

    //查找
    public int search(int[] arr,int key) {
        int low=0;
        int high=arr.length-1;
        int k=0;//斐波那契分割数值下标
        int mid=0;
        int f[]=fibonaqie();
        //获得斐波那契分割数值下标
        while(high>f[k]-1) { //int [] a= {1,3,5,7,9,11,12};  6
            k++;
        }
        //利用Java工具类Arrays 构造新数组并指向 数组 arr[]
        int[] temp= Arrays.copyOf(arr, f[k]);
        //对新构造的数组进行 元素补充
        for(int i=high+1;i<temp.length;i++) {
            temp[i]=arr[high];
        }
        while(low<=high) {
            //由于前面部分有f[k-1]个元素
            mid=low+f[k-1]-1;
            if(key<temp[mid]) {
                high=mid-1;
                /**
                 *
                 *全部元素=前部元素+后部元素
                 * f[k]=f[k-1]+f[k-2]
                 * 因为前部有f[k-1]个元素,所以可以继续拆分f[k-1]=f[k-2]+f[k-3]
                 * 即在f[k-1]的前部继续查找 所以k--
                 * 即下次循环 mid=f[k-1-1]-1
                 * */
                k--;
            }else if(key>temp[mid]) {//关键字大于切个位置元素 则查找后半部分
                low=mid+1;
                /***
                 * 全部元素=前部元素+后部元素
                 * f[k]=f[k-1]+f[k-2]
                 * 因为后部有f[k-2]个元素,所以可以继续拆分f[k-2]=f[k-3]+f[k-4]
                 * 即在f[k-2]的前部继续查找 所以k-=2
                 * 即下次循环 mid=f[k-1-2]-1
                 */
                k-=2;
            }else {
                if(mid<=high) {
                    return mid;
                }else {
                    return high;
                }
            }
        }
        return -1;
    }
    @Test
    public void test() {
        int [] a= {1,3,5,7,9,11,12};
        int i=search(a, 5);
        System.out.println("5在："+(i+1));
        int j=search(a, 12);
        System.out.println("12在："+(j+1));
    }
}
```

### 哈希查找

- 对于无冲突的Hash表查找复杂度为O(1)

## 图算法

图的遍历是指从图的任一顶点出发，对图中的所有顶点访问一次且只访问一次。

![](https://lsaiah-1259166707.cos.ap-shanghai.myqcloud.com/图片/图算法1583738176.png)

### 深度优先遍历

- 深度优先搜索是一种先遍历子节点而不是回溯的算法，如上图所示深度优先遍历的顺序为136245。
- 时间复杂度：O(|V| + |E|)

### 广度优先遍历

- 广度优先搜索是优先遍历邻节点而不是子节点的算法，如上图所示广度优先遍历的顺序为123456。
- 时间复杂度：O(|V| + |E|)

```java
package com.lsaiah.datatructure;
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.Queue;
import java.util.Stack;

//邻接矩阵
public class Graph {
    public ArrayList<String> vertexList;//存储节点
    public int [][]edges;//邻接矩阵，用来存储边
    public int numofEdges;//边的数目
    //初始化矩阵
    public  Graph(int n) {
        vertexList=new ArrayList<String>(n);
        edges=new int[n][n];
        numofEdges=0;
    }
    //得到节点的个数
    public int getNumofVertex() {
        return vertexList.size();
    }
    //得到边的个数
    public int getNunofEdges() {
        return numofEdges;
    }
    //返回两个节点之间的权值
    public int getWeight(String temp1,String temp2) {
        int i=vertexList.indexOf(temp1);
        int j=vertexList.indexOf(temp2);
        return edges[i][j];
    }
    //插入节点
    public void InsertVertex(String vertex) {
        vertexList.add(vertex);
    }
    //插入边
    public void InsertEdge(int v1,int v2,int weight) {
        edges[v1][v2]=weight;
        edges[v2][v1]=weight;
        numofEdges++;
    }
    public void deleteEdge(int v1,int v2) {
        edges[v1][v2]=0;
        numofEdges--;
    }
}
//利用堆栈的方式实现图的深度遍历
class Traversing
{
    public String DFS(String startnode,ArrayList<String> vertexList,int [][]edges) {
        if (!vertexList.contains(startnode)) {
            System.out.print("输入节点不在该图内");
            return null;
        }
        int startindex=vertexList.indexOf(startnode);
        int numOfNodes=vertexList.size();
        boolean[]visted=new boolean[numOfNodes];
        StringBuilder resultBuilder=new StringBuilder();
        Stack<Integer> stack=new Stack<Integer>();
        stack.push(startindex);
        visted[startindex]=true;
        while (!stack.isEmpty()) {
            int v=stack.pop();
            resultBuilder.append(vertexList.get(v)+",");
            for(int i=0;i<numOfNodes;i++)
            {
                //当edges[v][i]的值不为0，不为最大，且没有被访问时，将其压入栈中
                if((edges[v][i]!=0)&&(edges[v][i]!=Integer.MAX_VALUE)&&!visted[i])
                {
                    stack.push(i);
                    visted[i]=true;
                }
            }
        }
        return resultBuilder.length()>0?resultBuilder.substring(0,resultBuilder.length()-1):null;
    }
    //利用队列的方式实现图的广度遍历
    public String BFS(String startnode,ArrayList<String> vertexList,int [][]edges) {
        if (!vertexList.contains(startnode)) {
            System.out.print("输入节点不在该图内");
            return null;
        }
        StringBuilder resultBuilder=new StringBuilder();
        boolean []visited=new boolean[vertexList.size()];
        int startIndex=vertexList.indexOf(startnode);
        Queue<Integer>queue=new LinkedList<Integer>();
        queue.offer(startIndex);
        visited[startIndex]=true;
        while (!queue.isEmpty()) {
            int v=queue.poll();
            resultBuilder.append(vertexList.get(v)+",");
            for(int i=0;i<vertexList.size();i++)
            {
                if((edges[v][i]!=0) &&( edges[v][i]!=Integer.MAX_VALUE)&&!visited[i])
                {
                    queue.offer(i);
                    visited[i]=true;
                }
            }
        }
        return resultBuilder.length()>0?resultBuilder.substring(0,resultBuilder.length()-1):null;
    }
}

class Test {
    public static void main(String[] args) {
        //构建邻接矩阵
        Graph tuGraph=new Graph(6);
        //插入节点
        tuGraph.InsertVertex("V1");
        tuGraph.InsertVertex("V2");
        tuGraph.InsertVertex("V3");
        tuGraph.InsertVertex("V4");
        tuGraph.InsertVertex("V5");
        tuGraph.InsertVertex("V6");
        //插入边
        tuGraph.InsertEdge(0, 1, 1);
        tuGraph.InsertEdge(0, 2, 1);
        tuGraph.InsertEdge(1, 3, 1);
        tuGraph.InsertEdge(1, 4, 1);
        tuGraph.InsertEdge(2, 5, 1);
        for(int i=0;i<6;i++)
        {
            for(int j=0;j<6;j++)
            {
                System.out.print(tuGraph.edges[i][j]+"  ");
            }
            System.out.println();
        }
        //深度遍历邻接矩阵
        Traversing depthTraversing=new Traversing();
        String resultStringDFS=depthTraversing.DFS("V1", tuGraph.vertexList, tuGraph.edges);
        System.out.println(resultStringDFS);
        //广度遍历邻接矩阵
        String resultStringBFS=depthTraversing.BFS("V1", tuGraph.vertexList, tuGraph.edges);
        System.out.println(resultStringBFS);
    }
}
```

### 拓扑排序

.

## Dijkstra 算法

.

## Bellman-Ford 算法

.

## Floyd-Warshall 算法

.

## Prim 算法

.

## Kruskal 算法

.

## 位运算

.

## 习题

.