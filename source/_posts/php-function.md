---
title: PHP一些函数方法汇总
date: 2023-06-12 11:26:00
tags: [ 'PHP' ]
categories: [ 'PHP' ]
excerpt: "还包含已经写好的算法"
---

# 数组多键排序

``` php
        use Illuminate\Support\Collection;

        $tempArr = [
            [
                'name'    => '小红',
                'chinese' => '99',
                'math'    => '91',
                'english' => '87',
            ], [
                'name'    => '小华',
                'chinese' => '93',
                'math'    => '94',
                'english' => '89',
            ], [
                'name'    => '小明',
                'chinese' => '93',
                'math'    => '91',
                'english' => '98',
            ], [
                'name'    => '小丽',
                'chinese' => '93',
                'math'    => '94',
                'english' => '98',
            ], [
                'name'    => '小强',
                'chinese' => '98',
                'math'    => '92',
                'english' => '87',
            ]
        ];
        
        //方式1
        $sorted = collect($tempArr)->sortBy(function ($item) {
            return sprintf('%d-%s-%s', $item['chinese'], $item['math'], $item['english']);
        }, SORT_REGULAR, true);

        $sorted->each(function ($item) {
            dump($item['name']);
        });
        
        //方式2
        collect($tempArr)->sort(function ($a, $b) {
            if ($a['chinese'] != $b['chinese']) {
                return $b['chinese'] <=> $a['chinese'];
            }

            if ($a['math'] != $b['math']) {
                return $b['math'] <=> $a['math'];
            }
            return $b['english'] <=> $a['english'];
        })->each(function ($item) {
            dump($item['name']);
        });

```

## 代码解释
### 方式1
1. 定义一个 $tempArr 数组，包含了 5 名学生的姓名（name）、语文成绩（chinese）、数学成绩（math）以及英语成绩（english）。
2. 使用 Laravel 的 `collect()` 函数把 `$tempArr` 数组转换成一个集合（Collection）对象，然后调用 `sortBy()`方法对集合中的元素进行排序。`sortBy()` 方法需要一个回调函数作为参数，这个回调函数决定了排序的规则。
3. 在回调函数内部，使用 sprintf() 函数为每个学生生成一个字符串，格式为：`chinese-math-english`。这个字符串将作为排序的依据。
4. `sortBy()` 方法还有两个额外的参数：排序依据类型（`SORT_REGULAR`）和排序方向（`true`）。这里设置为 `SORT_REGULAR` 说明按照正常比较的方式（即数字按大小，字符串按字典序）进行排序，`true` 表示降序排序（从大到小）。
5. 排序完成后，使用 `each()` 方法依次迭代集合中的元素。`each()` 方法接受一个回调函数作为参数，在此回调函数内部，可以访问到当前元素以及元素的索引（键值）。
### 方式2
1. 首先比较两个元素的`chinese`。如果它们不相等，则用合并比较运算符`<=>`比较它们，并返回结果。注意：`$b['chinese'] <=> $a['chinese']` 这种写法是为了降序排列。因此，成绩较高的元素会排在较低的元素之前。
2. 如果`chinese`相等，比较函数会继续比较两个元素的`math`。如果它们不相等，同样使用合并比较运算符`<=>`进行比较，并返回结果。这里同样是降序排列。
3. 如果`math`也相等，最后比较`english`。与前两次比较一样，使用合并比较运算符`<=>`进行降序排序。
        










