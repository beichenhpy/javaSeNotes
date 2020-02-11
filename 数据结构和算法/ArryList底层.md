---
title: 面向对象的数组-Arrilist
categories: 数据结构与算法
---
# 面向对象的数组-Arrilist

## 1.定义

```java
//面向对象的数组
    //用于储存数据的数组
    private int[] elements;

    public array_List() {
        elements = new int[0];
    }

    //获取长度的方法

    public int size() {
        return elements.length;
    }

```

## 2.添加元素到数组尾部

```java
//往数组的末尾添加一个元素
    public void add(int element) {
        //创建新数组
        int[] newArra = new int[elements.length + 1];
        //把原数组元素赋值到新数组
        for (int i = 0; i < elements.length; i++) {
            newArra[i] = elements[i];
        }
        //添加新元素到新数组中
        newArra[elements.length] = element;
        //新数组替换旧数组
        elements = newArra;
    }
```

## 3.打印所有元素到控制台

```java
//打印所有元素到控制台
    public void show() {
        System.out.println(Arrays.toString(elements));
    }
```

## 4.删除数组中的元素

```java
//删除数组中的元素
    public void delete(int index) {
        //判断下标是否越界
        if (index < 0 || index > elements.length - 1) {
            throw new RuntimeException("下标越界");
        }
        //创建新数组 长度为原数组-1
        int[] newArr = new int[elements.length - 1];
        //把原数组除第index个都写入新数组
        for (int i = 0; i < elements.length - 1; i++) {
            if (i < index) {
                newArr[i] = elements[i];
            } else {
                newArr[i] = elements[i + 1];
            }
        }
        elements = newArr;
    }
```

## 5.获取某个元素

```java
//获取某个元素
    public int get(int index) {
        //判断下标是否越界
        if (index < 0 || index > elements.length - 1) {
            throw new RuntimeException("下标越界");
        }
        return elements[index];
    }
```

## 6.插入元素到指定位置

```java
//插入一个元素到指定位置
    public void insert(int index ,int number) {
        //判断下标是否越界
        if (index < 0 || index > elements.length - 1) {
            throw new RuntimeException("下标越界");
        }
        int[] newArr = new int[elements.length+1];
        for (int i = 0; i < elements.length; i++) {
           if (i < index) {
               newArr[i] = elements[i];
           } else {
               newArr[i+1] = elements[i];
           }

        }
        newArr[index] = number;
        elements = newArr;
    }
```

## 7.替换指定位置元素

```java
//替换指定位置的元素
    public void set (int index ,int number) {
        //判断下标是否越界
        if (index < 0 || index > elements.length - 1) {
            throw new RuntimeException("下标越界");
        }
        elements[index] = number;
    }
```

## 8.线性查找

```java
private static int find(int number, int[] arr) {
        int index = -1;
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] == number) {
                index = i;
                break;
            }
        }
        return index;
    }
```

## 9.二分查找

```java
 //二分查找
    private static int binaryfind(int number, int[] arr) {
        //初始化要找的位置
        int index = -1;
        //记录开始位置
        int min = 0;
        //记录结束位置
        int max = arr.length -1;
        //记录中间位置
        int mid = (min + max) / 2;
        //循环
        while(min <= max){
            //如果目标数在中间的右侧
            if (arr[mid] < number) {
                //把左范围改为mid+1
                min = mid + 1;
                //如果目标数在中间的左侧
            } else if (arr[mid] > number) {
                //把有范围改为mid -1
                max = mid - 1;
            } else {
                //找到后 赋值 index
                index = mid;
                break;
            }
            //更新mid
            mid = (min + max) / 2;
        }
        return index;
    }
```

