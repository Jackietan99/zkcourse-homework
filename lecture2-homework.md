# 第2课 课后作业

> 题目链接：https://zkshanghai.xyz/notes/exercise2.html


## 第1题： 转换为bit位Num2Bits

```rust
template Num2Bits(n) {

    var res = 0;
    var lv = 1;
    
    signal input in;
    signal output out[n];
   
    for (var i =0 ;i< n ;i++) {
         //向右移动 i 位，然后按位与运算 最右边一位（即第 i 位），最后将结果存储到数组 out 的第 i 个位置上
        out[i] <-- (in >> i) & 1;
        //约束
        out[i]*(out[i] - 1) === 0; 

        res += out[i]*lv;

        //计算下一个更高位的权重。
        lv = lv + lv;
    }
    //结果约束
    
    res === in;
}
```

## 第2题： 判零 IsZero



## 第3题： 相等 IsEqual



## 第4题： 少于 LessThan



## 第5题： 选择器 Selector


