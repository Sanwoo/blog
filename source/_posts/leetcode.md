---
title: leetcode
date: 2024-03-15 21:03:14
tags:
---

# 算法技巧

> 已排序数组去重时通常判断arr[i] === arr[i - 1]更有效

# leetcode-1 两数之和

> ![image-20220607110341092](images/image-20220607110341092.png)
>
> [1. 两数之和 - 力扣（LeetCode）](https://leetcode.cn/problems/two-sum/)
>
> > ```javascript
> > /**
> >  * @param {number[]} nums
> >  * @param {number} target
> >  * @return {number[]}
> >  */
> > var twoSum = function(nums, target) {
> >   let map = new Map();
> >   //new Map()的M必须要大写，否则报错
> >   for(let i = 0 ; i < nums.length ; i++){
> >       //map并不是一个数组，所以这里是nums.length而不是map.length
> >       let x = target - nums[i]
> >       if(map.has(x)){
> >           return [map.get(x),i]
> >       }
> >       map.set(nums[i],i)
> >       //通过for循环搭配map.set将nums数组的值和下标写入map中，for循环中的i就是下标
> > 
> >       //用和target减去当前i(即下标)所对应的数组中的值，得到另外一个值x，再用map.has判断另外一个值x是否存在于map中(也是判断是否存在于nums数组中)，如果存在则return当前值的下标i和另外一个值x的下标(此处已将数组的值和下标以keyvalue对的形式写入map，所以map.get(x)获取的就是x的下标)
> >       //map.set(nums[i],i)放在末尾而不是for循环的开头原因是如果出现例如target=6而nums[0]和x都=3的情况，放在开头的set已经写入了keyvalue对以至于map.get(x)=0，这样返回结果为[0,0]不符合题目要求。将map.set(nums[i],i)放在末尾先判断后写入可以避免出现x和nums[i]都为target值的一半，map.get(x)返回值为当前i的值的情况
> >   }
> > };
> > ```

# leetcode-9 回文数

> ![image-20220607110826917](images/image-20220607110826917.png)
>
> [9. 回文数 - 力扣（LeetCode）](https://leetcode.cn/problems/palindrome-number/)
>
> > ```javascript
> > /**
> >  * @param {number} x
> >  * @return {boolean}
> >  */
> > var isPalindrome = function(x) {
> > if(x < 0 || (!(x % 10) && x)) return false;
> > //首先判断x是否为负数，10的倍数，这两者都不是回文数return false，其中0是回文数
> >     let x2 = x, res = 0;
> >     while(x2){
> >         res = res * 10 + x2 % 10;
> >         x2 = ~~(x2 / 10);
> >     }
> > //循环将x2从个位数开始的每一位数都翻转(个位数变成res的最高位，十位数变成第二高位数，以此类推)
> >     return res === x;
> > //判断翻转后的res和x是否全等
> > };
> > ```

# leetcode-13 罗马数字转整数

> ![image-20220607110947755](images/image-20220607110947755.png)
>
> ![image-20220607111008146](images/image-20220607111008146.png)
>
> [13. 罗马数字转整数 - 力扣（LeetCode）](https://leetcode.cn/problems/roman-to-integer/)
>
> > ```javascript
> > /**
> >  * @param {string} s
> >  * @return {number}
> >  */
> > var romanToInt = function(s) {
> > const symbolValues = new Map();
> >     symbolValues.set('I', 1);
> >     symbolValues.set('V', 5);
> >     symbolValues.set('X', 10);
> >     symbolValues.set('L', 50);
> >     symbolValues.set('C', 100);
> >     symbolValues.set('D', 500);
> >     symbolValues.set('M', 1000);  
> > //将单个罗马数字字符和其对应的值作为一个keyvalue对写入一个map中    
> >     let ans = 0;
> >     const n = s.length;
> >     for (let i = 0; i < n; ++i) {
> > //for循环从最高位到最低位
> >         const value = symbolValues.get(s[i]);
> > //把每一位的罗马字符对应的数值赋给value变量
> >         if (i < n - 1 && value < symbolValues.get(s[i + 1])) {
> >             ans -= value;
> >         } else {
> >             ans += value;
> >         }
> > //判断当前的i是否为最低位，不是的话再判断当前的i对应的罗马字符的值比下一个i的罗马字符的值大还是小，小的话就是IV、IX、XL......这样的特殊情况，需要减去当前i的罗马字符的值，大的话就是正常情况，需要加上当前i的罗马字符的值        
> >     }
> >     return ans;
> > };
> > ```

# leetcode-14 最长公共前缀

> ![image-20220607111125660](images/image-20220607111125660.png)
>
> [14. 最长公共前缀 - 力扣（LeetCode）](https://leetcode.cn/problems/longest-common-prefix/)
>
> > ```javascript
> > /**
> >  * @param {string[]} strs
> >  * @return {string}
> >  */
> > var longestCommonPrefix = function(strs) {
> >   if(strs.length == 0){
> >       return ""             //判断strs数组长度是否为0，为0则公共前缀为""
> >   }
> >   let ans = strs[0];        
> > //将strs数组的第一个字符串赋值给ans，这样既可以用数组中其他字符串与ans做对比，也可以在strs数组只有一个字符串时直接输出这个字符串ans为公共前缀
> >   for(let i = 1; i < strs.length; i++){
> > //for循环对strs数组中除了第一个字符串进行遍历
> >       let j = 0
> >       for(;j < ans.length && j < strs[i].length; j++){
> > //for循环对字符串遍历
> >           if(ans[j] != strs[i][j]){
> > //判断第几个字符开始该字符串和ans出现不同并break，此时的j已经变成了第一个不同的那个字符的索引
> >               break;
> >           }
> >       }
> >       ans = ans.substr(0,j)
> > //将索引j用作substr的length参数，j作为索引是公共前缀的最后一个字符索引+1，刚好在substr中从0开始时的length为目标字符索引+1     
> >   }
> >   return ans
> > };
> > ```

# leetcode-20 有效的括号

> ![image-20220609135624772](images/image-20220609135624772.png)
>
> [20. 有效的括号 - 力扣（LeetCode）](https://leetcode.cn/problems/valid-parentheses/)
>
> > 栈解法
> >
> > ```js
> > /**
> >  * @param {string} s
> >  * @return {boolean}
> >  */
> > var isValid = function(s) {
> >     const map = new Map()
> >     const stack = []
> >     map.set('(',')')
> >     map.set('{','}')
> >     map.set('[',']')
> >     for(const i of s){
> >         if(map.has(i)){
> >             stack.push(map.get(i))
> >             continue
> >         }
> >         if(stack.pop() !== i){
> >             return false
> >         }
> >     }
> >     return !stack.length
> > };
> > ```
> >
> > 
> >
> > ```javascript
> > /**
> >  * @param {string} s
> >  * @return {boolean}
> >  */
> > var isValid = function(s) {
> >   let stack = [], length = s.length;
> >     if(length % 2) return false;
> >     //首先判断s长度奇偶性，奇数直接false
> >     for(let item of s){
> >         switch(item){
> >             case "{":
> >             case "[":
> >             case "(":
> >                 stack.push(item);
> >                 break;
> >             case "}":
> >                 if(stack.pop() !== "{") return false;
> >                 break;
> >             case "]":
> >                 if(stack.pop() !== "[") return false;
> >                 break;
> >             case ")":
> >                 if(stack.pop() !== "(") return false;
> >                 break;
> >         }
> >     }
> >     //for循环内嵌switch对s中的每一个符号进行条件判断，对于左符号将其push进stack数组，对于右符号先进行一次pop，并用pop返回值判断stack的最后一个符号是否与当前右符号匹配，不匹配则false。
> >     return !stack.length;
> >     //因为每一个左符号都push，每一个右符号都pop(不管if是否成立，都执行了pop这个过程)，所以这一条判断左符号和右符号数量是否相等(数量相等则stack.length必等于0则!stack.length返回true，反之返回false)
> > };
> > ```

# leetcode-26 删除有序数组中的重复项

> ![image-20220609132930055](images/image-20220609132930055.png)
>
> [26. 删除有序数组中的重复项 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/)
>
> > 官方解法
> >
> > ```javascript
> > /**
> >  * @param {number[]} nums
> >  * @return {number}
> >  */
> > var removeDuplicates = function(nums) {
> >    const n = nums.length;
> >    //将nums的长度赋值给n    
> >    if (n === 0) {
> >         return 0;
> >     }
> >    //判断n是否等于0，等于0直接返回0
> >     let fast = 1, slow = 1;
> >     //定义一个fast和一个slow用作双指针
> >     while (fast < n) {
> >         if (nums[fast] !== nums[fast - 1]) {
> >             nums[slow] = nums[fast];
> >             slow++;
> >         }
> >         fast++;
> >     }
> >     //快指针循环每次加一直到等于n，循环期间每次循环判断当前快指针指向值是否等于前一个值，不等于则把当前快指针指向的值赋给当前慢指针指向的值并让慢指针加一
> >     return slow;
> > };
> > ```
> >
> > 双指针
> >
> > > ```javascript
> > > /**
> > >  * @param {number[]} nums
> > >  * @return {number}
> > >  */
> > > var removeDuplicates = function(nums) {
> > >     let fast = 0, slow = 0
> > >     for(; fast < nums.length; fast++){
> > >         if(nums[fast] !== nums[slow]){
> > >             nums[++slow] = nums[fast]
> > >             //这里要++slow而不能slow++
> > >         }
> > >     }
> > >     return slow+1
> > > };
> > > ```
> >
> > > ```javascript
> > > /**
> > >  * @param {number[]} nums
> > >  * @return {number}
> > >  */
> > > var removeDuplicates = function(nums) {
> > >     let fast = 0, slow = 0
> > >     for(; fast < nums.length; fast++){
> > >         if(nums[fast] !== nums[fast + 1]){
> > >             nums[++slow] = nums[fast + 1]
> > >         }
> > >     }
> > >     return slow
> > > };
> > > ```
> > >
> > > ==这两种版本的差别主要是nums[nums.length]的结果是undefined !\== nums[fast]，这一个边界问题导致了return的slow要不要+1，如果是判断区间没有触及到nums[nums.length]的话则需要将slow+1，反之则不需要+1==

# leetcode-27 移除元素

> ![image-20220609135346412](images/image-20220609135346412.png)
>
> [27. 移除元素 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-element/)
>
> > 双指针
> >
> > ```JavaScript
> > /**
> >  * @param {number[]} nums
> >  * @param {number} val
> >  * @return {number}
> >  */
> > var removeElement = function(nums, val) {
> >     let fast = 0, slow = 0
> >     for(; fast < nums.length; fast++){
> >         if(nums[fast] !== val){
> >             nums[slow++] = nums[fast]
> >         }
> >     }
> >     return slow
> > };
> > ```
> > 

# leetcode-28 实现strStr()

> ![image-20220614144812045](images/image-20220614144812045.png)
>
> [28. 实现 strStr() - 力扣（LeetCode）](https://leetcode.cn/problems/implement-strstr/)
>
> > ```javascript
> > /**
> >  * @param {string} haystack
> >  * @param {string} needle
> >  * @return {number}
> >  */
> > var strStr = function(haystack, needle) {
> >   let m = haystack.length
> >   let n = needle.length
> >   //获取haystack和needle的长度
> >   if(n == 0){
> >     return 0
> >   }
> >   //当needle为空时，返回0
> >   for(let i = 0; i + n <= m; i++){
> >     let flag = true;
> >     for(let j = 0; j < n; j++){
> >         if(haystack[i+j] !== needle[j]){
> >             flag = false
> >             break
> >         }
> >     }
> >     if(flag){
> >         return i
> >     }
> >   }
> >   //嵌套循环，i是指haystack的索引，i+needle的长度不能超过haystack的长度，通过当前i的值作为索引找到haystack中对应的字符，从这个字符开始第二层循环，第二层循环判断从当前i对应的字符开始的后续字符是否与needle完全一致。这里定义了flag来作为判断haystack中是否有needle的依据，每次第一层循环开始时初始化为true，第二层循环判断当前i对应的字符开头没有needle时将flag=false，通过if(flag)来确定是否返回return i
> >   return -1
> > };
> > ```
> >
> > kmp解法
> >
> > [帮你把KMP算法学个通透！（理论篇）_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1PD4y1o7nd?spm_id_from=333.337.search-card.all.click&vd_source=9de430adabffb3f1879c1eb17ba0f461)
> >
> > [帮你把KMP算法学个通透！（求next数组代码篇）_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1M5411j7Xx?vd_source=9de430adabffb3f1879c1eb17ba0f461)
> >
> > [【neko】KMP算法【算法编程#7】_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1234y1y7pm?spm_id_from=333.337.search-card.all.click&vd_source=9de430adabffb3f1879c1eb17ba0f461)
> >
> > 一下讲解基于next数组统一不减一且不右移的情况
> >
> > 整体思路是构建模式串needle的前缀表next，next中分别对应每个needle字符的最长相等前后缀长度，当匹配到不相同的字符时，找到这个字符在next对应的数字，再将指针=这个数字即可(==不匹配字符之前的字符串是否存在最长相等前后缀呢，如果存在那么长度是多少呢，这里假设最长相等前后缀长度为n，那么存在相等前后缀也可以看作是不匹配字符之前的字符串存在两个完全相同的长度为n的子串，又因为当前字符不匹配还有另外一层含义那就是之前的字符都匹配，也就是说文本串指针之前的n个字符组成的字符串和模式串的前n个字符组成的字符串完全相同，既然完全相同那么就不必像暴力解法一样从头开始匹配，可以跳过这些完全相同的子串，那么匹配到不相同的字符后文本串的指针不变，模式串的指针找到当前字符对应的最长相等前后缀长度并指向它，因为索引从0开始那么模式串指针就指向了模式串的第n+1个字符，刚好和文本串的指针对应上，直接进行接下来的匹配==)，这样直到模式串指针指向模式串末尾且指向末尾时末尾字符和同时的文本串指针指向的字符相同时，匹配成功，这时返回文本串中模式串的头字符索引即可(文本串指针减去模式串长度再加一)
> >
> > ![KMP精讲1](images/KMP%E7%B2%BE%E8%AE%B21.gif)
> >
> > 接下来是next数组的构建，next数组构建需要对模式串进行自匹配，模式串上的两个指针prefix指向前缀末尾(==prefix不止代表前缀末尾，还代表了相等前后缀长度==)，suffix指向后缀末尾，这里相当于将模式串分割为两部分进行自匹配，自匹配的模式串为prefix之前的字符串，prefix之后到suffix之前的字符串作为自匹配的文本串。那么显而易见next[0]=0(==模式串头字符的之前的字符串为空，自然没有相等前后缀==)。那么初始化prefix=0，suffix=1。从suffix=1到模式串末尾进行遍历，如果两者指向的字符相同，那么prefix++再push即可，如果不相同(==也就是不匹配==)那么prefix=next[prefix - 1]，这样回溯去重新找直到回溯到头或者找到匹配上的字符，此时再prefix++然后push
> >
> > ```javascript
> > /**
> >  * @param {string} haystack
> >  * @param {string} needle
> >  * @return {number}
> >  */
> > var strStr = function(haystack, needle) {
> >     const getNext = (needle) => {
> >         //前缀表next统一不减一
> >         //构建next数组的过程是一次模式串的自匹配，将prefix之前的字符串作为模式串，prefix之后到suffix结尾的字符串作为文本串，有点递归的意思在里面
> >         const next =[]
> >         let prefix = 0, suffix = 1
> >         //初始化前缀末尾和当前最长相等前后缀长度prefix = 0，后缀末尾suffix = 1
> >         next.push(prefix)
> >         //needle[0]子串的最长相等前后缀必为0
> >         for(; suffix < needle.length; suffix++){
> >             while(prefix > 0 && needle[prefix] !== needle[suffix]){
> >                 prefix = next[prefix - 1]
> >             }
> >             if(needle[prefix] === needle[suffix]){
> >                 prefix++
> >             }
> >             next.push(prefix)
> >         }
> >         return next
> >     }
> >     let next = getNext(needle)
> >     let m = 0, n = 0
> >     //初始化文本串haystack指针m=0，模式串needle指针n=0
> >     for(;m < haystack.length; m++){
> >         while(n > 0 && needle[n] !== haystack[m]){
> >             n = next[n - 1]
> >         }
> >         if(needle[n] === haystack[m]){
> >             n++
> >         }
> >         while(n === needle.length){
> >             return m - needle.length + 1
> >         }
> >     }
> >     return -1
> > }; 
> > ```

# ==leetcode-35 搜索插入位置==

> ![image-20220614190435168](images/image-20220614190435168.png)
>
> [35. 搜索插入位置 - 力扣（LeetCode）](https://leetcode.cn/problems/search-insert-position/)
>
> > ```javascript
> > /**
> >  * @param {number[]} nums
> >  * @param {number} target
> >  * @return {number}
> >  */
> > var searchInsert = function(nums, target) {
> >     let left = 0, right = nums.length - 1
> >     while(left <= right){
> >         let mid = left + Math.floor((right- left) / 2)
> >         if(nums[mid] > target){
> >             right = mid - 1
> >         }else if(nums[mid] < target){
> >             left = mid + 1
> >         }else{
> >             return mid
> >         }
> >     }
> >     return left
> > };
> > //二分法，返回left的原因是原数组是一个升序数组，寻找插入target的位置或者等于target的位置，如果是等于target的位置，那么最终的left=right，返回left和right都可，如果是插入target的位置，那么最终的left=right+1(此处要细细体会，最好写个具体的例子走一遍流程)，因为升序插入位置，所以应该是结束循环的right+1即left
> > ```

# leetcode-53 最大子数组和

> ![image-20220618143610108](images/image-20220618143610108.png)
>
> [53. 最大子数组和 - 力扣（LeetCode）](https://leetcode.cn/problems/maximum-subarray/)
>
> > 动态规划/贪心算法
> >
> > ```javascript
> > /**
> >  * @param {number[]} nums
> >  * @return {number}
> >  */
> > var maxSubArray = function(nums) {
> >     let pre = 0, maxAns = nums[0];
> >     nums.forEach((x) => {
> >         pre = Math.max(pre + x, x);
> >         maxAns = Math.max(maxAns, pre);
> >     });
> >     return maxAns;
> > };
> > //对于每一个i，都有数个以i结尾的连续子数组，将这些数组的数组和中的最大值作为f(i)，再从i个f(i)中取最大值即为所求返回值。
> > 
> > //求f(i)则需要从nums[0]开始，到i为止，每一个nums[i]都和nums[0]到nums[i]的和进行Math.max取最大值(此处就是贪心算法，只要nums[0]到nums[i]的和为负数，那么Math.max取值就为nums[i])，并将该最大值赋给pre，那么pre就总是代表当前i之前的f(i-1)——以i-1结尾的数个连续子数组值和的最大值，类似于滚动数组，那么Math.max取最大值一直滚动到i时，就可以将f(i)求出来。将maxAns初始化为nums[0]，每个i都有最大pre，那么每个i的最大pre中的最大pre即为maxAns，那么只需要每次求出新的pre时将它与maxAns进行Math.max然后赋给maxAns即可。
> > ```
> >
> > 分治法——见该题官方解答方法二

# leetcode-58 最后一个单词的长度

> ![image-20220618192229206](images/image-20220618192229206.png)
>
> [58. 最后一个单词的长度 - 力扣（LeetCode）](https://leetcode.cn/problems/length-of-last-word/)
>
> > ```javascript
> > /**
> >  * @param {string} s
> >  * @return {number}
> >  */
> > var lengthOfLastWord = function(s) {
> >     let index = s.length - 1;
> >     let wordlength = 0;
> >     while(s[index] == ' '){
> >         index--
> >     }
> >     while(s[index] !== ' ' && index >= 0){
> >         wordlength++
> >         index--
> >     } 
> >     return wordlength
> > };
> > //反向遍历法，从字符串末尾开始遍历，先判断末尾是否有空格，有的话将index减小直到不是空格，在index所对应的字符不是空格后，再对wordlength进行++
> > ```

# leetcode-61 加一

> ![image-20220619221351930](images/image-20220619221351930.png)
>
> [66. 加一 - 力扣（LeetCode）](https://leetcode.cn/problems/plus-one/)
>
> > ```javascript
> > /**
> >  * @param {number[]} digits
> >  * @return {number[]}
> >  */
> > var plusOne = function(digits) {
> >     if(digits[0] == 0){
> >         digits[0] = digits[0] + 1
> >         return digits
> >     }
> >     let nums = 0
> >     for(var i = digits.length - 1; i >= 0; i--){
> >         if(digits[i] == 9){
> >              nums++
> >         }else{
> >             break
> >         }
> >     }
> >     if(nums == 0){
> >         digits[digits.length - 1]++
> >     }else if(nums == digits.length){
> >         digits.forEach((element,index) => {digits[index] = 0})
> >         digits.unshift('1')
> >     }else{
> >         while(digits.lastIndexOf(9) > i){
> >             digits[digits.lastIndexOf(9)] = 0
> >         }
> >         digits[i]++
> >     }
> >     return digits
> > };
> > //特例：当输入数组为[0]时，返回[1]。其余情况则判断从digits[digits.length - 1]开始，反向数有几个连续的9，总共有三种情况，分别是只有一个9，整个数组全是9、有n个9(1<n<digits.length)，然后分别对数组做出对应的操作
> > ```
> >
> > 官方解答
> >
> > ```javascript
> > var plusOne = function(digits) {
> >     const n = digits.length;
> >     for (let i = n - 1; i >= 0; --i) {
> >         if (digits[i] !== 9) {
> >             ++digits[i];
> >             for (let j = i + 1; j < n; ++j) {
> >                 digits[j] = 0;
> >             }
> >             return digits;
> >         }
> >     }
> > 
> >     // digits 中所有的元素均为 9
> >     const ans = new Array(n + 1).fill(0);
> >     ans[0] = 1;
> >     return ans;
> > };
> > ```

# leetcode-67 二进制求和

> ![image-20220625102531872](images/image-20220625102531872.png)
>
> [67. 二进制求和 - 力扣（LeetCode）](https://leetcode.cn/problems/add-binary/)
>
> > ```javascript
> > /**
> >  * @param {string} a
> >  * @param {string} b
> >  * @return {string}
> >  */
> > var addBinary = function(a, b) {
> >     //len为最长的a b
> >     let len = Math.max(a.length, b.length);
> >     //在前面补0使得等长
> >     a = a.padStart(len, 0).split('');
> >     b = b.padStart(len, 0).split('');
> >     let res = [];
> >     //进位
> >     let isten = 0;
> >     //从后向前计算
> >     for(let i = len - 1; i >= 0; i--){
> >         //将字符串变为number类型
> >         a[i] = parseInt(a[i]);
> >         b[i] = parseInt(b[i]);
> >         if(a[i] + b[i] + isten == 3){
> >             res.unshift(1);
> >             isten = 1;
> >         }else if(a[i] + b[i] + isten == 2){
> >             res.unshift(0);
> >             isten = 1;
> >         }else if(a[i] + b[i] + isten == 1){
> >             res.unshift(1);
> >             isten = 0;
> >         }else{
> >             res.unshift(0);
> >             isten = 0;
> >         }
> >     }
> >     //最后一个进位是1
> >     if(isten == 1)res.unshift(1);
> >     return res.join('');
> > };
> > ```

# leetcode-69 x的平方根

> ![image-20220625140919252](images/image-20220625140919252.png)
>
> [69. x 的平方根 - 力扣（LeetCode）](https://leetcode.cn/problems/sqrtx/)
>
> > 暴力解法
> >
> > ```javascript
> > /**
> >  * @param {number} x
> >  * @return {number}
> >  */
> > var mySqrt = function(x) {
> >     if(x == 1 || x == 0){
> >         return x
> >     }
> >     let m = x / 2
> >     for(let i = 1; i <= m; i++){
> >         if(i * i == x || (i + 1)*(i + 1) > x){
> >             return i
> >         }
> >     }
> > };
> > //暴力解法
> > ```
> >
> > 二分法
> >
> > ```javascript
> > /**
> >  * @param {number} x
> >  * @return {number}
> >  */
> > var mySqrt = function(x) {
> >     let left = 0, right = x
> >     while(left <= right){
> >         let mid = left + ((right - left) >> 1)
> >         if(mid * mid > x){
> >             right = mid - 1
> >         }else if(mid * mid < x){
> >             left = mid + 1
> >         }else return mid
> >     }
> >     return right
> >     // 此处与35题目相似，如果能找到整数平方根，那么此时left=right返回哪个都可以，如果是有小数的平方根，那么最后的left=right+1，根据题目要求要去掉小数那么久返回right，思路正好和35题相反
> > };
> > ```

# leetcode-88 合并两个有序数组

> ![image-20220704141428811](images/image-20220704141428811.png)
>
> [88. 合并两个有序数组 - 力扣（LeetCode）](https://leetcode.cn/problems/merge-sorted-array/)
>
> > 合并数组后排序
> >
> > ```javascript
> > /**
> >  * @param {number[]} nums1
> >  * @param {number} m
> >  * @param {number[]} nums2
> >  * @param {number} n
> >  * @return {void} Do not return anything, modify nums1 in-place instead.
> >  */
> > var merge = function(nums1, m, nums2, n) {
> >     for(let i = 0; i < n; i++){
> >         nums1[m + i] = nums2[i]
> >     }
> >     nums1.sort((a, b) => a - b);
> > };
> > //合并数组后排序
> > ```
> >
> > 反向双指针
> >
> > ```JavaScript
> > /**
> >  * @param {number[]} nums1
> >  * @param {number} m
> >  * @param {number[]} nums2
> >  * @param {number} n
> >  * @return {void} Do not return anything, modify nums1 in-place instead.
> >  */
> > var merge = function(nums1, m, nums2, n) {
> >     let p1 = m - 1, p2 = n - 1, tail = m + n - 1
> >     while (p1 >= 0 || p2 >= 0) {
> >         if (p1 === -1) {
> >             nums1[tail--] = nums2[p2--];
> >         } else if (p2 === -1) {
> >             nums1[tail--] = nums1[p1--];
> >         } else if (nums1[p1] > nums2[p2]) {
> >             nums1[tail--] = nums1[p1--];
> >         } else {
> >             nums1[tail--] = nums2[p2--];
> >         }
> >     }
> > }
> > //利用nums1和nums2数组已经排序过的特性，且nums1的后n个值全是0，进行反向双指针排序，分别有m=0、n=0、当前的nums1[p1]>nums2[p2]、当前的nums1[p1]<=nums[p2]这4种情况，分别对应四种对nums[tail--]赋值的情况
> > ```

##### 代码随想录-数组-二分查找

# leetcode-704 二分查找

> ![image-20220705140011630](images/image-20220705140011630.png)
>
> [704. 二分查找 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-search/)
>
> > （版本一）左闭右闭区间 [left, right]，==建议此版本，因其他二分查找题目例如35只能用此版本==
> >
> > ```javascript
> > /**
> >  * @param {number[]} nums
> >  * @param {number} target
> >  * @return {number}
> >  */
> > var search = function(nums, target) {
> >     let left = 0, right = nums.length - 1
> >     while(left <= right){
> >         let mid = left + Math.floor((right - left) / 2)
> >         if(nums[mid] > target){
> >             right = mid - 1
> >         }else if(nums[mid] < target){
> >             left = mid + 1
> >         }else return mid
> >     }
> >     return -1
> > };
> > ```
> >
> > （版本二）左闭右开区间 [left, right)
> >
> > ```javascript
> > /**
> >  * @param {number[]} nums
> >  * @param {number} target
> >  * @return {number}
> >  */
> > var search = function(nums, target) {
> >     let left = 0, right = nums.length - 1
> >     while(left < right){
> >         let mid = left + Math.floor((right - left) / 2)
> >         if(nums[mid] > target){
> >             right = mid
> >         }else if(nums[mid] < target){
> >             left = mid + 1
> >         }else return mid
> >     }
> >     return -1
> > };
> > ```
> >
> > 两个版本的区别主要是右边闭合不闭合的问题，反映到具体代码就是left<right还是left<=right以及right=mid-1还是right=mid，关键在于if判断完毕后进入下一次while循环需不需要将上一个mid考虑进判断区间内(==由区间的闭合不闭合控制，边界问题==)

# ==leetcode-35 搜索插入位置==

> 见上

# leetcode-34 在排序数组中查找元素的第一个和最后一个位置

> ![image-20220706132153788](images/image-20220706132153788.png)
>
> [34. 在排序数组中查找元素的第一个和最后一个位置 - 力扣（LeetCode）](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/)
>
> > ```javascript
> > /**
> >  * @param {number[]} nums
> >  * @param {number} target
> >  * @return {number[]}
> >  */
> > var searchRange = function(nums, target) {
> >     const getLeftBorder = (nums, target) => {
> >         let left = 0, right = nums.length - 1;
> >         let leftBorder = -2;// 初始化，当target比nums[0]都小的时候的leftborder的值
> >         while(left <= right){
> >             let middle = left + ((right - left) >> 1);
> >             if(nums[middle] >= target){ // 寻找左边界，nums[middle] >= target的时候更新right
> >                 right = middle - 1;
> >                 leftBorder = right;
> >             } else {
> >                 left = middle + 1;
> >             }
> >         }
> >         return leftBorder;
> >     }
> > 
> >     const getRightBorder = (nums, target) => {
> >         let left = 0, right = nums.length - 1;
> >         let rightBorder = -2; // 初始化，当target比nums[nums.length - 1]都大的时候的rightborder的值
> >         while (left <= right) {
> >             let middle = left + ((right - left) >> 1);
> >             if (nums[middle] > target) {
> >                 right = middle - 1;
> >             } else { // 寻找右边界，nums[middle] <= target的时候更新left
> >                 left = middle + 1;
> >                 rightBorder = left;
> >             }
> >         }
> >         return rightBorder;
> >     }
> > 
> >     //getLeftBorder和getRightBorder的区别主要在于nums[middle]==target的时候做出的动作，将right=middle-1则是将搜索区间向左平移，最终得到leftborder，将left=middle+1则是将搜索区间向右平移，最终得到rightborder
> > 
> >     let leftBorder = getLeftBorder(nums, target);
> >     let rightBorder = getRightBorder(nums, target);
> >     if(leftBorder === -2 || rightBorder === -2) return [-1,-1];
> >     // 当target比nums[0]都小或者比nums[nums.length-1]都大的时候
> >     if (rightBorder - leftBorder > 1) return [leftBorder + 1, rightBorder - 1];
> >     // 当target在nums中存在的时候
> >     return [-1, -1];
> >     //当target数值范围在nums中，但nums中没有和target相等的元素的时候
> > };
> > ```

# leetcode-69 x的平方根

> 见上

# leetcode-367 有效的完全平方数

> ![image-20220706171610032](images/image-20220706171610032.png)
>
> [367. 有效的完全平方数 - 力扣（LeetCode）](https://leetcode.cn/problems/valid-perfect-square/)
>
> > ```javascript
> > /**
> >  * @param {number} num
> >  * @return {boolean}
> >  */
> > var isPerfectSquare = function(num) {
> >     let left = 0, right = num
> >     while(left <= right){
> >         let mid = left + ((right - left) >> 1)
> >         if(mid * mid > num){
> >             right = mid -1
> >         }else if(mid * mid < num){
> >             left = mid + 1
> >         }else return true
> >     }
> >     return false
> > };
> > ```

##### 代码随想录-数组-移除元素/双指针

# leetcode-27 移除元素

> 见上

# leetcode-26 删除排序数组中的重复项

> 见上

# leetcode-283 移动零

> ![image-20220706230609599](images/image-20220706230609599.png)
>
> [283. 移动零 - 力扣（LeetCode）](https://leetcode.cn/problems/move-zeroes/)
>
> > ```javascript
> > /**
> >  * @param {number[]} nums
> >  * @return {void} Do not return anything, modify nums in-place instead.
> >  */
> > var moveZeroes = function(nums) {
> >     let slow = 0, fast = 0
> >     for(; fast < nums.length; fast++){
> >         if(nums[fast] !== 0){
> >             nums[slow++] = nums[fast]
> >         }
> >     }
> >     while(slow < nums.length){
> >         nums[slow++] = 0
> >     }
> >     return nums
> >     // 双指针，当快指针非零时将慢指针等于快指针，快指针为零时不作为，这样快指针到数组末尾时慢指针和快指针之间的差距就是数组中零的个数，最后再将慢指针和nums末尾之间全填为零即可。
> > };
> > ```

# ==leetcode-844 比较含退格的字符串==

> ![image-20220707003123244](images/image-20220707003123244.png)
>
> [844. 比较含退格的字符串 - 力扣（LeetCode）](https://leetcode.cn/problems/backspace-string-compare/)
>
> > ==思路没错，但是因为js字符串不能像数组一样直接利用下标来进行字符修改，所以执行出错，要想实现下标修改需要用到slice，背离了算法题的初衷==
> >
> > ![image-20220707003339469](images/image-20220707003339469.png)
> >
> > ![image-20220707003417580](images/image-20220707003417580.png)
> >
> > 记录思路，代码执行不成功，但思路正确
> >
> > ```javascript
> > /**
> >  * @param {string} s
> >  * @param {string} t
> >  * @return {boolean}
> >  */
> > var backspaceCompare = function(s, t) {
> >     const afterBackspace = (string) => {
> >     let fast = 0, slow = 0
> >     for(; fast < string.length; fast++){
> >         let nums = 0
> >         if(string[fast] !== '#'){
> >             string[slow++] = string[fast] 
> >         }
> >         while(string[fast + nums] === '#'){
> >             nums++
> >             string[slow--] = ''
> >         }
> >         fast = fast + nums
> >         string[slow++] = string[fast]
> >         //错误的原因在于字符串不能像数组一样直接通过下标来修改具体的字符
> >     }
> >     while(slow < string.length){
> >         string[slow++] = ''
> >     }
> >     }
> >     let ss = afterBackspace(s)
> >     let tt = afterBackspace(t)
> >     if(ss === tt){
> >         return true
> >     }else {
> >         return false
> >     }
> > };
> > ```
> >
> > 双指针法
> >
> > ```javascript
> > /**
> >  * @param {string} s
> >  * @param {string} t
> >  * @return {boolean}
> >  */
> > var backspaceCompare = function(s, t) {
> >     let i = s.length - 1,
> >         j = t.length - 1,
> >         skipS = 0,
> >         skipT = 0;
> >     // 大循环
> >     while(i >= 0 || j >= 0){
> >         // S 循环
> >         while(i >= 0){
> >             if(s[i] === '#'){
> >                 skipS++;
> >                 i--;
> >             }else if(skipS > 0){
> >                 skipS--;
> >                 i--;
> >             }else break;//break跳出里层while循环
> >         }
> >         // T 循环
> >         while(j >= 0){
> >             if(t[j] === '#'){
> >                 skipT++;
> >                 j--;
> >             }else if(skipT > 0){
> >                 skipT--;
> >                 j--;
> >             }else break;//break跳出里层while循环
> >         }
> >         if(s[i] !== t[j]) return false;
> >         i--;
> >         j--;
> >     }
> >     return true;
> >     // 准备两个指针i，j分别指向s，t的末端，再维护两个变量skipS和skipT分别存放s，t字符串中的#的数量，从后往前分别遍历s，t，会遇到以下三种情况：当前字符为#，则skip++，当前字符不为#，且skip不为0，则skip--，当前字符不为#，且skip为0，则不变。对比过程中出现s，t当前字符不匹配则return false，若都能一一匹配则return true
> > };
> > ```
> >
> > 还有将字符串转换为数组再通过下标进行元素替换的方法，空间复杂度过高，这里不再过多提及

# leetcode-977 有序数组的平方

> ![image-20220707135725923](images/image-20220707135725923.png)
>
> [977. 有序数组的平方 - 力扣（LeetCode）](https://leetcode.cn/problems/squares-of-a-sorted-array/)
>
> > 双指针解法
> >
> > ```javascript
> > /**
> >  * @param {number[]} nums
> >  * @return {number[]}
> >  */
> > var sortedSquares = function(nums) {
> >     const res = []
> >     for(let i = 0, j = nums.length - 1; i <= j;){
> >         let left = nums[i] * nums[i]
> >         let right = nums[j] * nums[j]
> >         if(left < right){
> >             res.push(right)
> >             j--
> >         }else{
> >             res.push(left)
> >             i++
> >         }
> >     }
> >     return res.reverse()
> > };
> > // 用push+reverse比直接用unshift性能更好一点
> > ```
> >
> > 接口写法
> >
> > ```JavaScript
> > /**
> >  * @param {number[]} nums
> >  * @return {number[]}
> >  */
> > var sortedSquares = function(A) {
> >     A.forEach((x,y) => A[y] = A[y]*A[y])
> >     A.sort((a,b) => a-b)
> >     return A
> > };
> > ```
> >
> > ```JavaScript
> > /**
> >  * @param {number[]} nums
> >  * @return {number[]}
> >  */
> > var sortedSquares = function(A) {
> >     return A.map(x => x*x).sort((a,b) => a-b)
> > };
> > ```
> >
> > 接口写法甚至比双指针写法快30-50ms，尚未明白原因

##### 代码随想录-数组-长度最小的子数组/滑动窗口 类似双指针

# leetcode-209 长度最小的子数组

> ![image-20220707150437805](images/image-20220707150437805.png)
>
> [209. 长度最小的子数组 - 力扣（LeetCode）](https://leetcode.cn/problems/minimum-size-subarray-sum/)
>
> > ```javascript
> > /**
> >  * @param {number} target
> >  * @param {number[]} nums
> >  * @return {number}
> >  */
> > var minSubArrayLen = function(target, nums) {
> >     let fast = 0, slow = 0, sum = 0, result = Infinity
> >     for(; fast < nums.length; fast++){
> >         sum += nums[fast]
> >         while(sum >= target){
> >             let len = fast - slow + 1
> >             result = Math.min(result,len)
> >             sum -= nums[slow++]
> >         }
> >     }
> >     return result == Infinity ? 0 : result
> > };
> > //这道题有许多值得注意的边界问题
> > //第一个是sum = target这个问题，经过具体例子模拟这个边界条件可以并入sum > target当中
> > //第二个是当target=nums的所有元素和的时候，因为我一开始将result=nums.length，return result == nums.length ? 0 : result，所以当这个边界条件触发时返回的是0，后面分别改为result=Infinity以及return result == Infinity ? 0 : result即可解决
> > //还有一个while中执行语句先后顺序的问题，应该先计算当前的滑动窗口的length以及比较大小，然后再对slow进行自增
> > ```

# ==leetcode-904 水果成篮==

> ![image-20220707191643981](images/image-20220707191643981.png)
>
> ![image-20220707191655210](images/image-20220707191655210.png)
>
> [904. 水果成篮 - 力扣（LeetCode）](https://leetcode.cn/problems/fruit-into-baskets/)
>
> > 代码不通过，记录思路，且思路需要改进，改进见下方红字
> >
> > ```javascript
> > /**
> >  * @param {number[]} fruits
> >  * @return {number}
> >  */
> > var totalFruit = function(fruits) {
> >     let fast = 0, slow = 0, sum = 0, result = Infinity, minIndex = fruits.length - 1
> >     const map = new Map()
> >     for(; fast < fruits.length; fast++){
> >         map.set(fruits[fast],fast)
> >         if(map.size > 2){
> >             for(const [fruit,index] of map){
> >                 if(index < minIndex){
> >                     minIndex = index
> >                 }
> >             }
> >             map.delete(minIndex)
> >             slow = minIndex + 1
> >         }
> >         sum = fast - slow + 1
> >         result = sum > result ? sum : result
> >     }
> >     return result == Infinity ? 0 : result
> > };
> > ```
> >
> > 滑动窗口
> >
> > ==最初使用一个长度为2的数组来抽象两个篮子，将这个篮子数组初始化为[fruits[0],fruits[1]]，再通过判断fast指向的数值是否存在于篮子数组中来进行进一步操作，但是对于类似[0,0,1,1]这样的fruits，抽象篮子数组初始化的两个值并非两个不同的值，这样导致后续的fast指向值判断出现问题==
> >
> > ==利用Map的set操作不会添加重复的keyvalue对的特性(key不会重复)，进行去重，这样可以避免[0,0,1,1]这样的情况出现==
> >
> > 每次map.size超过2时，将第一个key删除，然后判断fast-slow+1和之前的fast-slow+1相比谁大，去较大值
> >
> > ```javascript
> > /**
> >  * @param {number[]} fruits
> >  * @return {number}
> >  */
> > var totalFruit = function(fruits) {
> >     let fast = 0, slow = 0, result = 1
> >     const map = new Map()
> >     for(; fast < fruits.length; fast++){
> >         map.set(fruits[fast],fast)
> >         if(map.size > 2){
> >             let minIndex = fruits.length - 1
> >             for(const [fruit,index] of map){
> >                 if(index < minIndex){
> >                     minIndex = index
> >                 }
> >             }
> >             map.delete(fruits[minIndex])
> >             slow = minIndex + 1
> >         }
> >         result = Math.max(result, fast - slow + 1)
> >     }
> >     return result
> > };
> > ```
> >

# ==leetcode-76 最小覆盖子串==

> ![image-20220707224912987](images/image-20220707224912987.png)
>
> [76. 最小覆盖子串 - 力扣（LeetCode）](https://leetcode.cn/problems/minimum-window-substring/)
>
> > 超出执行时间，记录思路(应该是set、get、has操作过多)
> >
> > ```JavaScript
> > /**
> >  * @param {string} s
> >  * @param {string} t
> >  * @return {string}
> >  */
> > var minWindow = function(s, t) {
> >     let fast = 0, slow = 0, minLen = Infinity, start
> >     const need = new Map()
> >     for(let i of t){
> >         need.set(i, need.has(i) ? need.get(i) + 1 : 1)
> >     }
> >     let needNums = need.size
> >     for(; fast < s.length; fast++){
> >         if(need.has(s[fast])){
> >             need.set(s[fast], need.get(s[fast]) - 1)
> >         }
> >         if(need.get(s[fast]) === 0){
> >             needNums--
> >         }
> >         while(needNums === 0){
> >             if(minLen > fast - slow + 1){
> >                 minLen = fast - slow + 1
> >                 start = slow
> >             }
> >             if(need.has(s[slow])){
> >                 if(need.get(s[slow]) === 0){
> >                     needNums++
> >                     need.set(s[slow], need.get(s[slow]) + 1)
> >                     slow++
> >                 }else {
> >                     need.set(s[slow], need.get(s[slow]) + 1)
> >                 }
> >             }
> >         }
> >     }
> >     return s.slice(start, start + minLen)
> > };
> > ```
> >
> > 正确解法，要注意if(map[rightChar] !== undefined)不能简单写成if(map[rightChar])，leftChar也是一样，其中包含边界情况(主要是if(0)和if(undefined)都是false但是0!==undefined)(map[leftChar]可能存在<0的情况)
> >
> > [LeetCode#76. 最小覆盖子串 Minimum Window Substring | 滑动窗口_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1eV411C7s2?spm_id_from=333.1007.top_right_bar_window_history.content.click&vd_source=9de430adabffb3f1879c1eb17ba0f461)
> >
> > ```JavaScript
> > /**
> >  * @param {string} s
> >  * @param {string} t
> >  * @return {string}
> >  */
> > var minWindow = function (s,t) {
> >     let minLen = Infinity, start
> >     let map = {}
> >     let missingType = 0
> >     for (const char of t) {
> >         if(!map[char]){
> >             map[char] = 1
> >             missingType++
> >         }else {
> >             map[char]++
> >         }
> >     }
> >     let left = 0, right = 0
> >     for(; right < s.length; right++){
> >         let rightChar = s[right]
> >         if(map[rightChar] !== undefined){
> >             map[rightChar]--
> >         }
> >         if(map[rightChar] === 0){
> >             missingType--
> >         }
> >         while(missingType === 0){
> >             if(right - left + 1 < minLen){
> >                 minLen = right - left + 1
> >                 start = left
> >             }
> >             let leftChar = s[left]
> >             if(map[leftChar] !== undefined){
> >                 map[leftChar]++
> >             }
> >             if(map[leftChar] > 0){
> >                 missingType++
> >             }
> >             left++
> >         }
> >     }
> >     return s.slice(start, start + minLen)
> > }
> > ```

##### 代码随想录-数组-螺旋矩阵2

# leetcode-59 螺旋矩阵2

> ![image-20220708151955030](images/image-20220708151955030.png)
>
> [59. 螺旋矩阵 II - 力扣（LeetCode）](https://leetcode.cn/problems/spiral-matrix-ii/)
>
> > 此处应该注意矩阵和坐标轴两种体系对于确定一个元素点位置地思想方式的不同
> >
> > 例如从左到右地移动，矩阵的思想是从第一列移动到最后一列，所以是两个[]中的后一个[]的值进行自增，而坐标轴的思想则是横轴坐标自增，是两个[]中的前一个[]的值进行自增
> >
> > ```javascript
> > /**
> >  * @param {number} n
> >  * @return {number[][]}
> >  */
> > var generateMatrix = function(n) {
> >     //主要思想是循环不变量原则，对于每一条边都遵守只处理开头结点而不处理结尾结点这一原则
> >     let startx = 0, starty = 0, count = 1, offset = 1 
> >     //初始化起始位置startx和starty、维护一个用来自增的变量count、维护一个offset用来控制终止位置从而不去处理结尾结点
> >     let loop = mid = n >> 1
> >     //计算需要循环的次数，通过模二计算即可得到
> >     //计算可能存在的最中间单独一个数的情况下的该数的row和col
> >     let res = new Array(n).fill(0).map(() => new Array(n).fill(0))
> >     //初始化一个全零空矩阵
> >     while(loop--){
> >         let row = startx, col = starty
> >         //初始化row和col
> >         for(; col < n - offset; col++){
> >             res[row][col] = count++
> >         }
> >         for(; row < n - offset; row++){
> >             res[row][col] = count++
> >         }
> >         for(; col > starty; col--){
> >             res[row][col] = count++
> >         }
> >         for(; row > startx; row--){
> >             res[row][col] = count++
> >         }
> >         //按照顺时针方向从外向内转圈，设定好每一条边的开始结束条件
> >         startx++
> >         starty++
> >         offset++
> >         //转完一圈后，下一圈变小，对应地更改起始位置startx和starty，再对应地更改用于控制终止位置地变量offset
> >     }
> >     if(n % 2 === 1){
> >         res[mid][mid] = count
> >     }
> >     //特殊情况，如果存在最内的单独一个数，对这种情况进行单独赋值处理
> >     return res
> > };
> > ```

# ==leetcode-54 螺旋矩阵==

> ![image-20220709014819344](images/image-20220709014819344.png)
>
> [54. 螺旋矩阵 - 力扣（LeetCode）](https://leetcode.cn/problems/spiral-matrix/)
>
> > ```javascript
> > /**
> >  * @param {number[][]} matrix
> >  * @return {number[]}
> >  */
> > var spiralOrder = function(matrix) {
> >     const res = []
> >     let m = matrix.length, n = matrix[0].length
> >     let min = Math.min(m,n)
> >     let loop = Math.floor(min >> 1)
> >     let count = 0, startrow = 0, startcol = 0, offset = 1, endrow, endcol
> >     while(loop--){
> >         let row = startrow, col = startcol
> >         for(; col < n - offset; col++){
> >             res[count++] = matrix[row][col]
> >         }
> >         for(; row < m - offset; row++){
> >             res[count++] = matrix[row][col]
> >         }
> >         for(; col > startcol; col--){
> >             res[count++] = matrix[row][col]
> >         }
> >         for(; row > startrow; row--){
> >             res[count++] = matrix[row][col]
> >         }
> >         startcol++
> >         startrow++
> >         offset++
> >         endrow = row + 1
> >         endcol = col + 1
> >     }
> >     if(min % 2 === 1 && min !== 1){            
> >         //多行多列的情况，至少经历过一次完整的循环，可以使用endrow和endcol
> >         //此处是对多行多列中可能存在的中间剩余一行或一列不能形成一个完整循环的特殊处理
> >         if(m > n){                                     //行数多于列数，循环完毕最后剩下一列进行特殊处理               
> >             for(; endrow < m - offset + 1; endrow++){
> >                 res[count++] = matrix[endrow][endcol]
> >             }
> >         }else{                                         //行数小于等于列数，循环完毕最后剩下一行或一个数值进行特殊处理
> >             for(; endcol < n - offset + 1; endcol++){
> >                 res[count++] = matrix[endrow][endcol]
> >             }
> >         }
> >     }else if(n === 1 && m !== 1){                      //多行一列的情况，没有经历过一次完整循环
> >         for(let start = 0; start < m; start++){
> >                     res[count++] = matrix[start][0]
> >                 }
> >     }else if(m === 1 && n !== 1){                      //多列一行的情况，没有经历过一次完整循环
> >         for(let start = 0; start < n; start++){
> >                 res[count++] = matrix[0][start]
> >             }
> >     }else if(m === 1 && n === 1){                      //一行一列的情况，没有经历过一次完整循环
> >         res[count++] = matrix[0][0]
> >     }
> >     return res
> > };
> > ```

##### 代码随想录-链表

# 关于链表

> js中没有原生链表，需要人为模拟
>
> ```javascript
> class ListNode {
>   val;
>   next = null;
>   constructor(value) {
>     this.val = value;
>     this.next = null;//链表最末尾的结点的指针指向null
>   }
> }
> //leetcode已经提前定义好了ListNode不要自己手动定义了
> ```
>
> 定义一个链表head = [1,2,3,...,n]，那么链表本身head就是一个指向头节点的指针(==又或者可以解释为只要找到一条链表的第一个结点，这个节点包含了节点本身的value值和它的指针指向next，那么就可以顺着这个next一直找到链表的最末尾结点，那么这个第一个节点就用来可以代表整个链表，也就是说一条链表本身就是指向自身的头结点/头结点可以代表整个链表==)
>
> ```javascript
> const dummyHead = new ListNode[0,head]
> //创建虚拟头节点
> //原理为创建一个value为0、指针next指向head的结点，因为它指向的head是链表的头结点，所以它排在head头结点的前面，是链表head之前的一个虚拟节点
> ```

# leetcode-203 移除链表元素

> ![image-20220709172053824](images/image-20220709172053824.png)
>
> [203. 移除链表元素 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-linked-list-elements/)
>
> > ```javascript
> > /**
> >  * Definition for singly-linked list.
> >  * function ListNode(val, next) {
> >  *     this.val = (val===undefined ? 0 : val)
> >  *     this.next = (next===undefined ? null : next)
> >  * }
> >  */
> > /**
> >  * @param {ListNode} head
> >  * @param {number} val
> >  * @return {ListNode}
> >  */
> > var removeElements = function(head, val) {
> >     const dummyHead = new ListNode(0,head)
> >     let cur = dummyHead
> >     while(cur.next){
> >         if(cur.next.val === val){
> >             cur.next = cur.next.next
> >         }else{
> >             cur = cur.next
> >         }
> >     }
> >     return dummyHead.next
> > };
> > ```

# ==leetcode-707 设计链表==

> ![image-20220710125904840](images/image-20220710125904840.png)
>
> [707. 设计链表 - 力扣（LeetCode）](https://leetcode.cn/problems/design-linked-list/)
>
> > ```javascript
> > var MyLinkedList = function() {
> >     this._size = 0;
> >     this._tail = null;
> >     this._head = null;
> > };
> > 
> > /**
> >  * Get the value of the index-th node in the linked list. If the index is invalid, return -1. 
> >  * @param {number} index
> >  * @return {number}
> >  */
> > MyLinkedList.prototype.getNode = function(index) {
> >     if(index < 0 || index >= this._size) return null;
> >     // 创建虚拟头节点
> >     let cur = new ListNode(0, this._head);
> >     // 0 -> head
> >     while(index-- >= 0) {
> >         cur = cur.next;
> >     }
> >     return cur;
> > };
> > MyLinkedList.prototype.get = function(index) {
> >     if(index < 0 || index >= this._size) return -1;
> >     // 获取当前节点
> >     return this.getNode(index).val;
> > };
> > 
> > /**
> >  * Add a node of value val before the first element of the linked list. After the insertion, the new node will be the first node of the linked list. 
> >  * @param {number} val
> >  * @return {void}
> >  */
> > MyLinkedList.prototype.addAtHead = function(val) {
> >     const node = new ListNode(val, this._head);
> >     this._head = node;
> >     this._size++;
> >     if(!this._tail) {
> >         this._tail = node;
> >     }
> > };
> > 
> > /**
> >  * Append a node of value val to the last element of the linked list. 
> >  * @param {number} val
> >  * @return {void}
> >  */
> > MyLinkedList.prototype.addAtTail = function(val) {
> >     const node = new ListNode(val, null);
> >     this._size++;
> >     if(this._tail) {
> >         this._tail.next = node;
> >         this._tail = node;
> >         return;
> >     }
> >     this._tail = node;
> >     this._head = node;
> > };
> > 
> > /**
> >  * Add a node of value val before the index-th node in the linked list. If index equals to the length of linked list, the node will be appended to the end of linked list. If index is greater than the length, the node will not be inserted. 
> >  * @param {number} index 
> >  * @param {number} val
> >  * @return {void}
> >  */
> > MyLinkedList.prototype.addAtIndex = function(index, val) {
> >     if(index > this._size) return;
> >     if(index <= 0) {
> >         this.addAtHead(val);
> >         return;
> >     }
> >     if(index === this._size) {
> >         this.addAtTail(val);
> >         return;
> >     }
> >     // 获取目标节点的上一个的节点
> >     const node = this.getNode(index - 1);
> >     node.next = new ListNode(val, node.next);
> >     this._size++;
> > };
> > 
> > /**
> >  * Delete the index-th node in the linked list, if the index is valid. 
> >  * @param {number} index
> >  * @return {void}
> >  */
> > MyLinkedList.prototype.deleteAtIndex = function(index) {
> >     if(index < 0 || index >= this._size) return;
> >     if(index === 0) {
> >         this._head = this._head.next;
> >         // 如果删除的这个节点同时是尾节点，要处理尾节点
> >         if(index === this._size - 1){
> >             this._tail = this._head
> >         }
> >         this._size--;
> >         return;
> >     }
> >     // 获取目标节点的上一个的节点
> >     const node = this.getNode(index - 1);    
> >     node.next = node.next.next;
> >     // 处理尾节点
> >     if(index === this._size - 1) {
> >         this._tail = node;
> >     }
> >     this._size--;
> > };
> > 
> > /**
> >  * Your MyLinkedList object will be instantiated and called as such:
> >  * var obj = new MyLinkedList()
> >  * var param_1 = obj.get(idex)
> >  * obj.addAtHead(val)
> >  * obj.addAtTail(val)
> >  * obj.addAtIndex(index,val)
> >  * obj.deleteAtIndex(index)
> >  */
> > ```

# leetcode-206 反转链表

> ![image-20220709214316467](images/image-20220709214316467.png)
>
> [206. 反转链表 - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-linked-list/)
>
> > 双指针
> >
> > 下图有错，应该是pre先移动，cur再移动
> >
> > ![img](images/008eGmZEly1gnrf1oboupg30gy0c44qp.gif)
> >
> > ```javascript
> > /**
> >  * Definition for singly-linked list.
> >  * function ListNode(val, next) {
> >  *     this.val = (val===undefined ? 0 : val)
> >  *     this.next = (next===undefined ? null : next)
> >  * }
> >  */
> > /**
> >  * @param {ListNode} head
> >  * @return {ListNode}
> >  */
> > var reverseList = function(head) {
> >     if(!head || !head.next) return head;
> >     let temp = null, pre = null, cur = head;
> >     while(cur) {
> >         temp = cur.next;
> >         cur.next = pre;
> >         pre = cur;
> >         cur = temp;
> >     }
> >     // temp = cur = null;
> >     return pre;
> > };
> > ```

# leetcode-24 两两交换链表中的节点

> ![image-20220710124606082](images/image-20220710124606082.png)
>
> [24. 两两交换链表中的节点 - 力扣（LeetCode）](https://leetcode.cn/problems/swap-nodes-in-pairs/)
>
> > 注意交换中的先后顺序很容易搞混
> >
> > ![image-20220710125113950](images/image-20220710125113950.png)
> >
> > 在一个循环开始前，先将pre和cur分别定义为temp.next和temp.next.next
> >
> > 保证指针的连续性，第一步将pre.next = cur.next，这样可以避免改变cur.next的指向后丢失指向3的指针
> >
> > 第二步将cur.next = pre
> >
> > 第三步将temp.next = cur
> >
> > 最后将temp = pre，这样对于3和4来说，1也就是temp就是它们的虚拟头结点，这样进入了下一个循环
> >
> > ==由上可知，每一次循环的必要条件都是temp的next和temp的next的next都不为null，这样才能进行一次完整的循环，所以while循环的条件为while(temp.next && temp.next.next)==
> >
> > 最后返回dummyHead的next
> >
> > ```javascript
> > /**
> >  * Definition for singly-linked list.
> >  * function ListNode(val, next) {
> >  *     this.val = (val===undefined ? 0 : val)
> >  *     this.next = (next===undefined ? null : next)
> >  * }
> >  */
> > /**
> >  * @param {ListNode} head
> >  * @return {ListNode}
> >  */
> > var swapPairs = function(head) {
> >     let dummyHead = new ListNode(0,head)
> >     let temp = dummyHead
> >     while(temp.next && temp.next.next){
> >         let cur = temp.next.next, pre = temp.next
> >         pre.next = cur.next
> >         cur.next = pre
> >         temp.next = cur
> >         temp = pre
> >     }
> >     return dummyHead.next
> > };
> > ```

# ==leetcode-19 删除链表的倒数第n个节点==

> ![image-20220710133750322](images/image-20220710133750322.png)
>
> [19. 删除链表的倒数第 N 个结点 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)
>
> > 双指针
> >
> > 通过快指针先比慢指针走n+1步，然后两个指针同步走，直到快指针到达null停止，这时慢指针指向的就是倒数第n+1个点，这样就可以通过slow.next = slow.next.next实现删除倒数第n个指针
> >
> > ==这里需要注意几个边界问题==，快慢指针的初始值应该是dummyHead而不是dummyHead.next，在某些情况下要删除链表倒数第length个元素(也就是链表正数第一个元素)，那么快指针就需要先走length+1步，这样就走到了null的后面，这里会报错
> >
> > ```javascript
> > /**
> >  * Definition for singly-linked list.
> >  * function ListNode(val, next) {
> >  *     this.val = (val===undefined ? 0 : val)
> >  *     this.next = (next===undefined ? null : next)
> >  * }
> >  */
> > /**
> >  * @param {ListNode} head
> >  * @param {number} n
> >  * @return {ListNode}
> >  */
> > var removeNthFromEnd = function(head, n) {
> >     let dummyHead = new ListNode(0,head)
> >     let slow = fast = dummyHead
> >     for(let i = 0; i < n + 1; i++){
> >         fast = fast.next
> >     }
> >     while(fast){
> >         fast = fast.next
> >         slow = slow.next
> >     }
> >     slow.next = slow.next.next
> >     return dummyHead.next
> > };
> > ```

# ==leetcode-面试题02.07. 链表相交==

> ![image-20220710172444826](images/image-20220710172444826.png)
>
> ![image-20220710172458066](images/image-20220710172458066.png)
>
> [面试题 02.07. 链表相交 - 力扣（LeetCode）](https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/)
>
> > 解题思路
> >
> > 首先明确一点容易出错的地方，交点不是数值相等，而是指针相等，例如headA链表每个节点都是1，而headB只有交点是1，那么只有指针相等的地方才是交点，而不是headA中每个节点都是交点。
> >
> > 明确完毕后，解题思路可以转换为找到两个链表指针相等的交点，那么可以先求出两个链表的长度，再求出长度差，让长的链表的指针先走长度差，然后两个指针在同一起跑线上，同步向前移动，直到两个指针汇聚到同一交点(此时数值相等)，然后返回该相等数值即可
> >
> > ```javascript
> > /**
> >  * Definition for singly-linked list.
> >  * function ListNode(val) {
> >  *     this.val = val;
> >  *     this.next = null;
> >  * }
> >  */
> > 
> > /**
> >  * @param {ListNode} headA
> >  * @param {ListNode} headB
> >  * @return {ListNode}
> >  */
> > var getListLen = function(head){
> >     let dummyHead = new ListNode(0,head)
> >     let cur =dummyHead, Len = 0
> >     while(cur){
> >         cur = cur.next
> >         Len++
> >     }
> >     return Len
> > }
> > 
> > var getIntersectionNode = function(headA, headB) {
> >     let LenA = getListLen(headA), LenB = getListLen(headB)
> >     let curA = headA, curB = headB
> >     if(LenA < LenB){
> >         [LenA, LenB] = [LenB, LenA];
> >         [curA, curB] = [curB, curA];
> >     }
> >     while(LenA - LenB){
> >         curA = curA.next
> >         LenA--
> >     }
> >     while(curA !== curB){
> >         curA = curA.next
> >         curB = curB.next
> >     }
> >     return curA
> > };
> > ```

# ==leetcode-142 环形链表2==

> ![image-20220711111819452](images/image-20220711111819452.png)
>
> ![image-20220711111831358](images/image-20220711111831358.png)
>
> [142. 环形链表 II - 力扣（LeetCode）](https://leetcode.cn/problems/linked-list-cycle-ii/)
>
> > ![142.环形链表II（求入口）](images/008eGmZEly1goo58gauidg30fw0bi4qr.gif)
> >
> > 首先判断链表长度，如果是0和1那么肯定没有环直接返回null，不是0和1再使用双指针法，快指针每次走两步，慢指针每次走一步，如果这个链表有环，那么最终肯定会出现快慢指针指向同一节点的情况，由此来判断是否存在环，不存在返回null，存在则继续操作。此时将slow=head，从头开始每次只走一步，fast此时在相遇位置也每次只走一步，二者再次相遇位置即为环入口。
> >
> > ==两个小问题需要注意，在第一次两个指针指向同一节点的循环中，while循环判定条件是slow !\== fast，所以在初始化slow和fast时不能等于head，而要分别等于head.next和head.next.next，这样才不会破坏while循环。==
> >
> > 在while循环中用fast && fast.next来确定fast还能不能继续走两步，此处应该注意fast要在fast.next前面，如果fast在fast.next后面，会出现当fast为null时，先判定fast.next报错的情况，此时fast在fast.next后面还没有进行判定==(&&的短路特性)==
> >
> > [代码随想录 (programmercarl.com)](https://www.programmercarl.com/0142.环形链表II.html#思路)
> >
> > ```javascript
> > /**
> >  * Definition for singly-linked list.
> >  * function ListNode(val) {
> >  *     this.val = val;
> >  *     this.next = null;
> >  * }
> >  */
> > 
> > /**
> >  * @param {ListNode} head
> >  * @return {ListNode}
> >  */
> > var detectCycle = function(head) {
> >     if(!head || !head.next){
> >         return null
> >     }
> >     let slow = head.next, fast = head.next.next
> >     while(slow !== fast && fast && fast.next){
> >         fast = fast.next.next
> >         slow = slow.next
> >     }
> >     if(!fast || !fast.next){
> >         return null
> >     }
> >     slow = head
> >     while(slow !== fast){
> >         slow = slow.next
> >         fast = fast.next
> >     }
> >     return slow
> > };
> > ```

##### 代码随想录-哈希表

# leetcode-242 有效的字母异位词

> ![image-20220711123832694](images/image-20220711123832694.png)
>
> [242. 有效的字母异位词 - 力扣（LeetCode）](https://leetcode.cn/problems/valid-anagram/)
>
> > 自写，首先判断s和t的长度是否相等，不相等直接返回false。使用Map利用for循环来记录s的每个字符及其出现次数，再for循环t，来对应地减少Map中存在的字符的value值也就是次数，第二个for循环中如果出现循环到字符是Map中没有的或者有但其value值已经为0的情况，那么直接返回false。最后对Map进行遍历查找，如果有key的value值不为0，那么直接返回false。最后如果所有操作都通过返回true
> >
> > ```javascript
> > /**
> >  * @param {string} s
> >  * @param {string} t
> >  * @return {boolean}
> >  */
> > var isAnagram = function(s, t) {
> >     if(s.length !== t.length){
> >         return false
> >     }
> >     const record = new Map
> >     for(let i = 0; i < s.length; i++){
> >         if(!record.has(s[i])){
> >             record.set(s[i],1)
> >         }else{
> >             record.set(s[i],record.get(s[i]) + 1)
> >         }
> >     }
> >     for(let j = 0; j < s.length; j++){
> >         if(!record.has(t[j]) || record.get(t[j]) === 0){
> >             return false
> >         }else{
> >             record.set(t[j],record.get(t[j]) - 1)
> >         }
> >     }
> >     for(const[i,j] of record){
> >         if(j !== 0){
> >             return false
> >         }
> >     }
> >     return true
> > };
> > ```
> >
> > 代码随想录解法，和自写区别在于使用数组记录，数组长度26，0-25分别对应a-z，使用charCodeAt将字母变成对应地数字
> >
> > ```javascript
> > /**
> >  * @param {string} s
> >  * @param {string} t
> >  * @return {boolean}
> >  */
> > var isAnagram = function(s, t) {
> >     if(s.length !== t.length) return false;
> >     const resSet = new Array(26).fill(0);
> >     const base = "a".charCodeAt();
> >     for(const i of s) {
> >         resSet[i.charCodeAt() - base]++;
> >     }
> >     for(const i of t) {
> >         if(!resSet[i.charCodeAt() - base]) return false;
> >         resSet[i.charCodeAt() - base]--;
> >     }
> >     return true;
> > };
> > ```

# leetcode-383 赎金信

> ![image-20220711132536452](images/image-20220711132536452.png)
>
> [383. 赎金信 - 力扣（LeetCode）](https://leetcode.cn/problems/ransom-note/)
>
> > 自写
> >
> > ```javascript
> > /**
> >  * @param {string} ransomNote
> >  * @param {string} magazine
> >  * @return {boolean}
> >  */
> > var canConstruct = function(ransomNote, magazine) {
> >     if(ransomNote.length > magazine.length){
> >         return false
> >     }
> >     const record = new Array(26).fill(0)
> >     const base = 'a'.charCodeAt()
> >     for(const i of ransomNote){
> >         record[i.charCodeAt() - base]++
> >     }
> >     for(const i of magazine){
> >         record[i.charCodeAt() - base]--
> >     }
> >     return record.every((element) => element <= 0)
> > };
> > ```
> >
> > 代码随想录解法
> >
> > ```JavaScript
> > /**
> >  * @param {string} ransomNote
> >  * @param {string} magazine
> >  * @return {boolean}
> >  */
> > var canConstruct = function(ransomNote, magazine) {
> >     const strArr = new Array(26).fill(0), 
> >         base = "a".charCodeAt();
> >     for(const s of magazine) {
> >         strArr[s.charCodeAt() - base]++;
> >     }
> >     for(const s of ransomNote) {
> >         const index = s.charCodeAt() - base;
> >         if(!strArr[index]) return false;
> >         strArr[index]--;
> >     }
> >     return true;
> > };
> > ```

# ==leetcode-49 字母异位词分组==

> ![image-20220712153239261](images/image-20220712153239261.png)
>
> [49. 字母异位词分组 - 力扣（LeetCode）](https://leetcode.cn/problems/group-anagrams/)
>
> > 遍历数组strs，里面的每一个值都是一个字符串，再遍历每一个字符串，将字符串的每个字符转换为ascii值然后找到该ascii作为索引在arr数组中的位置，将该位置的数值++，这样每一个字符串就根据它自身字符的种类和数量对应了一个唯一的数组，将该数组join转换为以逗号为分隔符的字符串作为key存入map(==这里不直接用数组作为key的原因是数组是引用数据类型而字符串是基本数据类型，第一个循环中每次const arr= new Array时都将arr这个变量原先指向的堆内存地址覆盖掉了，赋予了arr一个全新的堆内存地址，所以在后面map.has[arr]的时候永远是false，即使此时判断的arr的具体表现形式在map中有相同的表现形式的key==)，对于每个字符串都判断一下map中是否存在以该字符串对应的数组作为key的情况，不存在即将该字符串变成数组作为value set进去，存在即将value也就是map.get(key)进行解构再加上该字符串然后变成数组set进去，最后遍历map，map的每个value就是字母异位词分组。
> >
> > ```javascript
> > /**
> >  * @param {string[]} strs
> >  * @return {string[][]}
> >  */
> > var groupAnagrams = function(strs) {
> >     const map = new Map()
> >     for(const str of strs){
> >         const arr = new Array(26).fill(0)
> >         for(const letter of str){
> >             arr[letter.charCodeAt() - 'a'.charCodeAt()]++
> >         }
> >         const key = arr.join()
> >         map.has(key) ? map.set(key, [...map.get(key),str]) : map.set(key, [str])
> >     }
> >     const result = []
> >     for(const arr of map){
> >         result.push(arr[1])
> >     }
> >     return result
> > };
> > ```

# ==leetcode-438 找到字符串中所有字母异位词==

> ![image-20220712185450955](images/image-20220712185450955.png)
>
> [438. 找到字符串中所有字母异位词 - 力扣（LeetCode）](https://leetcode.cn/problems/find-all-anagrams-in-a-string/)
>
> > 哈希表+滑动窗口
> >
> > 用哈希表存储p的数据，再用滑动窗口进行遍历，维护一个变量valid来记录当前滑动窗口中和哈希表中同一字母数量对应的上的字母的数量，当这个valid=map.size时，就说明当前滑动窗口包含的字符串是一个字母异位词，记录下慢指针并push进res数组即可，最后return res
> >
> > ```javascript
> > /**
> >  * @param {string} s
> >  * @param {string} p
> >  * @return {number[]}
> >  */
> > var findAnagrams = function (s, p) {
> >     const pLen = p.length
> >     const res = [] // 返回值
> >     const map = new Map() // 存储 p 的字符
> >     for (let item of p) {
> >         map.set(item, map.get(item) ? map.get(item) + 1 : 1)
> >     }
> >     // 存储窗口里的字符情况
> >     const window = new Map()
> >     let valid = 0 // 有效字符个数
> > 
> >     for (let i = 0; i < s.length; i++) {
> >         const right = s[i]
> >         // 向右扩展
> >         window.set(right, window.get(right) ? window.get(right) + 1 : 1)
> >         // 扩展的节点值是否满足有效字符
> >         if (window.get(right) === map.get(right)) {
> >             valid++
> >         }
> >         if (i >= pLen) {
> >             // 移动窗口 -- 超出之后，收缩回来， 这是 pLen 长度的固定窗口
> >             const left = s[i - pLen]
> >             // 原本是匹配的，现在移出去了，肯定就不匹配了
> >             if (window.get(left) === map.get(left)) {
> >                 valid--
> >             }
> >             window.set(left, window.get(left) - 1)
> >         }
> >         // 如果有效字符数量和存储 p 的map 的数量一致，则当前窗口的首字符保存起来
> >         if (valid === map.size) {
> >             res.push(i - pLen+1)
> >         }
> >     }
> >     return res
> > };
> > ```

# leetcode-349 两个数组的交集

> ![image-20220712194439265](images/image-20220712194439265.png)
>
> [349. 两个数组的交集 - 力扣（LeetCode）](https://leetcode.cn/problems/intersection-of-two-arrays/)
>
> > 将两个数组用set去重，再遍历size小的set，当出现两个set都含有的值时push进res数组，最后返回res数组
> >
> > ```javascript
> > /**
> >  * @param {number[]} nums1
> >  * @param {number[]} nums2
> >  * @return {number[]}
> >  */
> > var intersection = function(nums1, nums2) {
> >     let nums1set = new Set(nums1)
> >     let nums2set = new Set(nums2)
> >     const res = []
> >     if(nums1set.size < nums2set.size){
> >         const temp = nums1set
> >         nums1set = nums2set
> >         nums2set = temp
> >     }
> >     for(const i of nums2set){
> >         if(nums1set.has(i)){
> >             res.push(i)
> >         }
> >     }
> >     return res
> > };
> > ```

# leetcode-350 两个数组的交集2

> ![image-20220713132305266](images/image-20220713132305266.png)
>
> [350. 两个数组的交集 II - 力扣（LeetCode）](https://leetcode.cn/problems/intersection-of-two-arrays-ii/)
>
> > ```javascript
> > /**
> >  * @param {number[]} nums1
> >  * @param {number[]} nums2
> >  * @return {number[]}
> >  */
> > var intersect = function(nums1, nums2) {
> >     const map = new Map()
> >     const res = []
> >     if(nums1.length < nums2.length){
> >         const temp = nums1
> >         nums1 = nums2
> >         nums2 = temp
> >     }
> >     for(const s of nums1){
> >         map.has(s) ? map.set(s, map.get(s) + 1) : map.set(s,1)
> >     }
> >     for(const s of nums2){
> >         if(map.get(s)){
> >             res.push(s)
> >             map.set(s,map.get(s) - 1)
> >         }
> >     }
> >     return res
> > };
> > ```

# ==leetcode-202 快乐数==

> ![image-20220713140036104](images/image-20220713140036104.png)
>
> [202. 快乐数 - 力扣（LeetCode）](https://leetcode.cn/problems/happy-number/)
>
> > 解题思路在于快乐数最终会变成1，而非快乐数会无限循环下去，那么无限循环下去总会出现这一次的平方和和之前某一次的平方和相同的情况，那么依据这个条件来解题
> >
> > total || n这个语句total不存在时=n，存在时=total( || 的短路特性)，将它split为一个数组，用数组的reduce来求平方和(==此处注意要设置initialValue，否则oldValue为arr[0]而newValue为arr[1]，将initialValue设置为0那么第一次循环的oldValue为initialValue为0，newValue为arr[0]==)，然后判断set中是否存在这个total，存在就返回false，不存在就存入set，最外层套while循环条件为total !== 1
> >
> > ```javascript
> > /**
> >  * @param {number} n
> >  * @return {boolean}
> >  */
> > var isHappy = function(n) {
> >     let set = new Set()
> >     let total
> >     while(total !== 1){
> >         let arr = ((total || n) + '').split('')
> >         total = arr.reduce((oldValue, newValue) => {
> >             return oldValue + newValue * newValue
> >         },0)
> >         if(set.has(total)){
> >             return false
> >         }else{
> >             set.add(total)
> >         }
> >     }
> >     return true
> > };
> > ```

# leetcode-1 两数之和

> 见上，应该使用map而非数组、set
>
> 
>
> 本题呢，则要使用map，那么来看一下使用数组和set来做哈希法的局限。
>
> - 数组的大小是受限制的，而且如果元素很少，而哈希值太大会造成内存空间的浪费。
> - set是一个集合，里面放的元素只能是一个key，而两数之和这道题目，不仅要判断y是否存在而且还要记录y的下标位置，因为要返回x 和 y的下标。所以set 也不能用。
>
> 此时就要选择另一种数据结构：map ，map是一种key value的存储结构，可以用key保存数值，用value在保存数值所在的下标。

# leetcode-454 四数相加2

> ![image-20220714120753957](images/image-20220714120753957.png)
>
> [454. 四数相加 II - 力扣（LeetCode）](https://leetcode.cn/problems/4sum-ii/)
>
> > 还是两数相加的思路，只不过第一个数变成了双重for循环遍历nums1和nums2的每个元素相加和，第二个数则是nums3和nums4内每个元素相加和
> >
> > ```javascript
> > /**
> >  * @param {number[]} nums1
> >  * @param {number[]} nums2
> >  * @param {number[]} nums3
> >  * @param {number[]} nums4
> >  * @return {number}
> >  */
> > var fourSumCount = function(nums1, nums2, nums3, nums4) {
> >     let res = 0
> >     const map = new Map()
> >     for(const i of nums1){
> >         for(const j of nums2){
> >             map.has(i + j) ? map.set(i + j, map.get(i + j) + 1) : map.set(i + j, 1)
> >         }
> >     }
> >     for(const i of nums3){
> >         for(const j of nums4){
> >             if(map.has(0 - i - j) && map.get(0 - i - j) !== 0){
> >                 res += map.get(0 - i -j)
> >             }
> >         }
> >     }
> >     return res
> > };
> > ```

# ==leetcode-15 三数之和==

> ![image-20220714133201552](images/image-20220714133201552.png)
>
> [15. 三数之和 - 力扣（LeetCode）](https://leetcode.cn/problems/3sum/)
>
> > 这道题的难点在于答案不可以包含重复的三元组，nums本身是可能存在重复的元素的，而返回值又是一个数组中包含多个数组的形式，对于这种形式的数组进行去重十分麻烦，所以要点就是在将答案push进数组前就做好去重。正因去重的麻烦程度如此之高，所以使用哈希法解题超时概率极高，==本题应采用双指针法并在push答案之前进行去重。==
> >
> > 双指针法的思路：先将数组升序排列，判断nums[0]如果>0则直接返回res(==因为升序如果第一个数大于0那么就不可能存在三数和等于0==)，for循环整个数组，对于每个i都定义left=i+1，right=nums.length-1，然后在left<right的条件下循环判断nums[left] + nums[right]是否等于0 - nums[i]，如果大于0，则需要right--(因为升序排列)，如果小于0，则left++，如果等于0，则将三个数push进res，==此时不能直接break，因为可能出现left和right中间还有两个数的和等于0 - nums[i]的情况==，那么此处需要进一步缩减left和right的范围，此处需要进行去重，因为可能出现nums[left] \=== nums[left + 1]和nums[right] \=== nums[right - 1]的情况，这样根据while循环和if判断的条件会push一个和之前一样的数值的数组进去，所以此处应该两个while循环将nums[left] \=== nums[left + 1]和nums[right] \=== nums[right - 1]这两种情况分别left++和right--，直到left的下一个值与left不重复、right的上一个值与right不重复，这样再left++ right--实现范围缩减就不用担心push进重复的数组
> >
> > 上述过程解决了nums[left]和nums[right]的重复问题，下面解决nums[i]的重复问题，有可能存在nums[i]=\==nums[i+1]并且对应符合条件的left、right不受i的值的影响(==例如[-4,-4,2,2]会返回两个[-4,2,2]==)，因此需要过滤掉i相邻值相同的情况，此处需要注意过滤条件不能是nums[i]=\==nums[i+1]只能是nums[i]=\==nums[i-1]，因为存在[0,0,0,0]的特例，如果用前者条件过滤for循环可能出现最小值作为第一个数进行判定的流程一次都不会完整运行，后者条件可以保证最小数最为第一个数的流程至少完整运行一次
> >
> > ```javascript
> > /**
> >  * @param {number[]} nums
> >  * @return {number[][]}
> >  */
> > var threeSum = function(nums) {
> >     const res = []
> >     nums.sort((a,b) => a - b)
> >     for(let i = 0; i < nums.length; i++){
> >         if(nums[0] > 0){
> >             return res
> >         }
> >         if(nums[i] === nums[i - 1]){
> >             continue
> >         }
> >         let left = i + 1, right = nums.length - 1
> >         while(left < right){
> >             if(nums[i] + nums[left] + nums[right] > 0){
> >                 right--
> >             } 
> >             else if(nums[i] + nums[left] + nums[right] < 0){
> >                 left++
> >             }
> >             else{
> >                 res.push([nums[i], nums[left], nums[right]])
> >                 while(nums[left] === nums[left + 1]){
> >                     left++
> >                 }
> >                 while(nums[right] === nums[right - 1]){
> >                     right--
> >                 }
> >                 left++
> >                 right--
> >             }
> >         }
> >     }
> >     return res
> > };
> > ```

# ==leetcode-18 四数之和==

> ![image-20220714143431099](images/image-20220714143431099.png)
>
> [18. 四数之和 - 力扣（LeetCode）](https://leetcode.cn/problems/4sum/)
>
> > 大体思路和三数之和相同，主要区别是三数之和target确定为0，这里target不确定，在三数之和的代码外套一层for循环求前两个数的和，再两数相加求符合target-前两数和的后两数，相比较三数之和的去重这里的去重更复杂一点，主要是前两数的去重和三数之和的前一数的去重的不同，四数之和需要对第一个数和第二个数都进行相邻值相同情况的去重并且要保证整个流程至少运行一次，那么就不能对第一个和第二个数都是用nums[x] =\== nums[x - 1]的条件判定了，因为这个条件针对第一个数可以保证最小数作为第一个数进行判定的流程至少完整运行一次，但是对于第二个数有可能出现[nums[j],nums[nums.length-1]]区间内的最小数nums[j]作为第二个数进行判定的流程一次都不会完整运行的情况(==例如[2,2,2,2,2] 8==)，所以对于第二个数的去重条件判定应该是j > i + 1 && nums[j] =\== nums[j - 1]，前者保证至少完整运行一次，后者去重(即使是这里也不能用nums[j] =\== nums[j+1]，例如[2,2,2,3,3] 10对于第二个2不满足j > i + 1运行一次，对于第三个2不满足nums[j] === nums[j+1]会运行一次，这样返回两个[2,2,3,3]，去重失败)
> >
> > ```javascript
> > /**
> >  * @param {number[]} nums
> >  * @param {number} target
> >  * @return {number[][]}
> >  */
> > var fourSum = function(nums, target) {
> >     const res = []
> >     nums.sort((a,b) => a-b)
> >     for(let i = 0; i < nums.length; i++){
> >         if(nums[i] === nums[i - 1]){
> >             continue
> >         }
> >         for(let j = i + 1; j < nums.length; j++){
> >             if(j > i + 1 && nums[j] === nums[j - 1]){
> >                 continue
> >             }
> >             let sum = nums[i] + nums[j], left = j + 1, right = nums.length - 1
> >             while(left < right){
> >                 if(nums[left] + nums[right] > target - sum){
> >                     right--
> >                 }
> >                 else if(nums[left] + nums[right] < target - sum){
> >                     left++
> >                 }
> >                 else{
> >                     res.push([nums[i], nums[j], nums[left], nums[right]])
> >                     while(nums[left] === nums[left + 1]){
> >                         left++
> >                     }
> >                     while(nums[right] === nums[right - 1]){
> >                         right--
> >                     }
> >                     left++
> >                     right--
> >                 }
> >             }
> >         }
> >     }
> >     return res
> > };
> > ```

##### 代码随想录-字符串

# leetcode-344 反转字符串

> ![image-20220715122721668](images/image-20220715122721668.png)
>
> [344. 反转字符串 - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-string/)
>
> > 双指针
> >
> > ```javascript
> > /**
> >  * @param {character[]} s
> >  * @return {void} Do not return anything, modify s in-place instead.
> >  */
> > var reverseString = function(s) {
> >     let right = s.length - 1, left = 0
> >     while(left < right){
> >         const temp = s[left]
> >         s[left++] = s[right]
> >         s[right--] = temp
> >     }
> >     return s
> > };
> > ```

# ==leetcode-541 反转字符串2==

> ![image-20220715125244193](images/image-20220715125244193.png)
>
> [541. 反转字符串 II - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-string-ii/)
>
> > 将字符串Array.from成数组再反转，最后返回arr.join('')
> >
> > 双指针外面包for循环，每2k是一个循环，对每个2k的前一个k的字符串反转
> >
> > (==值得注意的是如果最后一个区间范围小于2k且大于k，那么将前k反转其余不动，如果最后一个区间小于k大于0，则将这些字符串全部反转==)(==那么逻辑上如果最后一个区间小于k，应该将最后一个right定义为s.length-1，right = i+k-1>s.length-1 ? s.length - 1 : i+k-1，但是由于最后需要join，join会将数组内的undefined和null元素变成空字符串，那么这里即使最后一个区间小于k right定义为i+k-1也可以实现正确返回值==)
> >
> > ```javascript
> > /**
> >  * @param {string} s
> >  * @param {number} k
> >  * @return {string}
> >  */
> > var reverseStr = function(s, k) {
> >     let arr = Array.from(s)
> >     for(let i = 0; i < arr.length; i += (2 * k)){
> >         let left = i, right = i + k - 1
> >         while(left < right){
> >             const temp = arr[left]
> >             arr[left++] = arr[right]
> >             arr[right--] = temp
> >         }
> >     }
> >     return arr.join('')
> > };
> > ```

# 剑指offer-05 替换空格

> ![image-20220715134114039](images/image-20220715134114039.png)
>
> [剑指 Offer 05. 替换空格 - 力扣（LeetCode）](https://leetcode.cn/problems/ti-huan-kong-ge-lcof/)
>
> > 先统计总共有多少个空格' '，然后将数组从后向前填充，双指针法left = s.length - 1，right = left+2*空格数(==这里是2倍空格数而不是3倍的原因是，空格本身占一个，%20占3个，那么只需要多加2倍就行==)，最后双指针从后向前，碰到空格' '就替换成%20，没碰到空格则arr[left--] = arr[right--]
> >
> > ![image-20220715134252004](images/image-20220715134252004.png)
> >
> > ==这题一定要注意不要写成''而是' '，前者为空字符串，后者为空格字符串space==
> >
> > ```javascript
> > /**
> >  * @param {string} s
> >  * @return {string}
> >  */
> > var replaceSpace = function(s) {
> >     let count = 0
> >     for(const i of s){
> >         if(i === ' '){
> >             count++
> >         }
> >     }
> >     let left = s.length - 1, right = s.length - 1 + 2 * count
> >     let arr = Array.from(s)
> >     while(left < right){
> >         if(arr[left] !== ' '){
> >             arr[right--] = arr[left--]
> >         }else{
> >             left--
> >             arr[right--] = '0'
> >             arr[right--] = '2'
> >             arr[right--] = '%'
> >         }
> >     } 
> >     return arr.join('')
> > };
> > ```

# ==leetcode-151 颠倒字符串中的单词==

> ![image-20220716112849249](images/image-20220716112849249.png)
>
> [151. 颠倒字符串中的单词 - 力扣（LeetCode）](https://leetcode.cn/problems/reverse-words-in-a-string/)
>
> > 使用原生api非常快就可以做出来，但是失去意义
> >
> > 具体思路是先将字符串前面和后面以及中间多余的空格去除掉，再将整个字符串反转，最后将每个单词再反转，即可完成
> >
> > ```javascript
> > /**
> >  * @param {string} s
> >  * @return {string}
> >  */
> > var reverseWords = function(s) {
> >     let arr = Array.from(s), slow = 0, fast = 0
> >     //去掉前面的空格
> >     while(arr[fast] === ' ' && fast < arr.length){
> >         fast++
> >     }
> >     //去掉中间的多余空格和末尾的空格，末尾如果有空格去掉空格后会留下来一个空格
> >     for(; fast < arr.length; fast++){
> >         if(fast - 1 > 0 && arr[fast - 1] === ' ' && arr[fast] === ' '){
> >             continue
> >         }
> >         else{
> >             arr[slow++] = arr[fast]
> >         }
> >     }
> >     //去掉末尾的空格可能存在的一个空格，如果末尾没有空格则arr.length = slow完成整个空格去重步骤
> >     if(arr[slow - 1] === ' '){
> >         arr.length = slow - 1
> >     }else{
> >         arr.length = slow
> >     }
> >     //将整个数组翻转
> >     let left = 0, right = arr.length - 1
> >     while(left < right){
> >         const temp = arr[left]
> >         arr[left++] = arr[right]
> >         arr[right--] = temp
> >     }
> >     //将每个单词翻转
> >     let start = 0
> >     for(let i = 0; i < arr.length; i++){
> >         if(arr[i] === ' ' || i === arr.length - 1){
> >             let end = i === arr.length - 1 ? i : i - 1, left = start, right = end
> >             start = end + 2
> >             while(left < right){
> >                 const temp = arr[left]
> >                 arr[left++] = arr[right]
> >                 arr[right--] = temp
> >             }
> >         }
> >     }
> >     return arr.join('')
> > };
> > ```

# ==剑指offer 58 左旋转字符串==

> ![image-20220716115334440](images/image-20220716115334440.png)
>
> [剑指 Offer 58 - II. 左旋转字符串 - 力扣（LeetCode）](https://leetcode.cn/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)
>
> > 自写，申请了额外空间，不是最优写法
> >
> > ```javascript
> > /**
> >  * @param {string} s
> >  * @param {number} n
> >  * @return {string}
> >  */
> > var reverseLeftWords = function(s, n) {
> >     let arr = Array.from(s)
> >     let m = arr.length
> >     let left = 0, mid = n, right = m
> >     arr.length = m + n
> >     while(mid < arr.length){
> >         if(right < arr.length){
> >             arr[right++] = arr[left]
> >         }
> >         arr[left++] = arr[mid++]
> >     }
> >     arr.length = m
> >     return arr.join('')
> > };
> > ```
> >
> > 代码随想录解法，反转前n子串，再反转剩下子串，最后反转整个字符串即可
> >
> > ```javascript
> > /**
> >  * @param {string} s
> >  * @param {number} n
> >  * @return {string}
> >  */
> > var reverseLeftWords = function(s, n) {
> >     let arr = Array.from(s), left = 0, right = n - 1
> >     while(left < right){
> >         const temp = arr[left]
> >         arr[left++] = arr[right]
> >         arr[right--] = temp
> >     }
> >     left = n, right = arr.length - 1
> >     while(left < right){
> >         const temp = arr[left]
> >         arr[left++] = arr[right]
> >         arr[right--] = temp
> >     }
> >     left = 0, right = arr.length - 1
> >     while(left < right){
> >         const temp = arr[left]
> >         arr[left++] = arr[right]
> >         arr[right--] = temp
> >     }
> >     return arr.join('')
> > };
> > ```

##### 代码随想录-栈与队列

# leetcode-232 用栈实现队列

> ![image-20220718135822669](images/image-20220718135822669.png)
>
> [232. 用栈实现队列 - 力扣（LeetCode）](https://leetcode.cn/problems/implement-queue-using-stacks/)
>
> > ```js
> > var MyQueue = function() {
> >     this.stackIn=[]
> >     this.stackOut=[]
> > };
> > 
> > /** 
> >  * @param {number} x
> >  * @return {void}
> >  */
> > MyQueue.prototype.push = function(x) {
> >     this.stackIn.push(x)
> > };
> > 
> > /**
> >  * @return {number}
> >  */
> > MyQueue.prototype.pop = function() {
> >     if(this.stackOut.length !== 0){
> >         return this.stackOut.pop()
> >     }else{
> >         while(this.stackIn.length){
> >             this.stackOut.push(this.stackIn.pop())
> >         }
> >     }
> >     return this.stackOut.pop()
> > };
> > 
> > /**
> >  * @return {number}
> >  */
> > MyQueue.prototype.peek = function() {
> >     const x = this.pop();
> >     this.stackOut.push(x);
> >     return x;
> > };
> > 
> > /**
> >  * @return {boolean}
> >  */
> > MyQueue.prototype.empty = function() {
> >     return !this.stackIn.length && !this.stackOut.length
> > };
> > 
> > /**
> >  * Your MyQueue object will be instantiated and called as such:
> >  * var obj = new MyQueue()
> >  * obj.push(x)
> >  * var param_2 = obj.pop()
> >  * var param_3 = obj.peek()
> >  * var param_4 = obj.empty()
> >  */
> > ```

# leetcode-225 用队列实现栈

> ![image-20220718135905244](images/image-20220718135905244.png)
>
> [225. 用队列实现栈 - 力扣（LeetCode）](https://leetcode.cn/problems/implement-stack-using-queues/)
>
> > ```js
> > var MyStack = function() {
> >     this.queue = []
> > };
> > 
> > /** 
> >  * @param {number} x
> >  * @return {void}
> >  */
> > MyStack.prototype.push = function(x) {
> >     this.queue.push(x)
> > };
> > 
> > /**
> >  * @return {number}
> >  */
> > MyStack.prototype.pop = function() {
> >     let size = this.queue.length;
> >     while(size-- > 1) {
> >         this.queue.push(this.queue.shift());
> >     }
> >     return this.queue.shift();
> > };
> > 
> > /**
> >  * @return {number}
> >  */
> > MyStack.prototype.top = function() {
> >     const x = this.pop();
> >     this.queue.push(x);
> >     return x;
> > };
> > 
> > /**
> >  * @return {boolean}
> >  */
> > MyStack.prototype.empty = function() {
> >     return !this.queue.length;
> > };
> > 
> > /**
> >  * Your MyStack object will be instantiated and called as such:
> >  * var obj = new MyStack()
> >  * obj.push(x)
> >  * var param_2 = obj.pop()
> >  * var param_3 = obj.top()
> >  * var param_4 = obj.empty()
> >  */
> > ```

# leetcode-20 有效的括号

> 见上，应用栈来解

# leetcode-1047 删除字符串中所有的相邻重复项

> ![image-20220718142458106](images/image-20220718142458106.png)
>
> [1047. 删除字符串中的所有相邻重复项 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/)
>
> > 使用栈模拟，每次push前判断和当前栈的最后一个元素是否相等，相等就pop，不相等就push当前元素，最后数组join即可。
> >
> > ```js
> > /**
> >  * @param {string} s
> >  * @return {string}
> >  */
> > var removeDuplicates = function(s) {
> >     const stack = []
> >     for(const i of s){
> >         if(stack[stack.length - 1] === i){
> >             stack.pop()
> >         }else{
> >             stack.push(i)
> >         }
> >     }
> >     return stack.join('')
> > };
> > ```

# leetcode-150 逆波兰表达式求值

> ![image-20220718171151197](images/image-20220718171151197.png)
>
> [150. 逆波兰表达式求值 - 力扣（LeetCode）](https://leetcode.cn/problems/evaluate-reverse-polish-notation/)
>
> > 分别列出五种情况'+' '-' '/' '*'和字符串类型的数字，分别作出对应的操作，注意/操作是要保留整数部分，所以如果/的结果大于零则floor，小于零则ceil
> >
> > ```js
> > /**
> >  * @param {string[]} tokens
> >  * @return {number}
> >  */
> > var evalRPN = function(tokens) {
> >     const stack = []
> >     for(const i of tokens){
> >         if(i === '+'){
> >             const latter = stack.pop()
> >             const former = stack.pop()
> >             stack.push(former + latter)
> >         }else if(i === '-'){
> >             const latter = stack.pop()
> >             const former = stack.pop()
> >             stack.push(former - latter)
> >         }else if(i === '*'){
> >             const latter = stack.pop()
> >             const former = stack.pop()
> >             stack.push(former * latter)
> >         }else if(i === '/'){
> >             const latter = stack.pop()
> >             const former = stack.pop()
> >             stack.push(former / latter > 0 ? Math.floor(former / latter) : Math.ceil(former / latter))
> >         }else{
> >             stack.push(i - 0)
> >         }
> >     }
> >     return stack[0]
> > };
> > ```
> >
> > 用哈希表存储
> >
> > ```js
> > /**
> >  * @param {string[]} tokens
> >  * @return {number}
> >  */
> > var evalRPN = function(tokens) {
> >     const s = new Map([
> >         ["+", (a, b) => a * 1  + b * 1],
> >         ["-", (a, b) => b - a],
> >         ["*", (a, b) => b * a],
> >         ["/", (a, b) => (b / a) | 0]
> >     ]);
> >     const stack = [];
> >     for (const i of tokens) {
> >         if(!s.has(i)) {
> >             stack.push(i);
> >             continue;
> >         }
> >         stack.push(s.get(i)(stack.pop(),stack.pop()))
> >     }
> >     return stack.pop();
> > };
> > ```

# leetcode-239 滑动窗口最大值

> ![image-20220718220455754](images/image-20220718220455754.png)
>
> [239. 滑动窗口最大值 - 力扣（LeetCode）](https://leetcode.cn/problems/sliding-window-maximum/)
>
> > 对于滑动窗口这个数组，只需要维护有可能成为最大值的数即可，即从左到右每次push值时，都和窗口数组内末尾的值while循环比较，末尾值比它小时pop()，直到末尾值比它大，或者窗口数组内没有值了再push，这样窗口数组内只剩可能成为最大值的值，那么在每次右移过程中，都要while循环判断处在窗口数组前列的值是否还在窗口范围内来进行shift()(==因为要判断是否还在窗口内所以stack内不能直接存储值只能存储值在nums中的下标==)，然后再将窗口数组内的第一个值(==也就是当前窗口范围内最大值==)push进res即可
> >
> > [单调队列解LeetCode239_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1d7411R7xo?spm_id_from=333.880.my_history.page.click&vd_source=9de430adabffb3f1879c1eb17ba0f461) 4：00
> >
> > ```js
> > /**
> >  * @param {number[]} nums
> >  * @param {number} k
> >  * @return {number[]}
> >  */
> > var maxSlidingWindow = function(nums, k) {
> >     const res = [], stack = []
> >     for(let i = 0; i < k; i++){
> >         while(stack.length && nums[i] >= nums[stack[stack.length - 1]]){
> >             stack.pop()
> >         }
> >             stack.push(i)
> >     }
> >     res.push(nums[stack[0]])
> >     for(let i = k; i < nums.length; i++){
> >         while(stack.length && nums[i] >= nums[stack[stack.length - 1]]){
> >             stack.pop()
> >         }
> >         stack.push(i)
> >         while(stack[0] <= i - k){
> >             stack.shift()
> >         }
> >         res.push(nums[stack[0]])
> >     }
> >     return res
> > };
> > ```

# leetcode-347 前k个高频元素

> ![image-20220719175628730](images/image-20220719175628730.png)
>
> [347. 前 K 个高频元素 - 力扣（LeetCode）](https://leetcode.cn/problems/top-k-frequent-elements/)
>
> > ```js
> > /**
> >  * @param {number[]} nums
> >  * @param {number} k
> >  * @return {number[]}
> >  */
> >   var topKFrequent = function(nums, k) {
> >     const countMap = new Map();
> >     for (let num of nums) {
> >         countMap.set(num, (countMap.get(num) || 0) + 1);
> >     }
> >     return Array.from(countMap)
> >         .sort((a, b) => b[1] - a[1])
> >         .slice(0, k)
> >         .map(i => i[0]);
> > };
> > ```

##### 代码随想录-二叉树

递归三部曲

> 确定递归参数和返回值
>
> 确定终止条件
>
> 确定单层循环的逻辑

# leetcode-144 二叉树的前序遍历

> ![image-20220719185600983](images/image-20220719185600983.png)
>
> ![image-20220719185614491](images/image-20220719185614491.png)
>
> [144. 二叉树的前序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-preorder-traversal/)
>
> > 递归
> >
> > ```js
> > /**
> >  * Definition for a binary tree node.
> >  * function TreeNode(val, left, right) {
> >  *     this.val = (val===undefined ? 0 : val)
> >  *     this.left = (left===undefined ? null : left)
> >  *     this.right = (right===undefined ? null : right)
> >  * }
> >  */
> > /**
> >  * @param {TreeNode} root
> >  * @return {number[]}
> >  */
> > var preorderTraversal = function(root) {
> >     const res = []
> >     const dfs = (root) => {
> >         if(root === null){
> >             return res
> >         }
> >         res.push(root.val)
> >         dfs(root.left)
> >         dfs(root.right)
> >     }
> >     dfs(root)
> >     return res
> > };
> > ```
> >
> > 迭代，思路：因为前序遍历是中左右(==入栈元素和处理元素是同一个元素==)，所以一上来就可以将root放进stack，然后while循环stack.length，每次循环都stack.pop()然后弹出的val push进res，再将cur.right push进stack，再将cur.left push进stack，这样反着来的原因是stack栈是先进后出的，为了达到前序遍历中左右的效果需要right比left先入栈
> >
> > ```js
> > /**
> >  * Definition for a binary tree node.
> >  * function TreeNode(val, left, right) {
> >  *     this.val = (val===undefined ? 0 : val)
> >  *     this.left = (left===undefined ? null : left)
> >  *     this.right = (right===undefined ? null : right)
> >  * }
> >  */
> > /**
> >  * @param {TreeNode} root
> >  * @return {number[]}
> >  */
> > var preorderTraversal = function(root) {
> >     const res = [], stack = []
> >     if(root === null){
> >         return res
> >     }
> >     stack.push(root)
> >     let cur = null
> >     while(stack.length){
> >         cur =stack.pop()
> >         res.push(cur.val)
> >         if(cur.right){
> >             stack.push(cur.right)
> >         }
> >         //注意这里先push右节点再push左节点是因为栈是先进后出的，这样要要反过来才能达到前序遍历中左右的效果
> >         if(cur.left){
> >             stack.push(cur.left)
> >         }
> >     }
> >     return res
> > };
> > ```
> >

# leetcode-94 二叉树的中序遍历

> ![image-20220719190239972](images/image-20220719190239972.png)
>
> [94. 二叉树的中序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-inorder-traversal/)
>
> > 递归
> >
> > ```js
> > /**
> >  * Definition for a binary tree node.
> >  * function TreeNode(val, left, right) {
> >  *     this.val = (val===undefined ? 0 : val)
> >  *     this.left = (left===undefined ? null : left)
> >  *     this.right = (right===undefined ? null : right)
> >  * }
> >  */
> > /**
> >  * @param {TreeNode} root
> >  * @return {number[]}
> >  */
> > var inorderTraversal = function(root) {
> >     const res = []
> >     const dfs = (root) => {
> >         if(root === null){
> >             return res
> >         }
> >         dfs(root.left)
> >         res.push(root.val)
> >         dfs(root.right)
> >     }
> >     dfs(root)
> >     return res
> > };
> > ```
> >
> > 迭代，思路：中序遍历是左中右(==入栈元素和处理元素不是同一个元素==)，先将cur指向root，while循环stack.length || cur然后if(cur)下push cur，push之后将cur指向cur.left，这样入栈的顺序就是root然后root.left、root.left.left这样循环到底，直到左孩子为空的节点，此时触发else，else中stack.pop()弹出的val push进res，push后cur指向cur.right(==这样无论左孩子为空的节点有没有右孩子都能实现左中右==)
> >
> > ```js
> > /**
> >  * Definition for a binary tree node.
> >  * function TreeNode(val, left, right) {
> >  *     this.val = (val===undefined ? 0 : val)
> >  *     this.left = (left===undefined ? null : left)
> >  *     this.right = (right===undefined ? null : right)
> >  * }
> >  */
> > /**
> >  * @param {TreeNode} root
> >  * @return {number[]}
> >  */
> > var inorderTraversal = function(root) {
> >     const res = [], stack = []
> >     let cur = root
> >     while(stack.length || cur){
> >         if(cur){
> >             stack.push(cur)
> >             cur = cur.left
> >         }else{
> >             cur = stack.pop()
> >             res.push(cur.val)
> >             cur = cur.right
> >         }
> >     }
> >     return res
> > };
> > ```
> >

# leetcode-145 二叉树的后序遍历

> ![image-20220719185957748](images/image-20220719185957748.png)
>
> [145. 二叉树的后序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-postorder-traversal/)
>
> > 递归
> >
> > ```js
> > /**
> >  * Definition for a binary tree node.
> >  * function TreeNode(val, left, right) {
> >  *     this.val = (val===undefined ? 0 : val)
> >  *     this.left = (left===undefined ? null : left)
> >  *     this.right = (right===undefined ? null : right)
> >  * }
> >  */
> > /**
> >  * @param {TreeNode} root
> >  * @return {number[]}
> >  */
> > var postorderTraversal = function(root) {
> >     const res = []
> >     const dfs = (root) => {
> >         if(root === null){
> >             return res
> >         }
> >         dfs(root.left)
> >         dfs(root.right)
> >         res.push(root.val)
> >     }
> >     dfs(root)
> >     return res
> > };
> > ```
> >
> > 迭代，后序遍历就是入栈的顺序和前序相反(==前序中左右入栈顺序反转 => 中右左==)
> >
> > 最后res翻转一次(==中右左 => 左右中 即为后序==)即可
> >
> > ```js
> > /**
> >  * Definition for a binary tree node.
> >  * function TreeNode(val, left, right) {
> >  *     this.val = (val===undefined ? 0 : val)
> >  *     this.left = (left===undefined ? null : left)
> >  *     this.right = (right===undefined ? null : right)
> >  * }
> >  */
> > /**
> >  * @param {TreeNode} root
> >  * @return {number[]}
> >  */
> > var postorderTraversal = function(root) {
> >     const res = [], stack = [root]
> >     if(root === null){
> >         return res
> >     }
> >     let cur = null
> >     while(stack.length){
> >         cur =stack.pop()
> >         res.push(cur.val)
> >         if(cur.left){
> >             stack.push(cur.left)
> >         }
> >         if(cur.right){
> >             stack.push(cur.right)
> >         }
> >     }
> >     return res.reverse()
> > };
> > ```
> >

# 前中后序遍历统一风格迭代法见代码随想录网站

# leetcode-102 二叉树的层序遍历

> ![image-20220720111325830](images/image-20220720111325830.png)
>
> [102. 二叉树的层序遍历 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-level-order-traversal/)
>
> > 需要注意每一层的节点值要作为组合成一个数组push进res，将节点值push进resLevel时不要忘记了.val
> >
> > ```js
> > /**
> >  * Definition for a binary tree node.
> >  * function TreeNode(val, left, right) {
> >  *     this.val = (val===undefined ? 0 : val)
> >  *     this.left = (left===undefined ? null : left)
> >  *     this.right = (right===undefined ? null : right)
> >  * }
> >  */
> > /**
> >  * @param {TreeNode} root
> >  * @return {number[][]}
> >  */
> > var levelOrder = function(root) {
> >     const res = [], queue = []
> >     let cur = null
> >     queue.push(root)
> >     if(root === null){
> >         return res
> >     }
> >     while(queue.length){
> >         const resLevel = []
> >         const len = queue.length
> >         for(let i = 0; i < len; i++){
> >             resLevel.push(queue[0].val)
> >             cur = queue.shift()
> >             if(cur.left){
> >                 queue.push(cur.left)
> >             }
> >             if(cur.right){
> >                 queue.push(cur.right)
> >             }
> >         }
> >         res.push(resLevel)
> >     }
> >     return res
> > };
> > ```

# leetcode-107 二叉树的层序遍历2

> 是102题的结果的翻转
>
> res.push(resLevel)换成res.unshift(resLevel)就行

# leetcode-199 二叉树的右视图

> ![image-20220911151837266](images/image-20220911151837266.png)
>
> [199. 二叉树的右视图 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-right-side-view/)
>
> > 和102题一样的层序遍历，只是将每一层最右边的节点push进res
> >
> > ```js
> > /**
> >  * Definition for a binary tree node.
> >  * function TreeNode(val, left, right) {
> >  *     this.val = (val===undefined ? 0 : val)
> >  *     this.left = (left===undefined ? null : left)
> >  *     this.right = (right===undefined ? null : right)
> >  * }
> >  */
> > /**
> >  * @param {TreeNode} root
> >  * @return {number[]}
> >  */
> > var rightSideView = function(root) {
> >     const res = [], queue = []
> >     let cur = null
> >     queue.push(root)
> >     if(root === null){
> >         return res
> >     }
> >     while(queue.length){
> >         const len = queue.length
> >         for(let i = 0; i < len; i++){
> >             if(i === len - 1){
> >                 res.push(queue[0].val)
> >             }
> >             cur = queue.shift()
> >             if(cur.left){
> >                 queue.push(cur.left)
> >             }
> >             if(cur.right){
> >                 queue.push(cur.right)
> >             }
> >         }
> >     }
> >     return res
> > };
> > ```

......

还有很多102二叉树层序遍历的变种题目，这里不做赘述

# leetcode-226 翻转二叉树

> ![image-20220720121159923](images/image-20220720121159923.png)
>
> [226. 翻转二叉树 - 力扣（LeetCode）](https://leetcode.cn/problems/invert-binary-tree/)
>
> > 递归法+前序遍历
> >
> > 此题核心思想是将每个节点的左右孩子翻转即可
> >
> > ```js
> > /**
> >  * Definition for a binary tree node.
> >  * function TreeNode(val, left, right) {
> >  *     this.val = (val===undefined ? 0 : val)
> >  *     this.left = (left===undefined ? null : left)
> >  *     this.right = (right===undefined ? null : right)
> >  * }
> >  */
> > /**
> >  * @param {TreeNode} root
> >  * @return {TreeNode}
> >  */
> > var invertTree = function(root) {
> >     const inverNode = (left, right) => {
> >         const temp = left
> >         left = right
> >         right = temp
> >         root.left = left
> >         root.right = right
> >     }
> >     if(root===null){
> >         return root;
> >     }
> >     //确定节点处理逻辑 交换
> >     inverNode(root.left,root.right);
> >     invertTree(root.left);
> >     invertTree(root.right);
> >     return root;
> > };
> > ```
> >
> > 迭代法+前序遍历和迭代法+层序遍历见代码随想录

# leetcode-101 对称二叉树

> ![image-20220720125928712](images/image-20220720125928712.png)
>
> ![image-20220720125938869](images/image-20220720125938869.png)
>
> [101. 对称二叉树 - 力扣（LeetCode）](https://leetcode.cn/problems/symmetric-tree/)
>
> > 递归，需要注意的是递归中终止条件的设定，此题一共有如下几种情况，分别是：
> >
> > 左孩子空，右孩子不空和左孩子不空，右孩子空
> >
> > 左右孩子都空
> >
> > 左右孩子都不空但数值不等
> >
> > 左右孩子都不空且数值相等
> >
> > ==需要注意的是最后一个情况左右孩子都不空且数值相等既不能用来判定true也不能用来判断false==，那么判定true的只有左右孩子都为空的时候，因为递归时只要遇到第一和第三个条件就会返回false，如果递归中除了叶子节点外的节点都满足左右节点不空且数值相等那么最后就会出现叶子节点的左右节点都为空的情况，也就是第二个条件为true
> >
> > ```js
> > /**
> >  * Definition for a binary tree node.
> >  * function TreeNode(val, left, right) {
> >  *     this.val = (val===undefined ? 0 : val)
> >  *     this.left = (left===undefined ? null : left)
> >  *     this.right = (right===undefined ? null : right)
> >  * }
> >  */
> > /**
> >  * @param {TreeNode} root
> >  * @return {boolean}
> >  */
> > var isSymmetric = function(root) {
> >     const compareNode = (left, right) => {
> >         if(left === null && right !== null || left !== null && right === null){
> >             return false
> >         }else if(left === null && right === null){
> >             return true
> >         }else if(left.val !== right.val){
> >             return false
> >         }
> >         let outside = compareNode(left.left, right.right)
> >         let inside = compareNode(left.right, right.left)
> >         return outside&&inside
> >     }
> >     if(root === null){
> >         return true
> >     }
> >     return compareNode(root.left, root.right)
> > };
> > ```
> >
> > 迭代+队列实现见代码随想录

# leetcode-104 二叉树的最大深度

> ![image-20220726114840081](images/image-20220726114840081.png)
>
> [104. 二叉树的最大深度 - 力扣（LeetCode）](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)
>
> > 递归+后序遍历(==先maxDepth(root.left)再maxDepth(root.right)然后再递归遍历root，顺序是左右中==)
> >
> > 只要传入的root不为null那么就+1
> >
> > ```js
> > /**
> >  * Definition for a binary tree node.
> >  * function TreeNode(val, left, right) {
> >  *     this.val = (val===undefined ? 0 : val)
> >  *     this.left = (left===undefined ? null : left)
> >  *     this.right = (right===undefined ? null : right)
> >  * }
> >  */
> > /**
> >  * @param {TreeNode} root
> >  * @return {number}
> >  */
> > var maxDepth = function(root) {
> >     if (root === null) return 0;
> >     return 1 + Math.max(maxDepth(root.left), maxDepth(root.right))
> > };
> > ```
> >
> > 迭代+层序遍历
> >
> > 维护一个数组进行层序遍历，将数组前一层的数组全部shift出去，每次shift都将shift出去的节点的left和right push进数组，这样直到数组内没有元素即遍历完毕
> >
> > ```js
> > /**
> >  * Definition for a binary tree node.
> >  * function TreeNode(val, left, right) {
> >  *     this.val = (val===undefined ? 0 : val)
> >  *     this.left = (left===undefined ? null : left)
> >  *     this.right = (right===undefined ? null : right)
> >  * }
> >  */
> > /**
> >  * @param {TreeNode} root
> >  * @return {number}
> >  */
> > var maxDepth = function(root) {
> >     if(root === null){
> >         return 0
> >     }
> >     const queue = [root]
> >     let res = 0
> >     while(queue.length){
> >         res++
> >         let len = queue.length
> >         while(len--){
> >             const node = queue.shift()
> >             if(node.left){
> >                 queue.push(node.left)
> >             }
> >             if(node.right){
> >                 queue.push(node.right)
> >             }
> >         }
> >     }
> >     return res
> > };
> > ```

# leetcode-559 n叉树的最大深度

> ![image-20220726130839780](images/image-20220726130839780.png)
>
> ![image-20220726130851787](images/image-20220726130851787.png)
>
> [559. N 叉树的最大深度 - 力扣（LeetCode）](https://leetcode.cn/problems/maximum-depth-of-n-ary-tree/)
>
> > 递归，思路和上一题递归大致相同
> >
> > ```js
> > /**
> >  * // Definition for a Node.
> >  * function Node(val,children) {
> >  *    this.val = val;
> >  *    this.children = children;
> >  * };
> >  */
> > 
> > /**
> >  * @param {Node|null} root
> >  * @return {number}
> >  */
> > var maxDepth = function(root) {
> >     if(root === null){
> >         return 0
> >     }
> >     let depth = 0
> >     for(const child of root.children){
> >         depth = Math.max(depth, maxDepth(child))
> >     }
> >     return depth + 1
> > };
> > ```
> >
> > 迭代，思路和上一题迭代大致相同
> >
> > ```js
> > /**
> >  * // Definition for a Node.
> >  * function Node(val,children) {
> >  *    this.val = val;
> >  *    this.children = children;
> >  * };
> >  */
> > 
> > /**
> >  * @param {Node|null} root
> >  * @return {number}
> >  */
> > var maxDepth = function(root) {
> >     if(root === null){
> >         return 0
> >     }
> >     const queue = [root]
> >     let count = 0
> >     while(queue.length){
> >         count++
> >         let len = queue.length
> >         while(len--){
> >             const node = queue.shift()
> >             for(const child of node.children){
> >                 queue.push(child)
> >             }
> >         }
> >     }
> >     return count
> > };
> > ```

# leetcode-111 二叉树的最小深度

> ![image-20220726141931079](images/image-20220726141931079.png)
>
> [111. 二叉树的最小深度 - 力扣（LeetCode）](https://leetcode.cn/problems/minimum-depth-of-binary-tree/)
>
> > 递归
> >
> > ==需要注意的是求最小深度和最大深度是有区别的，不是单纯将Math.max换成Math.min就可以的==
> >
> > ![111.二叉树的最小深度](images/20210203155800503.png)
> >
> > ==所以需要增加判定条件，不能只是root =\== null，还需要分别判定左节点为空右节点不为空、左节点不为空右节点为空和左右节点都为空这三种情况，这里还有一个小坑就是左右节点都为空的情况要先判定，因为这种情况是其他两种情况的交集==
> >
> > ```js
> > /**
> >  * Definition for a binary tree node.
> >  * function TreeNode(val, left, right) {
> >  *     this.val = (val===undefined ? 0 : val)
> >  *     this.left = (left===undefined ? null : left)
> >  *     this.right = (right===undefined ? null : right)
> >  * }
> >  */
> > /**
> >  * @param {TreeNode} root
> >  * @return {number}
> >  */
> > var minDepth = function(root) {
> >     if(root === null){
> >         return 0
> >     }
> >     if(root.left === null && root.right ===null){
> >         return 1
> >     }
> >     if(root.left === null){
> >         return 1 + minDepth(root.right)
> >     }
> >     if(root.right === null){
> >         return 1 + minDepth(root.left)
> >     }
> >     return 1 + Math.min(minDepth(root.left), minDepth(root.right))
> > };
> > ```
> >
> > 迭代
> >
> > ```js
> > /**
> >  * Definition for a binary tree node.
> >  * function TreeNode(val, left, right) {
> >  *     this.val = (val===undefined ? 0 : val)
> >  *     this.left = (left===undefined ? null : left)
> >  *     this.right = (right===undefined ? null : right)
> >  * }
> >  */
> > /**
> >  * @param {TreeNode} root
> >  * @return {number}
> >  */
> > var minDepth = function(root) {
> >     if(root === null){
> >         return 0
> >     }
> >     const queue = [root]
> >     let count = 0
> >     while(queue.length){
> >         count++
> >         let len = queue.length
> >         while(len--){
> >             const node = queue.shift()
> >             if(node.left){
> >                 queue.push(node.left)
> >             }
> >             if(node.right){
> >                 queue.push(node.right)
> >             }
> >             if(!node.left && !node.right){
> >                 return count
> >             }
> >         }
> >     }
> > };
> > ```

# leetcode-222 完全二叉树的节点个数

> ![image-20220731122800357](images/image-20220731122800357.png)
>
> [222. 完全二叉树的节点个数 - 力扣（LeetCode）](https://leetcode.cn/problems/count-complete-tree-nodes/)
>
> > 普通二叉树
> >
> > 递归+层序遍历
> >
> > ```js
> > /**
> >  * Definition for a binary tree node.
> >  * function TreeNode(val, left, right) {
> >  *     this.val = (val===undefined ? 0 : val)
> >  *     this.left = (left===undefined ? null : left)
> >  *     this.right = (right===undefined ? null : right)
> >  * }
> >  */
> > /**
> >  * @param {TreeNode} root
> >  * @return {number}
> >  */
> > var countNodes = function(root) {
> >     const getNodeSum = (root) => {
> >         if(root === null){
> >             return 0
> >         }
> >         let leftNodeSum = getNodeSum(root.left)
> >         let rightNodeSum = getNodeSum(root.right)
> >         return 1 + leftNodeSum + rightNodeSum
> >     }
> >     return getNodeSum(root)
> > };
> > ```
> >
> > 普通二叉树
> >
> > 迭代+层序遍历(==不知道为什么用时很长==)
> >
> > ```js
> > var countNodes = function(root) {
> >     if(root === null){
> >         return 0
> >     }
> >     const queue = [root]
> >     let sum = 0
> >     while(queue.length){
> >         let len = queue.length
> >         while(len--){
> >             sum++
> >             const node = queue.shift()
> >             if(node.left){
> >                 queue.push(node.left)
> >             }
> >             if(node.right){
> >                 queue.push(node.right)
> >             }
> >         }
> >     }
> >     return sum
> > };
> > ```
> >
> > 利用完全二叉树特点
> >
> > 递归
> >
> > 完全二叉树有两种情况，一是满二叉树，而是叶子节点不满，情况一可以直接用2^树深度-1计算，情况二则需要分别递归左孩子，和右孩子，递归到某一深度一定会有左孩子或者右孩子为满二叉树，然后依然可以按照情况1来计算。
> >
> > ![222.完全二叉树的节点个数](images/20201124092543662.png)
> >
> > ![222.完全二叉树的节点个数1](images/20201124092634138.png)
> >
> > ```js
> > var countNodes = function(root) {
> >     //利用完全二叉树的特点
> >     if(root===null){
> >         return 0;
> >     }
> >     let left=root.left;
> >     let right=root.right;
> >     let leftHeight=0,rightHeight=0;
> >     while(left){
> >         left=left.left;
> >         leftHeight++;
> >     }
> >     while(right){
> >         right=right.right;
> >         rightHeight++;
> >     }
> >     if(leftHeight==rightHeight){
> >         return Math.pow(2,leftHeight+1)-1;
> >     }
> >     return countNodes(root.left)+countNodes(root.right)+1;
> > };
> > ```

# leetcode-110 平衡二叉树

> ![image-20220731131555855](images/image-20220731131555855.png)
>
> ![image-20220731131610550](images/image-20220731131610550.png)
>
> [110. 平衡二叉树 - 力扣（LeetCode）](https://leetcode.cn/problems/balanced-binary-tree/)
>
> > 递归+前序遍历
> >
> > 当遍历到的当前节点为根节点的二叉树不是平衡二叉树的话返回-1，如果有一个节点为根节点的二叉树的高度返回是-1，那么这整颗二叉树就不是平衡二叉树，那么就一直返回-1直到返回给root，如果是平衡二叉树则返回高度，最后递归得到输入root的高度。最后判断root的高度是否为-1，是则返回false，否则返回true
> >
> > ```js
> > /**
> >  * Definition for a binary tree node.
> >  * function TreeNode(val, left, right) {
> >  *     this.val = (val===undefined ? 0 : val)
> >  *     this.left = (left===undefined ? null : left)
> >  *     this.right = (right===undefined ? null : right)
> >  * }
> >  */
> > /**
> >  * @param {TreeNode} root
> >  * @return {boolean}
> >  */
> > var isBalanced = function(root) {
> >     const getDepth = (root) => {
> >         if(root === null){
> >             return 0
> >         }
> >         let leftDepth = getDepth(root.left)
> >         if(leftDepth === -1){
> >             return -1
> >         }
> >         let rightDepth = getDepth(root.right)
> >         if(rightDepth === -1){
> >             return -1
> >         }
> >         if(Math.abs(leftDepth - rightDepth) > 1){
> >             return -1
> >         }else{
> >             return Math.max(leftDepth, rightDepth) + 1
> >         }
> >     }
> >     return getDepth(root) !== -1
> > };
> > ```
> >
> > 迭代法较为麻烦

# leetcode-257 二叉树的所有路径

> ![image-20220731145135063](images/image-20220731145135063.png)
>
> [257. 二叉树的所有路径 - 力扣（LeetCode）](https://leetcode.cn/problems/binary-tree-paths/)
>
> > 递归
> >
> > ```js
> > /**
> >  * Definition for a binary tree node.
> >  * function TreeNode(val, left, right) {
> >  *     this.val = (val===undefined ? 0 : val)
> >  *     this.left = (left===undefined ? null : left)
> >  *     this.right = (right===undefined ? null : right)
> >  * }
> >  */
> > /**
> >  * @param {TreeNode} root
> >  * @return {string[]}
> >  */
> > var binaryTreePaths = function(root) {
> >     const res = []
> >     const getPath = (root, curPath) => {
> >         if(root.left === null && root.right === null){
> >             curPath += root.val
> >             res.push(curPath)
> >             return
> >         }
> >         curPath += root.val + '->'
> >         if(root.left){
> >             getPath(root.left, curPath)
> >         }
> >         if(root.right){
> >             getPath(root.right, curPath)
> >         }
> >     }
> >     getPath(root, '')
> >     return res
> > };
> > ```
> >
> > 迭代见代码随想录

##### 代码随想录-回溯算法

回溯算法要将逻辑抽象成树状解构来分析

![回溯算法理论基础](images/20210130173631174.png)

==回溯三部曲==

> 递归参数
>
> 终止条件
>
> 单层搜索逻辑

==回溯算法模板==

> ```js
> const res = [], path = []
> function backtracking(参数) {
>  if (终止条件) {
>      存放结果;
>      return;
>  }
> 
>  for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
>      path.push(xxx)
>      //处理节点;
>      backtracking(路径，选择列表); // 递归
>      path.pop()
>      //回溯，撤销处理结果
>  }
> }
> ```

==回溯的树层去重普通模板==

> 先将原输入数组进行升序排列，再在for循环中加入if判断i > startIndex(==此举是为了避免进行树枝去重==)&& nums[i] =\== nums\[i - 1](==因为已经升序所以用相邻元素是否相等进行去重==)

# leetcode-77 组合

> ![image-20220721134818545](images/image-20220721134818545.png)
>
> [77. 组合 - 力扣（LeetCode）](https://leetcode.cn/problems/combinations/)
>
> > 回溯算法
> >
> > ```js
> > /**
> >  * @param {number} n
> >  * @param {number} k
> >  * @return {number[][]}
> >  */
> > var combine = function(n, k) {
> > const combineHelper = (n, k, startIndex) => {
> >     if (path.length === k) {
> >         result.push([...path])
> >         return
> >         //这里的return仅用于结束循环进行回溯
> >     }
> >     for (let i = startIndex; i <= n - (k - path.length) + 1; i++) {
> >         path.push(i)
> >         // console.log(path)
> >         combineHelper(n, k, i + 1)
> >         path.pop()
> >         // console.log(path)
> >     }
> > }
> >     let result = []
> >     let path = []
> >     combineHelper(n, k, 1)
> >     return result
> > };
> > ```

# leetcode-216 组合总和3

> ![image-20220721155311568](images/image-20220721155311568.png)
>
> [216. 组合总和 III - 力扣（LeetCode）](https://leetcode.cn/problems/combination-sum-iii/)
>
> > 回溯
> >
> > ```js
> > /**
> >  * @param {number} k
> >  * @param {number} n
> >  * @return {number[][]}
> >  */
> > var combinationSum3 = function(k, n) {
> >     const combine = (startIndex) => {
> >         const l = path.length;
> >         if (l === k) {
> >             const sum = path.reduce((a, b) => a + b);
> >             if (sum === n) {
> >                 res.push([...path]);
> >             }
> >             return;
> >         }
> >         for(let j = startIndex; j <= 9 - (k - l) + 1; j++){
> >             path.push(j)
> >             combine(j + 1)
> >             path.pop()
> >         }
> >     }
> >     let path =[], res = []
> >     combine(1)
> >     return res
> > };
> > ```

# leetcode-17电话号码的字母组合

> ![image-20220722142147699](images/image-20220722142147699.png)
>
> [17. 电话号码的字母组合 - 力扣（LeetCode）](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)
>
> > 回溯，这道题目和前面区别在于不是在同一数组中进行取值，那么对应for循环中不能从startIndex开始，需要从0开始(==因为不是从同一数组开始，所以每个数组都要从0下标开始==)
> >
> > ```js
> > /**
> >  * @param {string} digits
> >  * @return {string[]}
> >  */
> > var letterCombinations = function(digits) {
> >     const map = ['', '', 'abc', 'def', 'ghi', 'jkl', 'mno', 'pqrs', 'tuv', 'wxyz']
> >     const k = digits.length
> >     const res = [], path = []
> >     if(!k){
> >         return []
> >     }
> >     if(k === 1){
> >         return map[digits].split('')
> >     }
> >     const backtracking = (digits, k, a) => {
> >         if(path.length === k){
> >             res.push(path.join(''))
> >             return
> >         }
> >         for(let i = 0; i < map[digits[a]].length; i++){
> >             path.push(map[digits[a]][i])
> >             backtracking(digits, k, a + 1)
> >             path.pop()
> >         }
> >     }
> >     backtracking(digits, k, 0)
> >     return res
> > };
> > ```

# leetcode-39 组合总和

> ![image-20220723002951128](images/image-20220723002951128.png)
>
> [39. 组合总和 - 力扣（LeetCode）](https://leetcode.cn/problems/combination-sum/)
>
> > 回溯
> >
> > 这道题与前面题目的区别在于数组中的值可以重复选择，所以对应的在回溯时==不需要再加一==，即可达到重复选择的效果
> >
> > 因为在for循环外进行sum操作会因为path初始化为空而报错，所以转到for循环内进行求和，那么就需要将sum作为形参传入下一个循环以便进行终止条件判定，终止条件有两个分别是===target和>target，对应push和return空
> >
> > ```js
> > /**
> >  * @param {number[]} candidates
> >  * @param {number} target
> >  * @return {number[][]}
> >  */
> > var combinationSum = function(candidates, target) {
> >     const res = [], path = []
> >     const backtracking = (startIndex, sum) => {
> >         if(sum === target){
> >             res.push([...path])
> >             return
> >         }
> >         if(sum > target){
> >             return
> >         }
> >         for(let i = startIndex; i < candidates.length; i++){
> >             path.push(candidates[i])
> >             sum += candidates[i]
> >             backtracking(i, sum)
> >             path.pop()
> >             sum -= candidates[i]
> >         }
> >     }
> >     backtracking(0, 0)
> >     return res
> > };
> > ```

# ==leetcode-40 组合总和2==

> ![image-20220723011247658](images/image-20220723011247658.png)
>
> [40. 组合总和 II - 力扣（LeetCode）](https://leetcode.cn/problems/combination-sum-ii/)
>
> > 需要理解到底是哪里要去重，那么很明显不是数组内去重，而是数组和数组之间去重，那么也就是说是树的水平方向去重，树层去重，而不是竖直方向去重，不是树枝去重
> >
> > ![40.组合总和II1](images/20201123202817973.png)
> >
> > 想要实现去重需要深入了解backtracking内for循环的机制和实现的顺序(==建议在草稿上把整个流程列出来==)，那么想要实现树层去重就要在for内进行，先在外面将candidates进行升序排列，那么在for内判定i>startIndex(==这样可以避免进行树枝去重，如果不加这个条件那么会出现将(1,1,6)这样的情况排除在外，会出现在第2层及以上层在首次for循环直接结束循环==)以及candidates[i] =\== candidates[i - 1] (==因为已经升序排列，所以用判定是否相邻元素相同来去重==)
> >
> > ```js
> > /**
> >  * @param {number[]} candidates
> >  * @param {number} target
> >  * @return {number[][]}
> >  */
> > var combinationSum2 = function(candidates, target) {
> >     const res = [], path = []
> >     candidates.sort((a, b) => a - b)
> >     const backtracking = (startIndex, sum) => {
> >         if(sum === target){
> >             res.push([...path])
> >             return
> >         }
> >         if(sum > target){
> >             return
> >         }
> >         for(let i = startIndex; i < candidates.length; i++){
> >             if(i > startIndex && candidates[i] === candidates[i - 1]){
> >                 continue
> >             }
> >             path.push(candidates[i])
> >             sum += candidates[i]
> >             backtracking(i + 1, sum)
> >             path.pop()
> >             sum -= candidates[i]
> >         }
> >     }
> >     backtracking(0, 0)
> >     return res
> > };
> > ```

# ==leetcode-131 分割回文串==

> ![image-20220727124404718](images/image-20220727124404718.png)
>
> [131. 分割回文串 - 力扣（LeetCode）](https://leetcode.cn/problems/palindrome-partitioning/)
>
> > 回溯
> >
> > ![131.分割回文串](images/131.%E5%88%86%E5%89%B2%E5%9B%9E%E6%96%87%E4%B8%B2.jpg)
> >
> > 将startIndex作为分割线
> >
> > ```js
> > /**
> >  * @param {string} s
> >  * @return {string[][]}
> >  */
> > var partition = function(s) {
> >     const res = [], path = []
> >     const isPalindrome = (s, startIndex, i) => {
> >         for(let left = startIndex, right = i; left < right; left++, right--){
> >             if(s[left] !== s[right]){
> >                 return false
> >             }
> >         }
> >         return true
> >     }
> >     const backtracking = (startIndex) => {
> >         if(startIndex === s.length){
> >             res.push([...path])
> >         }
> >         for(let i = startIndex; i < s.length; i++){
> >             if(!isPalindrome(s, startIndex, i)){
> >                 continue
> >             }
> >             path.push(s.slice(startIndex, i + 1))
> >             backtracking(i + 1)
> >             path.pop()
> >         }
> >     }
> >     backtracking(0)
> >     return res
> > };
> > ```

# ==leetcode-93 复原ip地址==

> ![image-20220727133847785](images/image-20220727133847785.png)
>
> [93. 复原 IP 地址 - 力扣（LeetCode）](https://leetcode.cn/problems/restore-ip-addresses/)
>
> > 回溯
> >
> > 此题关键在于对终止条件的判定较上一题更复杂一点，需要判定只判定startIndex =\== s.length还不够(==可能出现2.5.5.2.5.5.1.1.1.3.5这种情况==)，还需要判定path.length === 4才行，根据这个条件path.length一旦>4就可以直接return了
> >
> > 还有判断ip地址中单个整数是否合法的问题，注意尽量少的使用隐式转换(=='00' == 0是true==)
> >
> > ```js
> > /**
> >  * @param {string} s
> >  * @return {string[]}
> >  */
> > var restoreIpAddresses = function(s) {
> >     const res = [], path = []
> >     const isLegal = (num) => {
> >         if(num.length > 1 && num[0] == '0'){
> >             return false
> >         }
> >         if(num.length > 3 || num > 255){
> >             return false
> >         }
> >         return true
> >     }
> >     const backtracking = (startIndex) => {
> >         if(path.length > 4){
> >             return
> >         }
> >         if(startIndex === s.length && path.length === 4){
> >             res.push(path.join('.'))
> >             return
> >         }
> >         for(let i = startIndex; i < startIndex + 3; i++){
> >             if(!isLegal(s.slice(startIndex, i + 1))){
> >                 continue
> >             }
> >             path.push(s.slice(startIndex, i + 1))
> >             backtracking(i + 1)
> >             path.pop()
> >         }
> >     }
> >     backtracking(0)
> >     return res
> > };
> > ```

# leetcode-78 子集

> ![image-20220727140203307](images/image-20220727140203307.png)
>
> [78. 子集 - 力扣（LeetCode）](https://leetcode.cn/problems/subsets/)
>
> > 回溯
> >
> > 此题要求所有子集，那么也就是求抽象树的所有节点
> >
> > ![78.子集](images/202011232041348.png)
> >
> > 此题的终止条件是startIndex > nums.length，但是由于满足终止条件的时候for循环也结束了，所以可以不加终止条件，将res.push放在最上面可以将所有子集都push进去
> >
> > ```js
> > /**
> >  * @param {number[]} nums
> >  * @return {number[][]}
> >  */
> > var subsets = function(nums) {
> >     const res = [], path = []
> >     const backtracking = (startIndex) => {
> >         res.push([...path])
> >         for(let i = startIndex; i < nums.length; i++){
> >             path.push(nums[i])
> >             backtracking(i + 1)
> >             path.pop()
> >         }
> >     }
> >     backtracking(0)
> >     return res
> > };
> > ```

# leetcode-90 子集2

> ![image-20220801134241566](images/image-20220801134241566.png)
>
> [90. 子集 II - 力扣（LeetCode）](https://leetcode.cn/problems/subsets-ii/)
>
> > 回溯+树层去重
> >
> > ![90.子集II](images/20201124195411977.png)
> >
> > ```js
> > /**
> >  * @param {number[]} nums
> >  * @return {number[][]}
> >  */
> > var subsetsWithDup = function(nums) {
> >     nums.sort((a, b) => a - b)
> >     const res = [], path = []
> >     const backtracking = (startIndex) => {
> >         res.push([...path])
> >         for(let i = startIndex; i < nums.length; i++){
> >             if(i > startIndex && nums[i] === nums[i - 1]){
> >                 continue
> >             }
> >             path.push(nums[i])
> >             backtracking(i + 1)
> >             path.pop()
> >         }
> >     }
> >     backtracking(0)
> >     return res
> > };
> > ```

# leetcode-491 递增子序列

> ![image-20220801142254727](images/image-20220801142254727.png)
>
> [491. 递增子序列 - 力扣（LeetCode）](https://leetcode.cn/problems/increasing-subsequences/)
>
> > 回溯
> >
> > 这一题去重的方法和之前的有很大区别，因为此题求递增子序列但是原数组并不是有序的，所以我们不能对nums进行排序，所以在每一个树层(==也就是每一次递归中==)初始化一个数组uset来将这一层中出现的数字对应的下标将其赋值为true，以此作为去重判断if的条件，另外一个条件则是为了求递增子序列所以当前要push的nums[i]要大于path的最末尾的数字，如果小于就进行去重
> >
> > ![491. 递增子序列1](images/20201124200229824.png)
> >
> > ```js
> > /**
> >  * @param {number[]} nums
> >  * @return {number[][]}
> >  */
> > var findSubsequences = function(nums) {
> >     const res = [], path = []
> >     const backTracking = (startIdex) => {
> >         if(path.length >= 2){
> >             res.push([...path])
> >         }
> >         const uset = []
> >         for(let i = startIdex; i < nums.length; i++){
> >             if(nums[i] < path[path.length - 1] || uset[nums[i]]){
> >                 continue
> >             }
> >             uset[nums[i]] = true
> >             path.push(nums[i])
> >             backTracking(i + 1)
> >             path.pop()
> >         }
> >     }
> >     backTracking(0)
> >     return res
> > };
> > ```

# leetcode-46 全排列

> ![image-20220801145208135](images/image-20220801145208135.png)
>
> [46. 全排列 - 力扣（LeetCode）](https://leetcode.cn/problems/permutations/)
>
> > 回溯
> >
> > 树枝去重，定义一个数组，当某个元素被使用过就在数组中标记并通过递归传给下一层，需要注意的是回溯pop后需要将标记删除，这样第二列树枝才能正常走通
> >
> > ![46.全排列](images/20211027181706.png)
> >
> > ```js
> > /**
> >  * @param {number[]} nums
> >  * @return {number[][]}
> >  */
> > var permute = function(nums) {
> >     const res = [], path = [], used = []
> >     const backTracking = (used) => {
> >         if(path.length === nums.length){
> >             res.push([...path])
> >             
> >         }
> >         for(let i = 0; i < nums.length; i++){
> >             if(used[i]){
> >                 continue
> >             }
> >             path.push(nums[i])
> >             used[i] = true
> >             backTracking(used)
> >             path.pop()
> >             used[i] = false
> >         }
> >     }
> >     backTracking(used)
> >     return res
> > };
> > ```

##### 代码随想录-贪心算法

贪心算法，每一次的局部最优达到整体的整体最优

==贪心算法四部曲==

> 将问题分解为若干个子问题
>
> 找出适合的贪心策略
>
> 求解每一个子问题的最优解
>
> 将局部最优解堆叠成全局最优解

# leetcode-455 分发饼干

> ![image-20220723120644464](images/image-20220723120644464.png)
>
> [455. 分发饼干 - 力扣（LeetCode）](https://leetcode.cn/problems/assign-cookies/)
>
> > 贪心算法
> >
> > 这里注意不能从小往大遍历，因为有可能出现最小饼干满足不了最小胃口，这样就遍历不动了(==但大的饼干能满足小的胃口==)，应该从大往小遍历，这样即使最大饼干满足不了最大胃口，也能遍历下去找到小的胃口
> >
> > ```js
> > /**
> >  * @param {number[]} g
> >  * @param {number[]} s
> >  * @return {number}
> >  */
> > var findContentChildren = function(g, s) {
> >     g.sort((a, b) => a - b)
> >     s.sort((a, b) => a - b)
> >     let res = 0
> >     let index = s.length - 1
> >     for(let i = g.length - 1; i >= 0; i--){
> >         if(g[i] <= s[index] && index >= 0){
> >             res++
> >             index--
> >         }
> >     }
> >     return res
> > };
> > ```

# leetcode-376 摇摆序列

> ![image-20220723143355039](images/image-20220723143355039.png)
>
> [376. 摆动序列 - 力扣（LeetCode）](https://leetcode.cn/problems/wiggle-subsequence/)
>
> > 贪心
> >
> > 因为子序列是可以通过删除元素得到的，那么只需要将一个单调序列的非两端节点都删除即可实现局部最优
> >
> > ![376.摆动序列](images/20201124174327597.png)
> >
> > 那么这个总序列内的每一条单调序列都经过了这样的操作后即可实现整体最优
> >
> > res初始化为1是根据题目的特殊情况默认序列最右有一个峰值(==不管是空序列、一个元素的序列、还是n个相同元素的序列==)，因此for遍历不遍历最后一个元素i < nums.length - 1。
> >
> > 因为最左边元素默认prediff为0，所以判断条件的prediff都要加上=0
> >
> > ```js
> > /**
> >  * @param {number[]} nums
> >  * @return {number}
> >  */
> > var wiggleMaxLength = function(nums) {
> >     let res = 1, prediff = 0, curdiff = 0
> >     for(let i = 0; i < nums.length - 1; i++){
> >         curdiff = nums[i + 1] - nums[i]
> >         if((prediff >= 0 && curdiff < 0) || (prediff <= 0 && curdiff > 0)){
> >             res++
> >             prediff = curdiff
> >         }
> >     }
> >     return res
> > };
> > ```

# leetcode-53 最大子数组和

> 见上

# leetcode-122 买卖股票的最佳时机2

> ![image-20220728152122755](images/image-20220728152122755.png)
>
> [122. 买卖股票的最佳时机 II - 力扣（LeetCode）](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/)
>
> > 贪心算法
> >
> > 假设从第n天到n+m天股票价格持续上升，那么从第n天买股票，到第n+m天卖股票的利润可以分解为(prices[n+1]-prices[n])+(prices[n+2]-prices[n+1])+...+(prices[n+m]-prices[n+m-1])，那么可以将问题分解为prices.length-1个局部问题，每个局部如果是正利润，则将其加入全局利润，如果局部利润是负利润，则将其摒弃
> >
> > ```js
> > /**
> >  * @param {number[]} prices
> >  * @return {number}
> >  */
> > var maxProfit = function(prices) {
> >     let res = 0
> >     for(let i = 0; i < prices.length - 1; i++){
> >         const profit = prices[i + 1] - prices[i]
> >         if(profit > 0){
> >             res += profit
> >         }
> >     }
> >     return res
> > };
> > ```

# leetcode-55 跳跃游戏

> ![image-20220728194352822](images/image-20220728194352822.png)
>
> [55. 跳跃游戏 - 力扣（LeetCode）](https://leetcode.cn/problems/jump-game/)
>
> > 贪心算法
> >
> > 将该问题理解为覆盖范围问题，每一个下标位置都有一个覆盖范围(==最远距离==)，每次向前移动都会更新一次覆盖范围，为了不错过任何一种情况，下标每次+1，覆盖范围cover就更新相对于下标0的最远距离，而下标i只能在cover范围内移动(i <= cover)，当cover能覆盖到nums.length - 1就返回true，否则返回false
> >
> > ```js
> > /**
> >  * @param {number[]} nums
> >  * @return {boolean}
> >  */
> > var canJump = function(nums) {
> >     let cover = 0
> >     for(let i = 0; i <= cover; i++) {
> >         cover = Math.max(cover, i + nums[i])
> >         if(cover >= nums.length - 1) {
> >             return true
> >         }
> >     }
> >     return false
> > };
> > ```

# leetcode-45 跳跃游戏2

> ![image-20220728213515326](images/image-20220728213515326.png)
>
> [45. 跳跃游戏 II - 力扣（LeetCode）](https://leetcode.cn/problems/jump-game-ii/)
>
> > 贪心算法
> >
> > 相比较上一题多了最小步数，基本思路还是一样的。此题同时维护两个覆盖范围，一个是当前的覆盖范围，另一个是下一步的覆盖范围，如果当前的覆盖范围覆盖到了nums.length - 1，就结束循环，如果没有，就steps++，然后当前范围变成下一步范围。
> >
> > 每一步都走到当前范围内最远位置，当当前范围覆盖到了终点，那么走的步数就是最小步数
> >
> > ```js
> > /**
> >  * @param {number[]} nums
> >  * @return {number}
> >  */
> > var jump = function(nums) {
> >     let curIndex = 0
> >     let nextIndex = 0
> >     let steps = 0
> >     for(let i = 0; i < nums.length - 1; i++) {
> >         nextIndex = Math.max(nums[i] + i, nextIndex)
> >         if(i === curIndex) {
> >             curIndex = nextIndex
> >             steps++
> >         }
> >     }
> >     return steps
> > };
> > ```

# leetcode1005 k次取反后最大化的数组和

> ![image-20220808131747015](images/image-20220808131747015.png)
>
> [1005. K 次取反后最大化的数组和 - 力扣（LeetCode）](https://leetcode.cn/problems/maximize-sum-of-array-after-k-negations/)
>
> > 贪心 * 2
> >
> > 首先将数组绝对值降序排列从前往后遍历数组，每遇到一个负数就将其取反并且k--，直到k不大于0，遍历结束后如果剩下的k % 2 =\== 1的话(==这里包括了k>0的含义==)，意味着还要接着取反并且一定会有一个负数出来(==因为如果%2结果为0的话剩下的k就是偶数只要对着一个数取偶数次反就可以了==)，那么就减去2倍的绝对值最小的数(==因为前面已经加过一次了==)
> >
> > 两次贪心算法，绝对值大的负数先取反，如果负数全反完了还剩k，那么就将绝对值最小的数反复取反将k消耗完即可
> >
> > ```js
> > /**
> >  * @param {number[]} nums
> >  * @param {number} k
> >  * @return {number}
> >  */
> > var largestSumAfterKNegations = function(nums, k) {
> >     nums.sort((a, b) => Math.abs(b) - Math.abs(a))
> >     let res = 0
> >     for(let i = 0; i < nums.length; i++){
> >         if(nums[i] < 0 && k-- > 0){
> >             nums[i] = -nums[i]
> >         }
> >         res += nums[i]
> >     }
> >     if(k % 2 === 1){
> >         res -= nums[nums.length - 1] * 2
> >     }
> >     return res
> > };
> > ```

# leetcode-134 加油站

> ![image-20220808134455487](images/image-20220808134455487.png)
>
> [134. 加油站 - 力扣（LeetCode）](https://leetcode.cn/problems/gas-station/)
>
> > 贪心算法
> >
> > 从零开始遍历，累计增加当前油量curSum，通过+= gas[i] - cost[i]的方式，一旦curSum小于0，那么从当前的start开始就不行，所以充值start = 当前的i + 1，充值curSum = 0，遍历的同时累计增加totalSum，最后如果totalSum小于0那么肯定跑不满全程return -1，最后return start
> >
> > ==重点解释为什么[i,j]区间的和为负数那新的起点就得是j+1，因为车能从i开始跑证明起码在i这点上是正的，那么[i,j]区间和是负的情况下从[i,j]中间作为新的起点的话，一个区间和是负的再舍弃掉正的就是负上加负，肯定不行。==
> >
> > ==那么如果从中间开始舍弃掉的前面部分区间和是负的呢，这种情况不可能发生，因为如果前面部分和是负的那么当车走到前面部分的时候就没油了就会进入到重新选起点的环节了，不会走到这一步。==
> >
> > ==[i,j]区间和为负肯定是走到j这一点才负的，因为如果中间就负了是不可能从i走到j的==
> >
> > ```js
> > /**
> >  * @param {number[]} gas
> >  * @param {number[]} cost
> >  * @return {number}
> >  */
> > var canCompleteCircuit = function(gas, cost) {
> >     const gasLen = gas.length
> >     let start = 0
> >     let curSum = 0
> >     let totalSum = 0
> > 
> >     for(let i = 0; i < gasLen; i++) {
> >         curSum += gas[i] - cost[i]
> >         totalSum += gas[i] - cost[i]
> >         if(curSum < 0) {
> >             curSum = 0
> >             start = i + 1
> >         }
> >     }
> > 
> >     if(totalSum < 0) return -1
> > 
> >     return start
> > };
> > ```

# leetcode-135 分发糖果

> ![image-20220808144242270](images/image-20220808144242270.png)
>
> [135. 分发糖果 - 力扣（LeetCode）](https://leetcode.cn/problems/candy/)
>
> > 贪心
> >
> > 这道题目一定是要确定一边之后，再确定另一边，例如比较每一个孩子的左边，然后再比较右边，**如果两边一起考虑一定会顾此失彼**。
> >
> > 先确定右边评分大于左边的情况（也就是从前向后遍历），如果右边大于左边，那么右边糖果+1，实现所有孩子的右边更优秀孩子都多一个糖果
> >
> > 再确定左边评分大于右边情况，(从后向前遍历)，如果左大于右，那么左在左和右边+1之间取最大值
> >
> > ==需要注意的是左边大于右边一定要从后往前遍历，因为如果从前往后遍历会出现例如[3,2,1]，然后3的糖果变成2的糖果数+1，2的糖果数变成1的糖果数加一，结果[2,2,1]，3比2评分高但是结果糖果数相同不服题意==
> >
> > ==同理右大于左的情况一定要从前往后遍历，这样每一次比较能都利用上一次比较的结果==
> >
> > ```js
> > /**
> >  * @param {number[]} ratings
> >  * @return {number}
> >  */
> > var candy = function(ratings) {
> >     let candys = new Array(ratings.length).fill(1)
> > 
> >     for(let i = 1; i < ratings.length; i++) {
> >         if(ratings[i] > ratings[i - 1]) {
> >             candys[i] = candys[i - 1] + 1
> >         }
> >     }
> > 
> >     for(let i = ratings.length - 2; i >= 0; i--) {
> >         if(ratings[i] > ratings[i + 1]) {
> >             candys[i] = Math.max(candys[i], candys[i + 1] + 1)
> >         }
> >     }
> > 
> >     let count = candys.reduce((a, b) => {
> >         return a + b
> >     })
> > 
> >     return count
> > };
> > ```

##### 代码随想录-动态规划

动态规划dp于贪心算法最大的不同是，贪心算法每个局部之间没有关联，而动态规划是当前局部由前一个局部推导过来的

==动态规划五部曲==

> 确定dp数组和下标含义
>
> 确定递推公式
>
> 初始化dp数组
>
> 确定遍历顺序
>
> 举例推导dp数组

# leetcode-509 斐波那契数

> ![image-20220724123005363](images/image-20220724123005363.png)
>
> [509. 斐波那契数 - 力扣（LeetCode）](https://leetcode.cn/problems/fibonacci-number/)
>
> > dp
> >
> > ```js
> > /**
> >  * @param {number} n
> >  * @return {number}
> >  */
> > var fib = function(n) {
> >     let dp = [0, 1]
> >     for(let i = 2; i <= n; i++){
> >         dp[i] = dp[i - 1] + dp[i - 2]
> >     }
> >     return dp[n]
> > };
> > ```

# leetcode-70 爬楼梯

> ![image-20220704125443290](images/image-20220704125443290.png)
>
> [70. 爬楼梯 - 力扣（LeetCode）](https://leetcode.cn/problems/climbing-stairs/)
>
> > dp
> >
> > 此题难点在于确定递推数组和下标含义以及确定递推公式，仔细研读题目可以知道到达第n层可以由第n-1层再跨一层到达，还可以从第n-2层再跨两层到达，那么可以推导出第n层的方法数为第n-1层的方法数+第n-2层的方法数，那么就可以确定dp数组下标为第n层，数值为第n层的方法数，那么递推公式则为dp[n] = dp[n - 1] + dp[n - 2]
> >
> > ==这里注意的是dp[0]在本题没有意义，dp[1]和dp[2]固定为1和2==
> >
> > ```js
> > /**
> >  * @param {number} n
> >  * @return {number}
> >  */
> > var climbStairs = function(n) {
> >     let dp = [0, 1, 2]
> >     for(let i = 3; i <= n; i++){
> >         dp[i] = dp[i - 1] + dp[i - 2]
> >     }
> >     return dp[n]
> > };
> > ```

# leetcode-746 最小花费爬楼梯

> ![image-20220724132307985](images/image-20220724132307985.png)
>
> [746. 使用最小花费爬楼梯 - 力扣（LeetCode）](https://leetcode.cn/problems/min-cost-climbing-stairs/)
>
> > dp
> >
> > 这题注意确定dp数组和下标的含义，cost[i]是指从第i层向上爬一次需要花费的值，那么==dp[i]则定义为从第0层或者第1层爬到第i层的最小花费再加上第i层再向上爬一次的花费的总花费==，因此要求的最小花费就是dp数组中最后两个数中的较小值
> >
> > ```js
> > /**
> >  * @param {number[]} cost
> >  * @return {number}
> >  */
> > var minCostClimbingStairs = function(cost) {
> >     const dp = [cost[0], cost[1]]
> >     for(let i = 2; i < cost.length; i++){
> >         dp[i] = Math.min(dp[i - 1], dp[i - 2]) + cost[i]
> >     }
> >     return Math.min(dp[dp.length - 1], dp[dp.length - 2])
> > };
> > ```

# leetcode-62 不同路径

> ![image-20220729130933330](images/image-20220729130933330.png)
>
> ![image-20220729130943133](images/image-20220729130943133.png)
>
> [62. 不同路径 - 力扣（LeetCode）](https://leetcode.cn/problems/unique-paths/)
>
> > dp
> >
> > 创建一个mxn的二维数组
> >
> > 确定dp数组和下标含义：dp[i]\[j]表示从dp[0]\[0]到dp[i]\[j]的路径总数
> >
> > 确定递推公式：因为机器人只能下移和右移，所以dp[i]\[j] = dp[i-1]\[j]+dp[i]\[j-1]
> >
> > dp数组初始化：dp[i]\[0]和dp[0]\[j]都为1，因为它们都是dp[0]\[0]一直右移或者一直下移得到的
> >
> > 确定遍历顺序：因为终点是右下角，所以从左到右一层一层遍历即可
> >
> > 举例推导dp数组：略
> >
> > ```js
> > /**
> >  * @param {number} m
> >  * @param {number} n
> >  * @return {number}
> >  */
> > var uniquePaths = function(m, n) {
> >     const dp = Array(m).fill().map(item => Array(n))
> >     for(let i = 0; i < m; i++){
> >         dp[i][0] = 1
> >     }
> >     for(let j = 0; j < n; j++){
> >         dp[0][j] = 1
> >     }
> >     for(let i = 1; i < m; i++){
> >         for(let j = 1; j < n; j++){
> >             dp[i][j] = dp[i - 1][j] + dp[i][j - 1]
> >         }
> >     }
> >     return dp[m - 1][n - 1]
> > };
> > ```

# leetcode-63 不同路径2

> ![image-20220729135220077](images/image-20220729135220077.png)
>
> ![image-20220729135239243](images/image-20220729135239243.png)
>
> [63. 不同路径 II - 力扣（LeetCode）](https://leetcode.cn/problems/unique-paths-ii/)
>
> > dp
> >
> > 大致思路和上一题一样，只需要将障碍物略过即可，但是在掠过障碍物的操作过程中有许多小问题值得注意
> >
> > 在掠过障碍物时应该将obstacleGrid[i]\[0] =\== 0放进将dp[i]\[0]初始化为1的for循环的条件判定中，而不是在for循环中用if(obstacleGrid[i]\[0] =\== 1)来判定然后将dp[i]\[0] = 0，这里实际上没有考虑到障碍物出现在第一行中间的情况，当障碍物出现在第一行中间时，这一行障碍物之前的路径数都为1，障碍物之后的路径数都为0，而if判断的方法显然和这种情况不相符。dp[0]\[j]初始化时也同理。
> >
> > 将obstacleGrid[i]\[0] =\== 0放入for循环判定条件中可以做到当循环碰到obstacleGrid[i]\[0] =\== 0的情况就停止for循环而不会继续循环赋值，而停止赋值后根据题意障碍物后面的应该为0，所以初始化dp时需要将所有元素都初始化为0==(item => Array(n).fill(0))==
> >
> > 在非第一行和第一列的dp赋值的循环中，如果碰到障碍物只需将障碍物坐标的dp元素赋值为0即可，因为这种情况dp元素的值是它上面和左边元素值的和，障碍物的存在不会将它之后的dp元素的值强行变为0
> >
> > ```js
> > /**
> >  * @param {number[][]} obstacleGrid
> >  * @return {number}
> >  */
> > var uniquePathsWithObstacles = function(obstacleGrid) {
> >     const m = obstacleGrid.length
> >     const n = obstacleGrid[0].length
> >     const dp = Array(m).fill().map(item => Array(n).fill(0))
> >     
> >     for (let i = 0; i < m && obstacleGrid[i][0] === 0; i++) {
> >         dp[i][0] = 1
> >     }
> >     
> >     for (let j = 0; j < n && obstacleGrid[0][j] === 0; j++) {
> >         dp[0][j] = 1
> >     }
> >     
> >     for (let i = 1; i < m; i++) {
> >         for (let j = 1; j < n; j++) {
> >             dp[i][j] = obstacleGrid[i][j] === 1 ? 0 : dp[i - 1][j] + dp[i][j - 1]
> >         }
> >     }
> >         
> >     return dp[m - 1][n - 1]
> > };
> > ```

# leetcode-343 整数拆分

> ![image-20220729142352748](images/image-20220729142352748.png)
>
> [343. 整数拆分 - 力扣（LeetCode）](https://leetcode.cn/problems/integer-break/)
>
> > dp
> >
> > 确定dp数组和下标含义：dp[i]代表整数i的最大乘积，因为数组下标从0开始，所以初始化数组dp时要Array(n + 1)
> >
> > 确定递推公式：==这里是本题难点，for循环j到i，每一层循环中都求(i - j)\*j和dp[i - j]\*j之间的较大值，然后赋给dp[i]，dp[i]则和下一层循环中的(i - j)\*j和dp[i - j]\*j之间的较大值进行比较去较大值。这里(i - j)\*j的其实就是将i分成两个整数求乘积，dp[i - j]\*j则是将i分成两个或两个以上的整数求乘积==
> >
> > dp数组初始化：n>=2，所以只需要将dp[2] = 1即可
> >
> > 确定遍历顺序：想要求dp[i]必须将小于i的dp先求出来，所以for循环i=3到n，内部则需要for循环j到i
> >
> > 举例推导：略
> >
> > ```js
> > /**
> >  * @param {number} n
> >  * @return {number}
> >  */
> > var integerBreak = function(n) {
> >     let dp = new Array(n + 1).fill(0)
> >     dp[2] = 1
> > 
> >     for(let i = 3; i <= n; i++) {
> >         for(let j = 1; j < i; j++) {
> >             dp[i] = Math.max(dp[i], dp[i - j] * j, (i - j) * j)
> >         }
> >     }
> >     return dp[n]
> > };
> > ```
> >
> > 贪心算法
> >
> > 每次拆成n个3，如果剩下是4、2、1，则保留，然后相乘，但是这个结论需要数学证明其合理性！
> >
> > ```js
> > /**
> >  * @param {number} n
> >  * @return {number}
> >  */
> > var integerBreak = function(n) {
> >     if (n == 2){
> >         return 1
> >     }
> >     if (n == 3){
> >         return 2
> >     }
> >     if (n == 4){
> >         return 4
> >     }
> >     let m = n, count = 0
> >     while(1){
> >         count++
> >         m -= 3
> >         if(m === 4 || m <= 3){
> >             break
> >         }
> >     }
> >     if(m === 4){
> >         return 3 ** count * 4
> >     }
> >     if(m <= 3){
> >         return 3 ** count * m
> >     }
> > };
> > ```

# leetcode-96 不同的二叉搜索树

> ![image-20220809091051860](images/image-20220809091051860.png)
>
> [96. 不同的二叉搜索树 - 力扣（LeetCode）](https://leetcode.cn/problems/unique-binary-search-trees/)
>
> > dp
> >
> > 确定dp数组和下标意义、dp[i]代表i这个数返回的二叉搜索树的种数
> >
> > ==本题难点是推导递推公式，观察可发现i总种数等于dp[0]\*dp[i-1]+dp[1]\*dp[i-2]+...+dp[i-2]\*dp[1]+dp[i-1]\*dp[0]==
> >
> > ==从0和i-1开始是因为总有一个根节点占据一个节点==
> >
> > ![96.不同的二叉搜索树2](images/20210107093226241.png)
> >
> > 初始化dp数组，为了满足递推公式将dp[0]初始化为1，dp[1]由题意也为1
> >
> > ```js
> > /**
> >  * @param {number} n
> >  * @return {number}
> >  */
> > var numTrees = function(n) {
> >     const dp = new Array(n + 1).fill(0)
> >     dp[0] = 1
> >     dp[1] = 1
> >     for(let i = 2; i < n + 1; i++){
> >         for(let j = 0; j < i; j++){
> >             dp[i] += dp[j] * dp[i - j - 1]
> >         }
> >     }
> >     return dp[n]
> > };
> > ```

# leetcode-416 分割等和子集(==01背包==)

> ![image-20220809121251371](images/image-20220809121251371.png)
>
> [416. 分割等和子集 - 力扣（LeetCode）](https://leetcode.cn/problems/partition-equal-subset-sum/)
>
> > 01背包dp、一维数组解法
> >
> > ```js
> > /**
> >  * @param {number[]} nums
> >  * @return {boolean}
> >  */
> > var canPartition = function(nums) {
> >     const sum = nums.reduce((a, b) => a + b)
> >     if(sum % 2 !== 0){
> >         return false
> >     }
> >     const dp = new Array(sum / 2 + 1).fill(0)
> >     for(let i = 0; i < nums.length; i++){
> >         for(let j = sum / 2; j >= nums[i]; j--){
> >             dp[j] = Math.max(dp[j], dp[j - nums[i]] + nums[i]);
> >             if (dp[j] === sum / 2) {
> >                 return true;
> >             }
> >         }
> >     }
> >     return false
> > };
> > ```

# leetcode-1049 最后一块石头的重量(==01背包==)

> ![image-20220809125848414](images/image-20220809125848414.png)
>
> [1049. 最后一块石头的重量 II - 力扣（LeetCode）](https://leetcode.cn/problems/last-stone-weight-ii/)
>
> > 01背包dp、一维数组解法
> >
> > 和上一题基本类似，唯一不同的是结果需要转一下弯，把所有石头重量加在一起再从最中间平分，然后两边相减就是结果
> >
> > ![image-20220809130434208](images/image-20220809130434208.png)
> >
> > ```js
> > /**
> >  * @param {number[]} stones
> >  * @return {number}
> >  */
> > var lastStoneWeightII = function(stones) {
> >     const sum = stones.reduce((a, b) => a + b)
> >     const half = Math.floor(sum / 2)
> >     const dp = new Array(half + 1).fill(0)
> >     for(let i = 0; i < stones.length; i++){
> >         for(let j = half; j >= stones[i]; j--){
> >             dp[j] = Math.max(dp[j], dp[j - stones[i]] + stones[i])
> >         }
> >     }
> >     return sum - dp[half] - dp[half]
> > };
> > ```

# leetcode-494 目标和(==01背包==)

> ![image-20220811153924671](images/image-20220811153924671.png)
>
> [494. 目标和 - 力扣（LeetCode）](https://leetcode.cn/problems/target-sum/)
>
> > dp 01背包，一维dp解法
> >
> > 思路:
> >
> > 由题nums元素全为非负整数，求nums数组和sum，将赋予+的数字和设为变量positive，赋予-的数字绝对值和设为negative，那么本题就是要positive - negative = target，也就是（sum - negative）- negative = target，也就是negative = （sum - target）/ 2，所以本体的dp[i]的含义转变为negative = i时满足negative = （sum - target）/ 2的方法数
> >
> > ```js
> > /**
> >  * @param {number[]} nums
> >  * @param {number} target
> >  * @return {number}
> >  */
> > var findTargetSumWays = function(nums, target) {
> >     const sum = nums.reduce((a, b) => a+b);
> >     if(Math.abs(target) > sum) {
> >         return 0;
> >     }
> >     if((target + sum) % 2 === 1) {
> >         return 0;
> >     }
> >     const neg = (target + sum) / 2;
> >     let dp = new Array(neg + 1).fill(0);
> >     dp[0] = 1;
> >     for(let i = 0; i < nums.length; i++) {
> >         for(let j = neg; j >= nums[i]; j--) {
> >             dp[j] += dp[j - nums[i]];
> >         }
> >     }
> >     return dp[neg];
> > };
> > ```

。。。

。。。背包题过滤掉

。。。

# leetcode-198 打家劫舍

> ![image-20220813142149686](images/image-20220813142149686.png)
>
> [198. 打家劫舍 - 力扣（LeetCode）](https://leetcode.cn/problems/house-robber/)
>
> > 简单的dp，不能连续偷只能隔一家偷
> >
> > dp数组定义为，偷到下标为i的位置能偷到的最大金额
> >
> > dp初始化为dp[0] = nums[0]，dp[1] = nums[0]和nums[1]的最大值(==因为不能连续偷==)
> >
> > 递推公式就是dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i])
> >
> > 递推从前往后
> >
> > ```js
> > /**
> >  * @param {number[]} nums
> >  * @return {number}
> >  */
> > var rob = function(nums) {
> >     const dp = [nums[0], Math.max(nums[0], nums[1])]
> >     for(let i = 2; i < nums.length; i++){
> >         dp[i] = Math.max(dp[i - 1], dp[i - 2] + nums[i])
> >     }
> >     return dp[nums.length - 1]
> > };
> > ```

# leetcode-213 打家劫舍2

> ![image-20220814121235054](images/image-20220814121235054.png)
>
> [213. 打家劫舍 II - 力扣（LeetCode）](https://leetcode.cn/problems/house-robber-ii/)
>
> > dp
> >
> > 因为成环首尾不能共存，所以分两种情况，一种只要头不要尾，一种只要尾不要头，两种情况分别求最大值，然后两个最大值再求最大值
> >
> > ```js
> > /**
> >  * @param {number[]} nums
> >  * @return {number}
> >  */
> > var rob = function(nums) {
> >     if(nums.length === 1){
> >         return nums[0]
> >     }
> >     if(nums.length === 0){
> >         return 0
> >     }
> >     const dp1 = [nums[0], Math.max(nums[0], nums[1])]
> >     const dp2 = [0, nums[1], Math.max(nums[1], nums[2])]
> >     for(let i = 2; i < nums.length - 1; i++){
> >         dp1[i] = Math.max(dp1[i - 2] + nums[i], dp1[i - 1])
> >     }
> >     for(let i = 2; i < nums.length; i++){
> >         dp2[i] = Math.max(dp2[i - 2] + nums[i], dp2[i - 1])
> >     }
> >     return Math.max(dp1[nums.length - 2], dp2[nums.length - 1])
> > };
> > ```

##### 代码随想录-单调栈

# leetcode-739 每日温度

> ![image-20220725125650671](images/image-20220725125650671.png)
>
> [739. 每日温度 - 力扣（LeetCode）](https://leetcode.cn/problems/daily-temperatures/)
>
> > 思路：首先确定单调栈中保存的是t数组的下标，创建t数组长的全0数组作为res(==相比空数组的好处是有些值为0的地方不用手动赋0了==)，创建栈数组stack，然后for循环遍历t数组。t数组的每一个元素先判断stack内有没有值，有值再判断这个元素是不是比stack内最右边的值大，然后将stack最右边的值弹出，并计算和当前遍历值下标之间的差然后传入res对应的下标位置。最后将当前遍历值下标push进stack(==第一次push0作为初始参考量==)。==注意这个判断应当是while而不是if，因为会出现前面的n个数据都不满足条件然后当前数据满足前面多个数据条件的情况==
> >
> > ```js
> > /**
> >  * @param {number[]} temperatures
> >  * @return {number[]}
> >  */
> > var dailyTemperatures = function(temperatures) {
> >     const res = Array(temperatures.length).fill(0), stack = []
> >     for(let i = 0; i < temperatures.length; i++){
> >         while(stack.length && temperatures[i] > temperatures[stack[stack.length - 1]]){
> >             const top = stack.pop()
> >             res[top] = i - top
> >         }
> >         stack.push(i)
> >     }
> >     return res
> > };
> > ```

# leetcode-496 下一个更大数1

> ![image-20220725135349084](images/image-20220725135349084.png)
>
> [496. 下一个更大元素 I - 力扣（LeetCode）](https://leetcode.cn/problems/next-greater-element-i/)
>
> > 暴力解法
> >
> > ```js
> > /**
> >  * @param {number[]} nums1
> >  * @param {number[]} nums2
> >  * @return {number[]}
> >  */
> > var nextGreaterElement = function(nums1, nums2) {
> >     const res = Array(nums1.length).fill(-1)
> >     for(let i = 0; i < nums1.length; i++){
> >         let j = nums2.indexOf(nums1[i])
> >         for(; j < nums2.length; j++){
> >             if(nums2[j] > nums1[i]){
> >                 res[i] = nums2[j]
> >                 break
> >             }
> >         }
> >     }
> >     return res
> > };
> > ```
> >
> > 单调栈
> >
> > 思路为求nums1中每一元素在nums2中的下一个更大元素求出来，如果存在这个更大元素就将nums1中的该元素和它在nums2中的下一个更大元素作为一个key value对存入map中，然后遍历nums1，判断每一元素在map中是否能get到，能get到就赋值给res
> >
> > ```js
> > /**
> >  * @param {number[]} nums1
> >  * @param {number[]} nums2
> >  * @return {number[]}
> >  */
> > var nextGreaterElement = function(nums1, nums2) {
> >     const res = Array(nums1.length).fill(-1), stack = [], map = new Map()
> >     for (let i = 0; i < nums2.length; i++) {
> >         while (stack.length && nums2[i] > nums2[stack[stack.length - 1]]) {
> >             let index = stack.pop();
> >             map.set(nums2[index], nums2[i]);
> >         }
> >         stack.push(i);
> >     }
> >     for(let j = 0; j < nums1.length; j++){
> >         if(map.get(nums1[j])){
> >             res[j] = map.get(nums1[j])
> >         }
> >     }
> >     return res
> > };
> > ```

# ==leetcode-503 下一个更大元素2==

> ![image-20220725155001773](images/image-20220725155001773.png)
>
> [503. 下一个更大元素 II - 力扣（LeetCode）](https://leetcode.cn/problems/next-greater-element-ii/)
>
> > 将循环范围扩大到两倍的nums长度，然后通过取余变相实现循环两遍，这样不需要手动将两个相同nums拼接，减少了空间时间的浪费
> >
> > ```js
> > /**
> >  * @param {number[]} nums
> >  * @return {number[]}
> >  */
> > var nextGreaterElements = function(nums) {
> >     const res = Array(nums.length).fill(-1), stack = []
> >     for(let i = 0; i < nums.length * 2; i++){
> >         while(stack.length && nums[i % nums.length] > nums[stack[stack.length - 1]]){
> >             const index = stack.pop()
> >             res[index] = nums[i % nums.length]
> >         }
> >         stack.push(i % nums.length)
> >     }
> >     return res
> > };
> > ```

# ==leetcode-42 接雨水==

> ![image-20220730113524341](images/image-20220730113524341.png)
>
> [42. 接雨水 - 力扣（LeetCode）](https://leetcode.cn/problems/trapping-rain-water/)
>
> > 单调栈
> >
> > queue中保存的是height中的下标
> >
> > 情况一：当前遍历下标代表数比单调栈末尾元素代表数小，那么直接push
> >
> > 情况二：当前遍历下标代表数和单调栈末尾元素代表数一样大，queue.pop()后再push(==因为求高度的时候需要最右边的柱子来计算高度==)
> >
> > 情况三：当前遍历下标代表数比单调栈末尾元素代表数大，queue.pop()并用mid变量接住，然后分别求h(==雨水深度==)w(==雨水宽度==)，注意求之前要if(queue.length)，否则求出NaN，求w时要用i - queue[queue.length - 1] - 1而不能用i - mid，因为情况二的存在可能有相同高度的柱子下标被pop出去了，只有i - queue[queue.length - 1] - 1才不会出错。
> >
> > 整个情况三都需要while循环，因为可能存在之前一直是情况一然后此时遍历下标代表数比queue的多个元素代表数都大，所以要while直到queue内没有元素代表数比当前下标代表数大
> >
> > ```js
> > /**
> >  * @param {number[]} height
> >  * @return {number}
> >  */
> > var trap = function(height) {
> >     const len = height.length
> >     if(len <= 2){
> >         return 0
> >     }
> >     const queue = [0]
> >     let res = 0
> >     for(let i = 1; i < len; i++){
> >         if(height[i] < height[queue[queue.length - 1]]){
> >             queue.push(i)
> >         }
> >         else if(height[i] == height[queue[queue.length - 1]]){
> >             queue.pop()
> >             queue.push(i)
> >         }else{
> >             while(queue.length && height[i] > height[queue[queue.length - 1]]){
> >                 const mid = queue.pop()
> >                 if(queue.length){
> >                     const h = Math.min(height[i], height[queue[queue.length - 1]]) - height[mid]
> >                     const w = i - queue[queue.length - 1] - 1
> >                     res += h * w
> >                 }
> >             }
> >             queue.push(i)
> >         }
> >     }
> >     return res
> > };
> > ```

# ==leetcode-84 柱状图中最大的矩形==

> ![image-20220730125105187](images/image-20220730125105187.png)
>
> ![image-20220730125116356](images/image-20220730125116356.png)
>
> [84. 柱状图中最大的矩形 - 力扣（LeetCode）](https://leetcode.cn/problems/largest-rectangle-in-histogram/)
>
> > 单调栈
> >
> > 和接雨水很相似，但是这一题单调栈方法背后的逻辑需要仔细想清楚
> >
> > 接雨水是queue中从左到右是从大到小，而此题是从小到大
> >
> > 只要当heights[i] < heights[stack[stack.length-1]]时，就将w = i - stack[stack.length -1] - 1，然后h就等于heights[stackTopIndex]
> >
> > 假设heights[i] =\== 0这样while的第一次循环球的就是n(==n个是因为有可能情况二pop掉了多个相同高度的下标==)个高度为heights[stack[stack.length-1]]的面积，而第二次循环则是前一次循环pop后的heights[stack[stack.length-1]]为高，但是宽则包括了前一次循环的宽。。。
> >
> > ![image-20220730130159832](images/image-20220730130159832.png)
> >
> > (==此处为了方便直接说栈内存储的是高度了，实际代码中存储的是高度对应的index==)
> >
> > 例如上面这个例子，单调栈内保存的是156，然后遍历到2的时候触发情况三，while循环第一次先求的是6的高度6乘2的index减去5的index再减1(==这样求宽为了避免可能出现的情况二pop掉了n个6==)，然后第二个循环则是5的高度5乘2的index减去1的index(==这样求宽不仅将可能出现的情况二pop掉了n个5计算在内了，还符合题意的将6的宽度也计算在内，这样符合题目中最大矩形的要求==)
> >
> > ```js
> > /**
> >  * @param {number[]} heights
> >  * @return {number}
> >  */
> > var largestRectangleArea = function(heights) {
> >     let maxArea = 0;
> >     const stack = [0];
> >     heights = [0,...heights,0]; // 数组头部加入元素0 数组尾部加入元素0 方便下面对原heights数组头尾元素进行条件判断
> >     for(let i = 1; i < heights.length; i++){
> >         if(heights[i] > heights[stack[stack.length-1]]){ // 情况三
> >             stack.push(i);
> >         } else if(heights[i] === heights[stack[stack.length-1]]){ // 情况二
> >             stack.pop(); // 这个可以加，可以不加，效果一样，思路不同
> >             stack.push(i);
> >         } else { // 情况一
> >             while(heights[i] < heights[stack[stack.length-1]]){// 当前bar比栈顶bar矮
> >                 const stackTopIndex = stack.pop();// 栈顶元素出栈，并保存栈顶bar的索引
> >                 let w = i - stack[stack.length -1] - 1;
> >                 let h = heights[stackTopIndex]
> >                 // 计算面积，并取最大面积
> >                 maxArea = Math.max(maxArea, w * h);
> >             }
> >             stack.push(i);// 当前bar比栈顶bar高了，入栈
> >         }
> >     }
> >     return maxArea;
> > };
> > ```

##### 其他

# 剑指offer 62 约瑟夫环

> ![image-20220914231606099](images/image-20220914231606099.png)
>
> [剑指 Offer 62. 圆圈中最后剩下的数字 - 力扣（LeetCode）](https://leetcode.cn/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)
>
> > ```js
> > /**
> >  * @param {number} n
> >  * @param {number} m
> >  * @return {number}
> >  */
> > var lastRemaining = function(n, m) {
> >     let res = 0;
> >     for(let i = 2; i <= n; i++){
> >         res = (res + m) % i;
> >     }
> >     return res;
> > };
> > ```