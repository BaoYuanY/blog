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

        $sorted = collect($tempArr)->sortBy(function ($item) {
            return sprintf('%d-%s-%s', $item['chinese'], $item['math'], $item['english']);
        }, SORT_REGULAR, true);

        $sorted->each(function ($item) {
            dump($item['name']);
        });
        
        //输出
            //小红
            //小强
            //小丽
            //小华
            //小明

```

## 代码解释

1. 定义一个 $tempArr 数组，包含了 5 名学生的姓名（name）、语文成绩（chinese）、数学成绩（math）以及英语成绩（english）。
2. 使用 Laravel 的 `collect()` 函数把 `$tempArr` 数组转换成一个集合（Collection）对象，然后调用 `sortBy()`方法对集合中的元素进行排序。`sortBy()` 方法需要一个回调函数作为参数，这个回调函数决定了排序的规则。
3. 在回调函数内部，使用 sprintf() 函数为每个学生生成一个字符串，格式为：`chinese-math-english`。这个字符串将作为排序的依据。
4. `sortBy()` 方法还有两个额外的参数：排序依据类型（`SORT_REGULAR`）和排序方向（`true`）。这里设置为 `SORT_REGULAR` 说明按照正常比较的方式（即数字按大小，字符串按字典序）进行排序，`true` 表示降序排序（从大到小）。
5. 排序完成后，使用 `each()` 方法依次迭代集合中的元素。`each()` 方法接受一个回调函数作为参数，在此回调函数内部，可以访问到当前元素以及元素的索引（键值）。
                
        











