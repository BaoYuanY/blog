---
title: PHP一些函数方法汇总
date: 2023-06-12 11:26:00
tags: [ 'PHP' ]
categories: [ 'PHP' ]
excerpt: "还包含已经写好的算法"
---

# ‣ 数组多键排序

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
       

# ‣ 查询任意一个字符串是否存在另一个字符串中

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


# ‣ 两个数组取交集

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


# ‣ json_encode时遇到\n解决办法

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

# ‣ 简单加密手机号方式

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


# ‣ 判断某个点位是否存在一个经纬度区域

```php
$polygon = new \League\Geotools\Polygon\Polygon([
    [48.9675969, 1.7440796],
    [48.4711003, 2.5268555],
    [48.9279131, 3.1448364],
    [49.3895245, 2.6119995],
]);

$polygon->setPrecision(5); // set the comparision precision
$polygon->pointInPolygon(new \League\Geotools\Coordinate\Coordinate([49.1785607, 2.4444580])); // true
$polygon->pointInPolygon(new \League\Geotools\Coordinate\Coordinate([49.1785607, 5])); // false
$polygon->pointOnBoundary(new \League\Geotools\Coordinate\Coordinate([48.7193486, 2.13546755])); // true
$polygon->pointOnBoundary(new \League\Geotools\Coordinate\Coordinate([47.1587188, 2.87841795])); // false
$polygon->pointOnVertex(new \League\Geotools\Coordinate\Coordinate([48.4711003, 2.5268555])); // true
$polygon->pointOnVertex(new \League\Geotools\Coordinate\Coordinate([49.1785607, 2.4444580])); // false
$polygon->getBoundingBox(); // return the BoundingBox object
```

## 代码解释
它可以帮助您知道点 (坐标) 是在多边形中还是在多边形的边界上，以及点是否在多边形的顶点上。

## composer包详情
https://packagist.org/packages/league/geotools
**这个包里面可以提供了一个方法：它可以帮助您了解一个点（坐标）是在多边形中还是在多边形的边界上，以及它是否在多边形的顶点上**
- 针对一个或一 组提供商以串行/并行方式批处理地理编码和反向地理编码请求
- 使用PSR-6缓存地理编码和反向地理编码结果以提高性能
- 在命令行界面(CLI) + 转储程序和格式化程序中计算地理编码和反向地理编码
- 接受几乎所有类型的 WGS84 地理坐标作为坐标。
- 支持23 种不同的椭圆体，如果需要，可以轻松提供新的椭圆体
- 将十进制度坐标转换并格式化为十进制度分或度分秒坐标。
- 在通用横轴墨卡托(UTM) 投影中转换十进制坐标 
- 使用flat 、 great circle 、haversine或vincenty算法计算两个坐标之间以米（默认）、km 、mi或ft为单位的距离
- 计算从原点坐标到目标坐标的 初始和最终方位角（以度为单位）
- 计算从原点坐标到目标坐标的初始和最终基点（方向），在wikipedia中阅读更多内容
- 计算起点坐标和终点坐标之间的中点（坐标）
- 使用以度为单位的给定方位和以米为单位的距离计算目的地点（坐标）
- 将坐标编码为地理哈希字符串并将其解码为坐标，请在 维基百科和geohash.org上阅读更多内容
- 通过 10:10 算法对坐标进行编码
- Polygon类提供了检查 poing（坐标）在多边形边界内或边界上的方法。
- Distance 、Point 、Geohash和Convert类的命令行界面( CLI) 


# ‣ 根据一个url获得携带的参数
```php
public static function getParamsByUrl(Request $request): array
{
    $arr = parse_url($request->url());
    parse_str($arr['query'] ?? '', $query);
    //unset鉴权
    if (isset($query['access_token'])) {
        unset($query['access_token']);
    }
    foreach ($query as $k => $v) {
        if ('' === $v) {
            unset($query[$k]);
        }
        if (is_array($v)) {
            foreach ($v as $kk => $vv) {
                if ('' === $vv) {
                    unset($query[$k][$kk]);
                }
            }
        }
    }
    return $query ?? [];
}
```

# ‣ 加密解密示例

加密

```php
<?php

class WeilingEncryptCryptUtilService
{
    protected $aesKey;

    public function __construct($encodingAesKey)
    {
        $this->aesKey = base64_decode(preg_replace('/\s+/', '', $encodingAesKey), true);
    }

    /**
     * @throws \Exception
     */
    public function encrypt($content)
    {
        try {
            list($cipherKey, $iv) = $this->prepareKeyAndIv($this->aesKey);
            $encodedContent = $this->encode($content);
            $encrypted      = openssl_encrypt($encodedContent, 'AES-256-CBC', $cipherKey, OPENSSL_RAW_DATA, $iv);
            $result         = base64_encode($encrypted);
        } catch (\Exception $e) {
            throw new \Exception("Error occurred during encryption: " . $e->getMessage());
        }

        return $result;
    }

    protected function prepareKeyAndIv($key)
    {
        $cipherKey = substr($key, 0, 32);
        $iv        = substr($key, 0, 16);

        return [$cipherKey, $iv];
    }

    protected function encode($content)
    {
        $contentLength = strlen($content);
        $networkOrder  = $this->number2BytesNetworkOrder($contentLength);
        $randomString  = $this->getRandomString(16);

        $pad = 32 - ($contentLength % 32);
        if ($pad === 0) {
            $pad = 32;
        }

        $padString      = str_repeat(chr($pad), $pad);
        $encodedContent = $randomString . $networkOrder . $content . $padString;

        return $encodedContent;
    }

    protected function number2BytesNetworkOrder($number)
    {
        $bytesInNetworkOrder = "";
        for ($i = 0; $i < 4; $i++) {
            $bytesInNetworkOrder = chr(($number >> (8 * (3 - $i))) & 0xFF) . $bytesInNetworkOrder;
        }
        return $bytesInNetworkOrder;
    }

    protected function getRandomString($length)
    {
        $characters   = '0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ';
        $charLen      = strlen($characters);
        $randomString = '';

        for ($i = 0; $i < $length; $i++) {
            $randomString .= $characters[rand(0, $charLen - 1)];
        }

        return $randomString;
    }
}
```


