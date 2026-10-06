```
str = "runoob"                 # 字符串只有字母
print(str.isalpha())
str = "Runoob example....wow!!!"      # 字符串除了字母，还有别的字符
print(str.isalpha())
str = "123456"                      # 字符串只有数学
print(str.isdigit())
str = "Runoob example....wow!!!"
print(str.isdigit())
str = "RUNOOB example....wow!!!"    # 字符串有大写字母
print(str.islower())
str = "runoob example....wow!!!"    # 字符串只有小写字母
print(str.islower())
str = "      "                      # 字符串中只包含空白
print(str.isspace())
str = "Runoob example....wow!!!"
print(str.isspace())
str = "THIS IS STRING EXAMPLE....WOW!!!"   # 字符串只有大写字母
print(str.isupper())
str = "THIS is string example....wow!!!"
print(str.isupper())
s1 = "-"
s2 = ""
seq = ("r", "u", "n", "o", "o", "b")     # 字符串序列
print(s1.join(seq))
print(s2.join(seq))
str = "runoob"
print("字符串长度:",len(str))             # 字符串长度
l = [1,2,3,4,5]
print("列表元素个数:",len(l))             # 列表元素个数
str = "Runoob example....wow!!!"
print("左对齐:  ",str.ljust(50,'*'))
str = "this is string example....wow!!!"
print("右对齐:  ",str.rjust(50,'*'))
str = "     this is string example....wow!!!     "
print("删除字符串左边的空格:  ",str.lstrip() )
str = "     this is string example....wow!!!     "
print("删除字符串右边的空格:  ",str.rstrip() )
str = "     this is string example....wow!!!     "
print("删除字符串左右两边的空格:  ",str.strip() )
str = "runoob"
print("最大字符:" + max(str))
str = "runoob";
print("最小字符:" + min(str));
str = "www.w3cschool.cc"
print("菜鸟旧地址:  ", str)
```