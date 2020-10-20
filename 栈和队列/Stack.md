# Stack

## 232 [用栈实现队列](https://leetcode-cn.com/problems/implement-queue-using-stacks/)

###  问题

使用栈实现队列的下列操作：

`push(x) `-- 将一个元素放入队列的尾部。
`pop()` -- 从队列首部移除元素。
`peek()` -- 返回队列首部的元素。
`empty()` -- 返回队列是否为空。


示例:

```
MyQueue queue = new MyQueue();

queue.push(1);
queue.push(2);  
queue.peek();  // 返回 1
queue.pop();   // 返回 1
queue.empty(); // 返回 false
```


说明:

你只能使用标准的栈操作 -- 也就是只有 `push to top`, `peek/pop from top`, `size`, 和 `is empty` 操作是合法的。
你所使用的语言也许不支持栈。你可以使用 list 或者 deque（双端队列）来模拟一个栈，只要是标准的栈操作即可。
假设所有操作都是有效的 （例如，一个空的队列不会调用 pop 或者 peek 操作）。

### 思路 使用两个栈 入队 O（1）  出队 O（1）

**入队：**

```
新元素总是压入s1的栈顶，同时我们会把s1中压入的第一个元素赋值给做为队首元素的font变量
```

**出队：**

```
根据栈的先进后出的特性，s1中第一个压入的元素在栈底，我们的把s1中所有的元素全部弹出，再把它们压入到另一个栈s2中，这个操作会让元素的入栈顺序反转过来。这样，s1中栈底就变成了s2的栈顶元素，这样就可以直接在s2将它弹出，一旦s2空了，我们只需要s1中元素再一次转移到s2就行了
```

**取队首元素（peek）**：

```
我们定义了 front 变量来保存队首元素，每次 入队 操作我们都会随之更新这个变量。当 s2 为空，front 变量就是对首元素，当 s2 非空，s2 的栈顶元素就是队首元素。
```

**判断空（empty）：**

```
s1 和 s2 都存有队列的元素，所以只需要检查 s1 和 s2 是否都为空就可以了。
```

### 代码

```java
class MyQueue {
    
    private Stack<Integer> s1 = new Stack<>();
    private Stack<Integer> s2 = new Stack<>();
    private int front;

    /** Initialize your data structure here. */
    public MyQueue() {
        // Stack<Integer> s1 = new Stack<>();
        // Stack<Integer> s2 = new Stack<>();
        int front;
    }
    
    /** Push element x to the back of queue. */
    public void push(int x) {
        if (s1.empty())
        front = x;
        s1.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    public int pop() {
        if (s2.isEmpty()) {
        while (!s1.isEmpty())
            s2.push(s1.pop());
        }
        return s2.pop();    
    }
    
    /** Get the front element. */
    public int peek() {
        if (!s2.isEmpty()) {
        return s2.peek();
        }
        return front;
    }
    
    /** Returns whether the queue is empty. */
    public boolean empty() {
        return s1.isEmpty() && s2.isEmpty();
    }
}

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue obj = new MyQueue();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.peek();
 * boolean param_4 = obj.empty();
 */
```

![image-20201017223454067](C:\Users\Auraros\AppData\Roaming\Typora\typora-user-images\image-20201017223454067.png)



## 225 用队列实现栈

### 问题

使用队列实现栈的下列操作：

`push()`-- 元素 x 入栈
`pop()` -- 移除栈顶元素
`top()` -- 获取栈顶元素
`empty() `-- 返回栈是否为空
注意:

你只能使用队列的基本操作-- 也就是 `push to back`, `peek/pop from front`, `size`, 和` is empty` 这些操作是合法的。
你所使用的语言也许不支持队列。 你可以使用 list 或者 deque（双端队列）来模拟一个队列 , 只要是标准的队列操作即可。
你可以假设所有操作都是有效的（例如, 对一个空的栈不会调用 pop 或者 top 操作）。

### 思路  入栈 O(n)  出栈O(1）

**入栈**