解密

```php
<?php

class WeilingCallbackCryptUtilService
{

    protected $aesKey;

    public function __construct($encodingAesKey)
    {
        $this->aesKey = base64_decode(preg_replace('/\s+/', '', $encodingAesKey), true);
    }

    public function decrypt($encryptedText)
    {
        try {
            list($cipherKey, $iv) = $this->prepareKeyAndIv($this->aesKey);
            $decrypted = openssl_decrypt(base64_decode($encryptedText), 'AES-256-CBC', $cipherKey, 7, $iv);
            $result    = $this->decode($decrypted);
        } catch (\Exception $e) {
            $result = null;
        }

        return $result;
    }

    protected function prepareKeyAndIv($key)
    {
        $cipherKey = substr($key, 0, 32);
        $iv        = substr($key, 0, 16);

        return [$cipherKey, $iv];
    }

    protected function decode($decrypted)
    {
        $pad = ord($decrypted[strlen($decrypted) - 1]);
        if ($pad < 1 || $pad > 32) {
            $pad = 0;
        }
        $content = substr($decrypted, 0, strlen($decrypted) - $pad);

        $networkOrder  = substr($content, 16, 4);
        $contentLength = $this->bytesNetworkOrder2Number($networkOrder);
        $trueContent   = substr($content, 20, $contentLength);
        $pad           = ord($trueContent[strlen($trueContent) - 1]);
        return substr($trueContent, 0, -1 * $pad);
    }

    protected function bytesNetworkOrder2Number($bytesInNetworkOrder)
    {
        $sourceNumber = 0;
        for ($i = 0; $i < 4; $i++) {
            $sourceNumber <<= 8;
            $sourceNumber |= ord($bytesInNetworkOrder[$i]);
        }
        return $sourceNumber;
    }
}
```


# ‣ BC Math

BC Math 扩展提供了一些任意精度的数学运算函数。以下是这些函数的列表以及相应的示例：

1. `bcadd` - 加法

```php
$num1 = '1.235';
$num2 = '2.764';
$precision = 3;

$result = bcadd($num1, $num2, $precision);
echo $result; // 输出 "3.999"
```

2. `bcsub` - 减法

```php
$num1 = '5.321';
$num2 = '2.125';
$precision = 3;

$result = bcsub($num1, $num2, $precision);
echo $result; // 输出 "3.196"
```

3. `bcmul` - 乘法

```php
$num1 = '4.75';
$num2 = '3.23';
$precision = 4;

$result = bcmul($num1, $num2, $precision);
echo $result; // 输出 "15.3375"
```

4. `bcdiv` - 除法

```php
$num1 = '9.876';
$num2 = '2.53';
$precision = 4;

$result = bcdiv($num1, $num2, $precision);
echo $result; // 输出 "3.9012"
```

5. `bcmod` - 模（求余数）

```php
$num1 = '105';
$num2 = '4';

$result = bcmod($num1, $num2);
echo $result; // 输出 "1"
```

6. `bcpow` - 乘方

```php
$num1 = '2';
$exponent = '5';
$precision = 0;

$result = bcpow($num1, $exponent, $precision);
echo $result; // 输出 "32"
```

7. `bcpowmod` - 应用模运算的乘方

```php
$num1 = '5';
$exponent = '3';
$modulus = '11';

$result = bcpowmod($num1, $exponent, $modulus);
echo $result; // 输出 "4"
```

8. `bcsqrt` - 求平方根

```php
$num1 = '9';
$precision = 0;

$result = bcsqrt($num1, $precision);
echo $result; // 输出 "3"
```

9. `bccomp` - 比较大小

```php
$num1 = '2.1415';
$num2 = '2.1416';
$precision = 4;

$result = bccomp($num1, $num2, $precision);
echo $result; // 输出 "-1"（$num1 < $num2）

$num1 = '2.1416';
$num2 = '2.1416';
$result = bccomp($num1, $num2, $precision);
echo $result; // 输出 "0"（$num1 == $num2）

$num1 = '2.1417';
$num2 = '2.1416';
$result = bccomp($num1, $num2, $precision);
echo $result; // 输出 "1"（$num1 > $num2）
```

在使用 BC Math 函数时，选择适当的精度参数非常重要，因为它决定了结果中小数点后的位数。注意，BC Math 默认只支持整数计算，如果需要保留小数，请务必指定精度。







