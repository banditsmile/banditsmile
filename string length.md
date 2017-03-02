

```
/**
 * 可以统计中文字符串长度的函数
 * @param string $str 要计算长度的字符串
 * @param int $threeByteLen 三字节字符的计算长度，2(默认)表示一个三字节算二个字符，1表示一个三字节算一个字符
 * @param int $fourByteLen 四字节字符的计算长度，2(默认)表示一个四字节算二个字符，1表示一个四字节算一个字符
 * @param bool $cleanSkin  是否去掉emoji肤色再计算长度
 * @return int;
 *
 */
function abslength($str,$threeByteLen=2,$fourByteLen=2,$cleanSkin=true){
    if(!is_string($str)){
        return false;
    }
    if(empty($str)){
        return 0;
    }
    if($cleanSkin){
		$str = preg_replace("/[\x{1F3FB}-\x{1F3FF}]{1}/u","",$str);
	}
    //4e00-9fa5 这个是中文的unicode编码范围
    //preg_match_all("/([\x{4e00}-\x{9fa5}]){1}/u",$str,$arrCh);
    //800-ffff 这个是三字节unicode编码范围
    preg_match_all("/([\x{800}-\x{ffff}]){1}/u",$str,$arrCh3);
    //10000-1ffff 这个是四字节unicode编码范围
    preg_match_all("/([\x{10000}-\x{1ffff}]){1}/u",$str,$arrCh4);
    $length =  strlen($str)-count($arrCh3[0])*(3-$threeByteLen)
        -count($arrCh4[0])*(4-$fourByteLen);
    return $length;
}
```
参考文档

[emoji](https://zh.wikipedia.org/wiki/%E7%B9%AA%E6%96%87%E5%AD%97)
[阮一峰 字符编码笔记](http://www.ruanyifeng.com/blog/2007/10/ascii_unicode_and_utf-8.html)
[php 多字节字符分隔](http://php.net/manual/zh/function.mb-split.php)