```
初始化一个队列，每次入队列的时候只要把队列前面的数依次放到后面即可
```

### 代码

```java
class MyStack {

    Queue<Integer> queue1;
    // private int tail;

    /** Initialize your data structure here. */
    public MyStack() {
        queue1 = new LinkedList<Integer>();
    }
    
    /** Push element x onto stack. */
    public void push(int x) {
        queue1.offer(x);
        while(queue1.peek() != x){
            if(queue1.isEmpty()){
                break;
            }else{
                int tmp = queue1.poll();
                queue1.offer(tmp);
            }
        }
    }
    
    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        return queue1.poll();
    }
    
    /** Get the top element. */
    public int top() {
        return queue1.peek();
    }
    
    /** Returns whether the stack is empty. */
    public boolean empty() {
        return queue1.isEmpty();
    }
}

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack obj = new MyStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.top();
 * boolean param_4 = obj.empty();
 */
```

![image-20201017230810286](C:\Users\Auraros\AppData\Roaming\Typora\typora-user-images\image-20201017230810286.png)



## 150 逆波兰表达式求值

### 问题

根据 逆波兰表示法，求表达式的值。有效的运算符包括 +, -, *, / 。每个运算对象可以是整数，也可以是另一个逆波兰表达式。

说明：

整数除法只保留整数部分。
给定逆波兰表达式总是有效的。换句话说，表达式总会得出有效数值且不存在除数为 0 的情况。


示例 1：

```
输入: ["2", "1", "+", "3", "*"]
输出: 9
解释: 该算式转化为常见的中缀算术表达式为：((2 + 1) * 3) = 9
示例 2：

输入: ["4", "13", "5", "/", "+"]
输出: 6
解释: 该算式转化为常见的中缀算术表达式为：(4 + (13 / 5)) = 6
示例 3：

输入: ["10", "6", "9", "3", "+", "-11", "*", "/", "*", "17", "+", "5", "+"]
输出: 22
```

解释: 

```
该算式转化为常见的中缀算术表达式为：
  ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22
```

### 思路

```
建立一个栈，只要是数字就入栈，是符号就提出两个计算后放入站
```

### 代码

```java
class Solution {
    public int evalRPN(String[] tokens) {
        Stack<Integer> stack = new Stack<>();

        for(String token : tokens){
            if(token.equals("+")){
                int num1 = stack.pop();
                int num2 = stack.pop();
                int sum = num1 + num2;
                stack.push(sum);
            }else if(token.equals("*")){
                int num1 = stack.pop();
                int num2 = stack.pop();
                int sum = num1 * num2;
                stack.push(sum);
            }else if(token.equals("-")){
                int num1 = stack.pop();
                int num2 = stack.pop();
                int sum =  num2 - num1;
                stack.push(sum);
            }else if(token.equals("/")){
                int num1 = stack.pop();
                int num2 = stack.pop();
                int sum =  num2 / num1;
                stack.push(sum);
            }else{
                stack.push(Integer.valueOf(token));
            }
        }
        return stack.pop();
    }
}
```

![image-20201017232858582](C:\Users\Auraros\AppData\Roaming\Typora\typora-user-images\image-20201017232858582.png)



## 71. 简化路径

以 Unix 风格给出一个文件的绝对路径，你需要简化它。或者换句话说，将其转换为规范路径。

在 Unix 风格的文件系统中，一个点（.）表示当前目录本身；此外，两个点 （..） 表示将目录切换到上一级（指向父目录）；两者都可以是复杂相对路径的组成部分。更多信息请参阅：Linux / Unix中的绝对路径 vs 相对路径

请注意，返回的规范路径必须始终以斜杠 / 开头，并且两个目录名之间必须只有一个斜杠 /。最后一个目录名（如果存在）不能以 / 结尾。此外，规范路径必须是表示绝对路径的最短字符串。

示例 1：

```
输入："/home/"
输出："/home"
解释：注意，最后一个目录名后面没有斜杠。
```


示例 2：

```
输入："/../"
输出："/"
解释：从根目录向上一级是不可行的，因为根是你可以到达的最高级。
```


