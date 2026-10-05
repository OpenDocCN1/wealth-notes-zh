# 5.1.4 字符串函数

字符串函数及意义如表 5.4 所示。

# 表 5.4 字符串函数及意义

| 字符串函数 | 意义 |
|---|---|
| capitalize() | 将字符串的第一个字符转换为大写 |
| center(width, fillchar) | 返回一个指定的宽度 width 居中的字符串，fillchar 为填充的字符，默认为空格 |
| count(str, beg=0, end=len(string)) | 返回 str 在 string 里面出现的次数，可选参数为在字符串搜索的开始与结束位置 |
| expandtabs(tabsize=8) | 把字符串中的 tab 符号为空格，tab 符号默认的空格数是 8 |
| find(str, beg=0, end=len(string)) | 检测 str 是否包含在字符串中，如果指定范围 beg 和 end，则检查是否包含在指定范围内，如果包含则返回开始的索引值，否则返回-1 |
| index(str, beg=0, end=len(string)) | 跟 find()方法一样，只不过如果 str 不在字符串中会报一个异常 |
| isalnum() | 如果字符串至少有一个字符并且所有字符都是字母或数字则返回 True，否则返回 False |
| isalpha() | 如果字符串至少有一个字符并且所有字符都是字母则返回 True，否则返回 False |
| isdigit() | 如果字符串只包含数字则返回 True，否则返回 False |
| isspace() | 如果字符串中只包含空白，则返回 True，否则返回 False |
| islower() | 如果字符串中至少包含一个区分大小写的字符，并且所有这些（区分大小写的）字符都是小写，则返回 True，否则返回 False |
| isupper() | 如果字符串中至少包含一个区分大小写的字符，并且所有这些（区分大小写的）字符都是大写，则返回 True，否则返回 False |
| join(seq) | 以指定字符串为分隔符，将 seq 中的所有元素（的字符串表示）合并为一个新的字符串 |
| len(string) | 返回字符串长度 |
| ljust(width[, fillchar]) | 返回一个原字符串左对齐，并使用 fillchar 填充至长度为 width 的新字符串，fillchar 默认为空格 |
| rjust(width[, fillchar]) | 返回一个原字符串右对齐，并使用 fillchar（默认空格）填充至长度 width 的新字符串 |
| lstrip() | 截掉字符串左边的空格或指定字符 |
| rstrip() | 删除字符串末尾的空格 |
| strip([chars]) | 在字符串上执行 lstrip()和 rstrip() |
| max(str) | 返回字符串 str 中最大的字母 |
| min(str) | 返回字符串 str 中最小的字母 |
| replace(old, new [, max]) | 将字符串中的 old 替换成 new，如果 max 指定，则替换不超过 max 次 |
| split(str="", num=string.count(str)) | num=string.count(str)以 str 为分隔符截取字符串，如果 num 有指定值，则仅截取 num 个子字符串 |