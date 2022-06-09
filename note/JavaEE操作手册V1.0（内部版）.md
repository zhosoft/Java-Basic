# 数组

## 数组定义

> 一维数组的 声明方式：type var[] 或 type[] var；
>
> int a[];
> int[] a1;
> double b[];
> String[] c; //引用类型变量数组

动态初始化 ：数组声明且为数组元素分配空间与赋值的操作分开进行。

静态初始化 ：在定义数组的同时就为数组元素分配空间并赋值。



## 二维数组

> 随机生成整型的数字：[a,b]  公式：(int)(Math.random()*(b-a+1)+a)

###  数组中涉及的常见算法

1. 数组元素的赋值(杨辉三角、回形数等)
2. 求数值型数组中元素的最大值、最小值、平均数、总和等
3. 数组的复制、反转、查找(线性查找、二分法查找)
4. 数组元素的排序算法



> 求数值型数组中元素的最大值、最小值、平均数、总和等

```java
// 两位数随机数

// [10,99]
double d = 10.6;
//		System.out.println((int)d);
//		System.out.println((int)(Math.random()*(99-10+1)+10));
int[] arr1 = new int[10];
for (int i = 0; i < 10; i++) {
    // [a,b] 公式：(int)(Math.random()*(b-a+1)+a)
    arr1[i] = (int) (Math.random() * (99 - 10 + 1) + 10);
}

for (int i = 0; i < arr1.length; i++) {
    System.out.print(arr1[i] + "\t");
}
System.out.println();
//1、数值型数值的最大值
int maxValue = arr1[0];
for (int i = 1; i < arr1.length; i++) {
    if (maxValue < arr1[i]) {
        maxValue = arr1[i];			
    }
}
System.out.println("最大值:" + maxValue);
//2、数值型数值的最小值
int minValue =  arr1[0];
for (int i = 0; i < arr1.length; i++) {
    if (minValue > arr1[i]) {
        minValue = arr1[i];
    }
}
System.out.println("最小值：" + minValue);
//3、数值型数值的和
int sumValue = 0;
for (int i = 0; i < arr1.length; i++) {
    sumValue += arr1[i];
}
System.out.println("和：" + sumValue);
//4、数值型数值的平均数
Double argVaule = 0.0;
argVaule =(double) (sumValue / arr1.length);
System.out.println("平均数：" + argVaule);
```



```java
int[] arr1,arr2;
arr1 =new int[]{2,3,5,7,11,13,17,19};
for (int i = 0; i < arr1.length; i++) {
    System.out.print(arr1[i] + "\t");
}
System.out.println();
arr2 = arr1;
for (int i = 0; i < arr1.length; i++) {
    if(i % 2 == 0) {
        arr1[i] = i;
    }
}
for (int i = 0; i < arr1.length; i++) {
    System.out.print(arr1[i] + "\t");//0 3 2 7 4 13 6 19
}

//arr1，arr2：地址值一样，都指向堆空间的同一个地址值；
```



> 数组复制

```java
String[] array  = new String[] {"CC","DD","AA","GG","FF","BB"};
for (int i = 0; i < array.length; i++) {
    System.out.print(array[i] + "\t");
}
System.out.println();

String[] array2 = new String[array.length];
for (int i = 0; i < array.length; i++) {
    array2[i] = array[i];
}

for (int i = 0; i < array2.length; i++) {
    System.out.print(array2[i] + "\t");
}
System.out.println();

for (int i = 0; i < array.length; i++) {
    if (i % 2 == 0) {
        array[i] = "ZZ";
    }
}		

for (int i = 0; i < array.length; i++) {
    System.out.print(array[i] + "\t");
}
```

> 二分查找，条件：需要排序对象为有序（从小到大，从大到小，都可以）

```java
int[] array = new int[] { -30, -26, -10, 0, 20, 34, 55, 78, 100 };

// 需要查找的元素值
int dest = 100;
// 开始的索引值
int head = 0;
// 结束的索引值
int end = array.length - 1;
//用来标记是那种条件不满足break掉的
boolean endFlag = true;
// head <= end 条件不满足时，循环结束
while (head <= end) {
    int i = (head + end) / 2;
    if (dest == array[i]) {
        System.out.println("查找的位置是：" + i);
        endFlag = false;
        break;
    } else if (dest < array[i]) {
        end = i - 1;
    } else {
        head = i + 1;
    }
}
if (endFlag) {
    System.out.println("没有查询到需要查询的元素！！！");
}
```