示例 3：

```
输入："/home//foo/"
输出："/home/foo"
解释：在规范路径中，多个连续斜杠需要用一个斜杠替换。
```


示例 4：

```
输入："/a/./b/../../c/"
输出："/c"
```


示例 5：

```
输入："/a/../../b/../c//.//"
输出："/c"
```


示例 6：

```
输入："/a//b////c/d//././/.."
输出："/a/b/c"
```



### 思路

```
先将输入根据/为分隔符转化为列表,这样可以解决多个/的问题
"/a//b////c/d//././/.."
转为
['a','b','c','d','.','..']
然后遍历每个元素
遇到字母就存到栈中
遇到 . 不管他
遇到 .. 就取出一个栈
```

### 代码

```java
class Solution {
    public String simplifyPath(String path) {
        Stack<String> stack  = new Stack<>();
        String[] str = path.split("/");
        for(String s : str){
            if(s.equals("..")){
                if(!stack.isEmpty())
                    stack.pop();
            }else if(!s.equals("") && !s.equals(".")){
                stack.push(s);
            }
        }
        if(stack.isEmpty()){
            return "/";
        }
        StringBuilder ans = new StringBuilder();
        for(int i=0; i<stack.size(); i++){
            ans.append("/"+stack.get(i));
        }
        return ans.toString();
    }
}
```

![image-20201017235542000](C:\Users\Auraros\AppData\Roaming\Typora\typora-user-images\image-20201017235542000.png)



## 388 [文件的最长绝对路径](https://leetcode-cn.com/problems/longest-absolute-file-path/)

假设我们以下述方式将我们的文件系统抽象成一个字符串:

字符串` dir\n\tsubdir1\n\tsubdir2\n\t\tfile.ext `表示:

```
dir
    subdir1
    subdir2
        file.ext
```


目录 dir 包含一个空的子目录` subdir1` 和一个包含一个文件 file.ext 的子目录 `subdir2` 。

字符串 `dir\n\tsubdir1\n\t\tfile1.ext\n\t\tsubsubdir1\n\tsubdir2\n\t\tsubsubdir2\n\t\t\tfile2.ext` 表示:

```
dir
    subdir1
        file1.ext
        subsubdir1
    subdir2
        subsubdir2
            file2.ext
```


目录 dir 包含两个子目录 `subdir1` 和 `subdir2`。 `subdir1` 包含一个文件 `file1.ext` 和一个空的二级子目录 `subsubdir1`。`subdir2` 包含一个二级子目录 `subsubdir2` ，其中包含一个文件 `file2.ext`。

寻找我们文件系统中文件的最长 (按字符的数量统计) 绝对路径。例如，在上述的第二个例子中，最长路径`dir/subdir2/subsubdir2/file2.ext`，其长度为 32 (不包含双引号)。

给定一个以上述格式表示文件系统的字符串，返回文件系统中文件的最长绝对路径的长度。 如果系统中没有文件，返回 0。

说明:

- 文件名至少存在一个 . 和一个扩展名。
- 目录或者子目录的名字不能包含 .。

要求时间复杂度为 O(n) ，其中 n 是输入字符串的大小。

请注意，如果存在路径 aaaaaaaaaaaaaaaaaaaaa/sth.png 的话，那么  a/aa/aaa/file1.txt 就不是一个最长的路径。

### 思路

```
1. 通过 .spilt('\n')进行字符串划分
2. 计算 \t 的个数，分情况讨论

#第一种情况
(1)如果\t比stack的数量小或者相等，则表示后面的文件是在上层目录中，则循环把stack提取出来。
(2)比\t 比stack的数量大，则计算长度，存入栈中

3. 如果存在.的话，计算长度与最大长度进行比较。
```

### 代码

```java
class Solution {
    public int lengthLongestPath(String input) {
        Stack<Integer> stack = new Stack<>();
        stack.push(0);
        String[] str = input.split("\n");
        int ans = 0;
        for(String s : str){
            
            int level = s.lastIndexOf("\t") + 1;
            while(level + 1 < stack.size()){
                stack.pop();
            }

            int len = stack.peek() + (s.length() - level + 1);
            stack.push(len);
            if (s.contains(".")) {
                ans = Math.max(ans, len - 1);
            }
        }
        return ans;
    }
}
```

