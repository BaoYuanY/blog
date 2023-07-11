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
       

# 查询任意一个字符串是否存在另一个字符串中

```php
$match = function($string, $keywords) {
    $pattern = '/(' . implode('|', $keywords) . ')/';
    return (bool) preg_match($pattern, $string);
}

$keywords = ['物流', '配送', '送货', '司机'];
$string   = "这里有一个关于物流配送的例子";

$res      = $match($string, $keywords);

//输出true
```

## 代码解释
`strpos()` 只针对于一个字符串，所以这里用正则匹配更好一点


# 两个数组取交集

```php
$array1 = [1, 2, 3];
$array2 = [2, 3, 4];

$intersection = array_intersect($array1, $array2);

print_r($intersection);

//输出
Array
(
    [1] => 2
    [2] => 3
)

```

## 代码解释
1. 在上面的示例中，`array_intersect()` 函数接受两个数组 `$array1` 和 `$array2` 作为参数，并返回它们的交集。最后，使用 `print_r()` 函数打印交集数组。
2. 请注意，交集数组中的元素保留了原始数组中的键。如果你想重置键，可以使用 `array_values()` 函数对交集数组进行重新索引：


# json_encode时遇到\n解决办法

当你使用`json_decode`函数将包含`\n`的字符串转换为数组时，可能会出现错误。这是因为`\n`是一个特殊的转义字符，表示换行符。在PHP中，当你使用双引号字符串时，`\n`会被解析为换行符，而不是作为字符串的一部分。

例如，如果你有以下代码：

```php
$jsonString = '{"field": "Hello\nWorld"}';
$array      = json_decode($jsonString, true);
```

在这种情况下，`json_decode`将无法正确解析包含`\n`的字符串，因为它会尝试将其解析为换行符。这将导致无效的JSON格式，从而引发错误。

为了解决这个问题，你可以使用`addslashes`函数在填充字段之前对字符串进行转义，或者使用`json_encode`函数在将数组转换为JSON字符串之前对字段进行转义。这样可以确保`\n`被正确地转义为`\\n`，而不会被解析为换行符。

以下是使用`addslashes`函数转义字段的示例：

```php
$fieldValue         = "Hello\nWorld";
$escapedFieldValue  = addslashes($fieldValue);
$jsonString         = '{"field": "'.$escapedFieldValue.'"}';
$array              = json_decode($jsonString, true);
```

或者，你可以直接使用`json_encode`函数来处理整个数组，而不是手动构建JSON字符串：

```php
$array        = array("field" => "Hello\nWorld");
$jsonString   = json_encode($array);
$decodedArray = json_decode($jsonString, true);
```

这样，`\n`将被正确地转义为`\\n`，并且`json_decode`函数将能够正确地解析JSON字符串并将其转换为数组。

# 简单加密手机号方式

```php
    function encryptPhone(string $phone): string
    {
        if (strlen($phone) == 11) {
            return substr_replace($phone, '****', 3, 4);
        } else {
            return "***********";
        }
    }
```