![image-20201018173706513](C:\Users\Auraros\AppData\Roaming\Typora\typora-user-images\image-20201018173706513.png)

## 394 [字符串解码](https://leetcode-cn.com/problems/decode-string/)

给定一个经过编码的字符串，返回它解码后的字符串。

编码规则为: `k[encoded_string]`，表示其中方括号内部的 encoded_string 正好重复 k 次。注意 k 保证为正整数。

你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。

此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 k ，例如不会出现像 `3a `或` 2[4] `的输入。

示例 1：

```
输入：s = "3[a]2[bc]"
输出："aaabcbc"
```


示例 2：

```
输入：s = "3[a2[c]]"
输出："accaccacc"
```


示例 3：

```
输入：s = "2[abc]3[cd]ef"
输出："abcabccdcdcdef"
```


示例 4：

```
输入：s = "abc3[cd]xyz"
输出："abccdcdcdxyz"
```

### 思路一

```
时间 O（N）
空间 O（N）
建立两个栈
1. 一个数字栈，存放需要复制的次数
2. 一个字符串栈， 存放需要加上的字符串 比如 2[a2[b]] 这个栈会存a

然后分条件遍历：
1. 如果是 [  的话，就把数字存入栈中，字符也存入栈中，然后初始化数字和字符

2. 如果是 ] 的话，就复制字符串再加上字符栈栈底的字符

3. 如果是 数字 的话，就前一个数字 * 10 +现在的数字

4. 如果是字符，就字符累加
```

### 代码

```java
class Solution {
    public String decodeString(String s) {
        Stack<String> stack_str = new Stack<>();
        Stack<Integer> stack_int = new Stack<>();
        StringBuilder res = new StringBuilder();
        int number = 0;
        for (Character str : s.toCharArray()){
            if(str == '['){
                stack_int.push(number);
                stack_str.push(res.toString());
                number = 0;
                res = new StringBuilder();
            }
            else if (str == ']'){
                StringBuilder tmp = new StringBuilder();
                int repeat_int = stack_int.pop();
//              String repeat_str = stack_str.pop();
                for(int i = 0 ; i < repeat_int; i++){
                    tmp.append(res);
                }
                res = new StringBuilder(stack_str.pop() + tmp);
            }
            else if (str >= '0' && str <= '9'){
                number = number * 10 + Integer.parseInt(str + "");
            }
            else{
                res.append(str);
            }
        }
        return res.toString();
    }
}
```

### 思路 2

```
时间 O（N）
空间 O（N）
总体思路和辅助栈方法一致，不同点在于将 [  和  ]  分别作为递归的开始和终止条件：

1. 当 字符为 ] 的时候，返回当前括号内记录的 res 字符串与 ] 的索引 i （更新上层递归指针位置）；

2. 当 字符为 [ 时，开启新的一层递归，记录 [] 内的字符串tmp和递归后新的索引，并执行 res + multi * tmp

3. 遍历完返回 res
```

### 代码

```java
s = "3[a]2[bc]"
class Solution {
    public String decodeString(String s) {
        return dfs(s, 0)[0];  //两个参数，一个是字符串，一个索引i
    }

    private String[] dfs(String s, int i){
        StringBuilder res = new StringBuilder();
        int multi = 0;
        while( i < s.length()){
            if(s.charAt(i) >= '0' && s.charAt(i) <= '9')
                multi = multi * 10 + Integer.parseInt(String.valueOf(s.charAt(i)));
            else if (s.charAt(i) == '['){
                String[] tmp = dfs(s, i+1);
                i = Integer.parseInt(tmp[0]);
                while(multi > 0){
                    res.append(tmp[1]);
                    multi--;
                }
            }
            else if(s.charAt(i) == ']')
                return new String[] {String.valueOf(i), res.toString()};
            else
                res.append(String.valueOf(s.charAt(i)));
            i++;
        }
        return new String[] {res.toString()};
    }
}
```

## 224. [基本计算器](https://leetcode-cn.com/problems/basic-calculator/)

实现一个基本的计算器来计算一个简单的字符串表达式的值。

字符串表达式可以包含左括号 ( ，右括号 )，加号 + ，减号 -，非负整数和空格  

示例 1:

```
输入: "1 + 1"
输出: 2
```


示例 2:

```
输入: " 2-1 + 2 "
输出: 3
```


示例 3:

```
输入: "(1+(4+5+2)-3)+(6+8)"
输出: 23
```


说明：

- 你可以假设所给定的表达式都是有效的。

- 请不要使用内置的库函数 eval。

### 思路一

```
自己思路是使用两个栈，一个存符号一个数字进行计算，但是考虑少了一个细节，就是减号并不能做到 (a - b) +c 与 a -(b+c)相等；所以代码更改一下：

1. 使用两个栈，stack_int用于存储操作数，stack_str用于存储操作符
2. 从左往右扫描，遇到操作数就入栈
3. 遇到操作符的时候，如果当前的优先级低于或者等于栈顶操作符优先级，则stack_int弹出两个元素，从stack1弹出一个操作符，进行计算，结果继续压入栈中，
4. 如果遇到高级的操作符，直接入栈
5. 遇到左括号就入栈
6. 遇到右括号，一直计算，直到遇到左括号。
```

### 代码

```java
class Solution {
    public int calculate(String s) {
    char[] array = s.toCharArray();
    int n = array.length;
    Stack<Integer> num = new Stack<>();
    Stack<Character> op = new Stack<>();
    int temp = -1;
    for (int i = 0; i < n; i++) {
        if (array[i] == ' ') {
            continue;
        }
        // 数字进行累加
        if (isNumber(array[i])) {
            if (temp == -1) {
                temp = array[i] - '0';
            } else {
                temp = temp * 10 + array[i] - '0';
            }
        } else {
            //将数字入栈
            if (temp != -1) {
                num.push(temp);
                temp = -1;
            }
            //遇到操作符
            if (isOperation(array[i] + "")) {
                while (!op.isEmpty()) {
                    if (op.peek() == '(') {
                        break;
                    }
                    //不停的出栈，进行运算，并将结果再次压入栈中
                    int num1 = num.pop();
                    int num2 = num.pop();
                    if (op.pop() == '+') {
                        num.push(num1 + num2);
                    } else {
                        num.push(num2 - num1);
                    }

                }
                //当前运算符入栈
                op.push(array[i]);
            } else {
                //遇到左括号，直接入栈
                if (array[i] == '(') {
                    op.push(array[i]);
                }
                //遇到右括号，不停的进行运算，直到遇到左括号
                if (array[i] == ')') {
                    while (op.peek() != '(') {
                        int num1 = num.pop();
                        int num2 = num.pop();
                        if (op.pop() == '+') {
                            num.push(num1 + num2);
                        } else {
                            num.push(num2 - num1);
                        }
                    }
                    op.pop();
                }

            }
        }
    }
    if (temp != -1) {
        num.push(temp);
    }
    //将栈中的其他元素继续运算
    while (!op.isEmpty()) {
        int num1 = num.pop();
        int num2 = num.pop();
        if (op.pop() == '+') {
            num.push(num1 + num2);
        } else {
            num.push(num2 - num1);
        }
    }
    return num.pop();
}

private boolean isNumber(char c) {
    return c >= '0' && c <= '9';
}

private boolean isOperation(String t) {
    return t.equals("+") || t.equals("-") || t.equals("*") || t.equals("/");
}

}
```

### 代码二递归

```java
class Solution {
    public static int calculate(String s) {
        return dfs(s, 0)[0];
    }

    public static int[] dfs(String s, int index) {
        Stack<Integer> stk = new Stack<>();
        char[] arr = s.toCharArray();
        char lastop = '+';

        for(int i = index; i < arr.length; i++){
            if(arr[i] == ' ') continue;


            if(Character.isDigit(arr[i])){
                int tempNum = 0;
                while(i < arr.length && Character.isDigit(arr[i])){
                    tempNum = tempNum * 10 + (arr[i] - '0');
                    i++;
                }i--;
                if (lastop == '+') stk.push(tempNum);
                else if(lastop == '-') stk.push(-tempNum);
                
            }else if(arr[i] == '+'|| arr[i] == '-'|| arr[i] == '*'|| arr[i] == '/'){
                lastop = arr[i];
            }else if(arr[i] == '('){
                int[] result = dfs(s, i+1);
                int tempNum = result[0];
                i = result[1];
                if (lastop == '+') stk.push(tempNum);
                else if(lastop == '-') stk.push(-tempNum);
            }else if (arr[i] == ')'){
                index = i;
                break;
            }
        }
        int res = 0;
        for(int number : stk) res += number;
        return new int[] {res, index};
    }
}
```

## 227 [ 基本计算器 II](https://leetcode-cn.com/problems/basic-calculator-ii/)

实现一个基本的计算器来计算一个简单的字符串表达式的值。

字符串表达式仅包含非负整数，+， - ，*，/ 四种运算符和空格  。 整数除法仅保留整数部分。

示例 1:

```
输入: "3+2*2"
输出: 7
```


示例 2:

```
输入: " 3/2 "
输出: 1
```


示例 3:

```
输入: " 3+5 / 2 "
输出: 5
```


说明：

你可以假设所给定的表达式都是有效的。
请不要使用内置的库函数 eval。

### 代码

```
class Solution {
    
 public int calculate(String s) {
        Stack<Integer> numStack = new Stack<>();

        char lastOp = '+';
        char[] arr = s.toCharArray();
        for(int i = 0; i < arr.length; i ++){
            if(arr[i] == ' ') continue;

            if(Character.isDigit(arr[i])){
                int tempNum = arr[i] - '0';
                while(++i < arr.length && Character.isDigit(arr[i])){
                    tempNum = tempNum * 10 + (arr[i] - '0');
                } i--;

                if(lastOp == '+') numStack.push(tempNum);
                else if(lastOp == '-') numStack.push(-tempNum);
                else numStack.push(res(lastOp, numStack.pop(), tempNum));
            } else lastOp = arr[i];
        }

        int ans = 0;
        for(int num : numStack) ans += num;
        return ans;
    }
    
    private int res(char op, int a, int b){
        if(op == '*') return a * b;
        else if(op == '/') return a / b;
        else if(op == '+') return a + b; //其实加减运算可以忽略
        else return a - b;
    }

}
```

![image-20201019231411006](C:\Users\Auraros\AppData\Roaming\Typora\typora-user-images\image-20201019231411006.png)

## 总结一下：计算器的最终实现

我们最终要实现的计算器功能如下：

1. 输入与一个字符串，可以包含 + -  * /、数字、空格以及括号，算法返回结果。
2. 要符合运算规则，括号的优先级最高，先乘除然后加减。
3. 除号是整数除法，无论正负都向0取整（5/2=2， -5/2=-2）。
4. 可以假定输入的算式一定合法，且计算过程不会出现整型溢出，不会出现除数为 0 的意外情况。

比如输入如下字符串，会返回 9 ：
$$
3 * (2-6 /(3 -7))
$$

### 字符串化整数

```java
String s = "458";
char[] arr = s.toCharArray();
int n = 0;
for (int i = 0; i < arr.length; i++) {
    if(Character.isDigit(arr[i])){
    	char c = s[i];
   	 	n = 10 * n + (c - '0');
    }
}
```

### 处理加减号

第二步，如果输入的算式只包含了加减法，而且不存在空格，那怎么计算。
$$
1-12+3
$$

- 先给第一个数组加一个默认符号 + ， 变成了 +1 -12 + 3
- 把一个运算符和数组组合成一对儿，也就是三个 +1， -12， +3 ,把它们转化为数字，然后放到一个栈中。
- 将栈中的数字所有求和

```java
public int calculate(String s){
	Stack<Integer> stk = new Stack<>();
    char[] arr = s.toCharArray();
    char lastop = '+';

    for(int i = 0; i < arr.length; i++){
        if(arr[i] == ' ') continue;
        if(Character.isDigit(arr[i])){
            int tempNum = 0;
            while(i < arr.length && Character.isDigit(arr[i])){
                tempNum = tempNum * 10 + (arr[i] - '0');
                i++;
            }i--;
            if (lastop == '+') stk.push(tempNum);
            else stk.push(-tempNum);
        }else lastop = arr[i];
    }
    int res = 0;
    for(int number : stk) res += number;
    return res; 
}
```

### 处理乘除法

思路和处理加减法没什么区别：
$$
2-3*4/5+5
$$
可以分解为 +2， -3， *4， /5， +5

然后只需要在处理加减号那里在添加乘除的处理即可

```java
    public static int calculate(String s) {
        Stack<Integer> stk = new Stack<>();
        char[] arr = s.toCharArray();
        char lastop = '+';

        for(int i = 0; i < arr.length; i++){
            if(arr[i] == ' ') continue;



            if(Character.isDigit(arr[i])){
                int tempNum = 0;
                while(i < arr.length && Character.isDigit(arr[i])){
                    tempNum = tempNum * 10 + (arr[i] - '0');
                    i++;
                }i--;
                if (lastop == '+') stk.push(tempNum);
                else if(lastop == '-') stk.push(-tempNum);
                else stk.push(res(lastop, stk.pop(), tempNum));
            }else lastop = arr[i];
        }
        int res = 0;
        for(int number : stk) res += number;
        return res;
    }

    public static int res(char op, int a, int b){
        if(op == '*') return a * b;
        else if(op == '/') return a / b;
        else if(op == '+') return a + b; //其实加减运算可以忽略
        else return a - b;
   }
}
```

### 处理括号

递归的话实际并不难，看看下面例子：
$$
calculate(3*(4-5/2)-6)
$$

$$
= 3 * calculate(4-5/2)-6
$$

$$
= 3 * 2 - 6
$$

使用递归即可：

- 递归开始条件为 ` (`

- 递归结束条件为  `)`

```java
    public static int calculate(String s) {
        return dfs(s, 0)[0];
    }

    public static int[] dfs(String s, int index) {
        Stack<Integer> stk = new Stack<>();
        char[] arr = s.toCharArray();
        char lastop = '+';

        for(int i = index; i < arr.length; i++){
            if(arr[i] == ' ') continue;


            if(Character.isDigit(arr[i])){
                int tempNum = 0;
                while(i < arr.length && Character.isDigit(arr[i])){
                    tempNum = tempNum * 10 + (arr[i] - '0');
                    i++;
                }i--;
                if (lastop == '+') stk.push(tempNum);
                else if(lastop == '-') stk.push(-tempNum);
                else stk.push(res(lastop, stk.pop(), tempNum));
            }else if(arr[i] == '+'|| arr[i] == '-'|| arr[i] == '*'|| arr[i] == '/'){
                lastop = arr[i];
            }else if(arr[i] == '('){
                int[] result = dfs(s, i+1);
                int tempNum = result[0];
                i = result[1];
                if (lastop == '+') stk.push(tempNum);
                else if(lastop == '-') stk.push(-tempNum);
                else stk.push(res(lastop, stk.pop(), tempNum));
            }else if (arr[i] == ')'){
                index = i;
                break;
            }
        }
        int res = 0;
        for(int number : stk) res += number;
        return new int[] {res, index};
    }



    public static int res(char op, int a, int b){
        if(op == '*') return a * b;
        else if(op == '/') return a / b;
        else if(op == '+') return a + b; //其实加减运算可以忽略
        else return a - b;
}
```

## 385 [迷你语法分析器](https://leetcode-cn.com/problems/mini-parser/)

给定一个用字符串表示的整数的嵌套列表，实现一个解析它的语法分析器。

列表中的每个元素只可能是整数或整数嵌套列表

提示：你可以假定这些字符串都是格式良好的：

```
字符串非空
字符串不包含空格
字符串只包含数字0-9、[、-、,、]
```

示例 1：

2. ```
   给定 s = "324",
   
   你应该返回一个 NestedInteger 对象，其中只包含整数值 324。
   示例 2：
   
   给定 s = "[123,[456,[789]]]",
   
   返回一个 NestedInteger 对象包含一个有两个元素的嵌套列表：
   
   1. 一个 integer 包含值 123
   2. 一个包含两个元素的嵌套列表：
      i.  一个 integer 包含值 456
      ii. 一个包含一个元素的嵌套列表
           a. 一个 integer 包含值 789
   ```



### 思路

```
设定一个getNest()函数用于返回一个列表类型的NestedInteger。
```

### 代码

```java
class Solution {
    char[] chars;
    int cur = 0;
    public NestedInteger deserialize(String s) {
        chars = s.toCharArray();
        if(chars[0] != '[') return new NestedInteger(Integer.valueOf(s));
        return getNest();
    }

    public NestedInteger getNest(){
        NestedInteger nest = new NestedInteger();
        int num = 0;
        int sign = 1;
        while(cur != chars.length -1){
            cur++;
            if(chars[cur]==',') continue;
            if(chars[cur]=='[') nest.add(getNest());
            else if (chars[cur] == ']') return nest;
            else if(chars[cur] == '-') sign = -1;
            else{
                num = 10*num + sign * (chars[cur]-'0');
                if(chars[cur+1]==','|| chars[cur+1]==']'){
                    nest.add(new NestedInteger(num));
                    num = 0;
                    sign = 1;
                }
            }
        }
        return null;
    }
}
```

![image-20201019235647512](C:\Users\Auraros\AppData\Roaming\Typora\typora-user-images\image-20201019235647512.png)

## 84 [ 柱状图中最大的矩形](https://leetcode-cn.com/problems/largest-rectangle-in-histogram/)

给定 n 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。

求在该柱状图中，能够勾勒出来的矩形的最大面积。

 ![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/histogram.png)



以上是柱状图的示例，其中每个柱子的宽度为 1，给定的高度为 [2,1,5,6,2,3]。

 

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/10/12/histogram_area.png)

图中阴影部分为所能勾勒出的最大矩形面积，其面积为 10 个单位。

 

示例:

```
输入: [2,1,5,6,2,3]
输出: 10
```

### 思路

```
有一个方法是可以实现的：就是我们从左往右数组进行遍历，借助单调栈求出了没根主子的左边界，随后从右往左对数组进行遍历，求出右边界。但这样太过麻烦，有一个之遍历一遍的方法。
在方法一中，我们在对位置 i 进行入栈操作时，确定了它的左边界。从直觉上来说，与之对应的我们在对位置 i 进行出栈操作时可以确定它的右边界。
```

### 代码

```java
class Solution {
    public int largestRectangleArea(int[] heights) {

        int n = heights.length;
        int[] left = new int[n];
        int[] right = new int[n];
        Arrays.fill(right, n);

        if (n == 0) {
            return 0;
        }
        if (n == 1) {
            return heights[0];
        }

        Stack<Integer> mono_stack = new Stack<>();
        for (int i = 0; i < n; i++){
            while(!mono_stack.isEmpty() && heights[mono_stack.peek()] >= heights[i]){
                right[mono_stack.peek()] = i;
                mono_stack.pop();
            }
            left[i] = (mono_stack.isEmpty()?-1:mono_stack.peek());
            mono_stack.push(i);
        }
        mono_stack.clear();
        int ans = 0;
        for (int i = 0; i < n; i++){
            ans = Math.max(ans, (right[i] - left[i] - 1) * heights[i]);
        }
        return ans;
    }
}
```

![image-20201020165709679](C:\Users\Auraros\AppData\Roaming\Typora\typora-user-images\image-20201020165709679.png)

## 单调栈相关题目

