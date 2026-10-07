
比较长，一行没有办法写完，这时一条命令要分几行写，对于跨行命令，需要告诉 Stata 这几行是一个命令，这时可以利用注释命令 /* */ 或 // 将多行语句连接成一个命令。举个简单的例子，比如一条命令，先清除系统然后打开数据 auto.dta. 写成两行，但它是一个命令，用第三种注释方法把它们连接在一起。下面两种方法都可以：

```
use ///
auto. clear
或
use /*
*/ auto. clear
```

上面两种方式，Stata 看到 // 或 /* */ 都直接跳到下一行接着读，将两行语句当作一条命令执行。对于跨行命令较多时，还可以利用 # delimit 命令重新设定语句结束符，比如下下面语句定义分号为命令结束符，则 Stata 不再按行读入语句，而是寻找分号，分号出现时，命令结束。

```
# delimit ; use auto, clear; # delimit cr
```

## 2. 两种函数

Stata 的很多命令会有返回值，返回值会保存在两种函数中，一般估计命令的返回值会保存在 e() 函数中，其他命令的返回值会保存在 r() 函数中，命令后的返回值，可以通过 return list 或 ereturn list 进行查询。比如下下面显示的是用 sumarize 命令返回的结果。这些返回结果可以通过新的变量取出来，用于后面的语句中。当运行另外的语句时，r() 函数中储存的值会发生变化，因而，要取出来先保存在特定的变量或宏中，以便后面程序使用。

```
. sysuse auto, clear
```

(1978 Automobile Data)

```
. sum mpg
```

| Variable | Obs | Mean | Std. Dev. | Min | Max |
|---|---|---|---|---|---|
| mpg | 74 | 21.2973 | 5.785503 | 12 | 41 |

```
. ret list
scalars:
```

```
r(N)      =  74
r(sum_w)  =  74
r(mean)   =  21.2972972972973
r(Var)    =  33.47204738985561
r(sd)     =  5.785503209735141
r(min)    =  12
r(max)    =  41
r(sum)    =  1576
```

下面是做了一个简单的线性回归模型后 e() 所保存的结果。

```
. qui reg mpg weight price, vce(robust)
. eret list
scalars:
```

```
e(N)      =  74
e(df_m)   =  2
e(df_r)   =  71
e(F)      =  51.14373982809451
e(r2)     =  .6531446579233133
e(rmse)   =  3.454996314099513
e(mss)    =  1595.932492798133
e(rss)    =  847.5269666613266
e(r2_a)   =  .6433740849070686
e(ll)     =  -195.2169813478502
e(ll_0)   =  -234.3943376482347
e(rank)   =  3
```

macros:

```
e(cmdline)   : "regress mpg weight price, vce(robust)"
e(title)      : "Linear regression"
e(marginsok)  : "XB default"
e(vce)        : "robust"
e(depvar)     : "mpg"
e(cmd)        : "regress"
e(properties) : "b V"
e(predict)    : "regres_p"
e(model)      : "ols"
e(estat_cmd)  : "regress_estat"
e(vcetype)    : "Robust"
```

matrices:

e(b)  :  1 x 3
e(V)  :  3 x 3
e(V_modelbased)  :  3 x 3

functions:

e(sample)

## 3. 两种宏

在数据处理过程中，有时需要一些中间变量，但不想将它保存在数据中，或者有些语句需要重复书写，这时可以考虑使用 Stata 提供的宏命令（macro）。Stata 提供了两种宏：全局宏和局部宏。全局宏会在整个进程中有效，直到关闭 Stata 软件，局部宏只在相应程序中有效，程序结束，局部宏消失。一般应用中很少使用全局宏，因为全局宏比较危险，它可能会影响到所有程序中相同名字宏的调用，一般情况下不建议使用全局宏，除非用户非常清楚全局宏的影响。宏是用一个字符串代替另一个字符串，可以将它理解成一个盒子，而里面会装一些东西，调用时就是用盒子里的东西来替换宏。全局宏用 global 定义，用 $ 调用，局部宏用 local 定义，用 ‘ ’ 调用。① 下面定义了一个全局宏 xlist 和一个局部宏 xlist2，它们里面都装了字符串 price weight，调用时全局宏用 $ xlist，局部宏用 ‘xlist2’，都会将宏中的字符串取出来，可以利用 display “xlist”和 display “‘xlist2’”显示宏里面的字符串内容。

```
global xlist "price weight"
di "$xlist"
reg mpg $xlist // 等价于命令 reg mpg price weight
local xlist2 "price weight"
di "`xlist2'"
reg mpg `xlist2' // 等价于命令 reg mpg price weight
```

使用宏的好处是，有时要重复输入比较长的变量列表，可以用宏来进行代替，这样可以避免不同的模型中变量输入的错误，还可以节约程序输入的字符。另一个好处是，有时需要不断地调整一些参数，而这些参数如果在程序的不同位置，每次变更就会很麻烦，但是如果用宏代替，只需要在程序开始处，对宏的内容进行调整，则此后所有语句中使用的宏的内容也就相应变化。因而，灵活使用宏

> ① 注意这里的左单引号是 PC 键盘左上角～键上的点。

变量可以大大提高编程效率，降低错误率。

## 4. 三种循环语句

在数据处理过程中，经常遇到一些重复处理的工作，如果基本的处理过程相似，可以通过使用循环语句来提高效率。Stata 中提供了三种循环语句，分别是 foreach、forvalues 和 while。foreach 针对变量列表或宏列表，非常有弹性，进行循环的可以是没有任何规律的变量列表。forvalues 主要针对数值型的循环，while 更加自由，可以根据用户需要设计。下面看一个简单的循环，产生 4 个随机数，并把它们加和起来。先用下列语句生成 4 个标准均匀分布的随机数 x1、x2、x3、x4。

```
clear // 关闭系统中的数据
set obs 100 // 产生 100 个样本点
set seed 1234 // 设定随机产生器种子
gen x1 = runiform() // 产生一个标准均匀分布随机数
gen x2 = runiform()
gen x3 = runiform()
gen x4 = runiform()
```

下面，我们用三种循环语句构造求和，首先利用 foreach 实现：

```
gen sum = 0 // 产生一个变量 sum 作为后面计算和的变量
foreach var of varlist x1 x2 x3 x4 {
    qui replace sum = sum + `var'
}
di "The sum of x1 - x4 = " sum
| gen sum = 0
| local xlist "x1 x2 x3 x4"
| foreach var of local xlist {
|     qui replace sum = sum + `var'
| }
| di "The sum of x1 - x4 = " sum
```

上面两个程序都可以实现求和的效果，左边利用变量列表，右边是定义了一个局部宏，然后利用宏列表。可以看到 foreach 后面的参数是一个局部宏，调用时用单引号。

因为 4 个变量具有一定的规律性，其中的数字是按顺序的，因而，也可以利用 forvalues 来实现。

```
replace sum = 0 // 重置变量 sum 为零，作为后面计算和的变量
forvalues i = 1/4 {
```

```
qui replace sum = sum + x`i'
}
di "The sum of x1 - x4 = " sum
```

用 while 也可以实现。

```
replace sum = 0 // 重置 sum = 0
local i = 1 // 设定初始指针
while `i' <= 4 { // 注意这里条件设定，i 是局部宏，调用时用单引号
    qui replace sum = sum + x`i'
    local i = `i' + 1
}
di "The sum of x1 - x4 = " sum
```

上面举了个简单的小例子说明三种循环的使用，具体应用方法可以参考 Stata 的帮助文件或 Baum(2016)。

## 第三节 结果呈现利器：estout 软件包

在经济学实证分析中，往往需要进行变量的简单统计和估计结果呈现，如果估计的模型比较多，制作符合学术论文要求的表格往往需要花费一定的时间，瑞士苏黎世大学社会学研究所的 Ben Jann 提供了一个非常优秀的结果呈现软件包 estout，①共包括 esttab、estout、eststo、estadd、estpost 等命令。esttab、estout 命令将 Stata 估计命令保存在 e() 函数中的信息自动生成基本符合学术期刊要求的表格，稍加修改一般就可以用在学术论文中，对于不提供 e() 的命令，可以使用 estpost 收集相关结果，并将它们推送(post)到 e() 中用于制作表格。② eststo 类似于 Stata 的官方命令 estimate store，可以将估计结果保存起来用于后面的分析。estadd 可以将没有包含在 e() 函数中的结果添加到结果中去。因为 estout 软件包不是官方命令，使用之前需要通过命令 ssc install estout, replace 进行安装，或利用 findit estout 搜索后安装。

esttab 的命令语法如下：

```
esttab [ namelist ] [ using filename ] [ , options estout_options ]
```

> ① 关于 estout 软件包的详细介绍可以参见 http://repec.org/bocode/e/estout/index.html。
② postfile, post 是另一组有用的命令。具体用法可以参考 Cameron and Trivedi(2010)。

它的作用是将保存的回归结果输出成表格。由于参考选项很多，不再一一介绍，读者可以利用 help esttab 调出其帮助文件进行查询，下面举一些例子，管窥一下它的用途。首先利用 Stata 自带的数据 auto.dta 估计两个线性回归模型。将回归结果保存到 est1、est2 中，并用 esttab 命令将估计结果表格输出到 Stata 结果窗口。

```
. sysuse auto. clear // 打开系统自带数据 auto.dta
(1978 Automobile Data)
. eststo: qui regress mpg weight price, vce(robust) // 将回归结果保存到 est1 中
(est1 stored)
. eststo:qui regress mpg weight price mpg foreign, vce(robust) //将回归结果保存到 est2 中
(est2 stored)
. esttab // 将刚才估计的两个结果输出到屏幕
```

|  | (1) | (2) |
|  | mpg | mpg |
| weight | -0.00582*** | -0.00678*** |
|  | (-8.91) | (-7.49) |
| price | -0.0000935 | 0.0000566 |
|  | (-0.53) | (0.29) |
| foreign |  | -1.856 |
|  |  | (-1.44) |
| _cons | 39.44*** | 41.96*** |
|  | (19.56) | (17.65) |
| N | 74 | 74 |

t statistics in parentheses
* p<0.05, ** p<0.01, *** p<0.001

上面是 esttab 输出的结果，默认报告估计系数的 t 统计量，也可以通过改变相应选项换成标准误差(se)、p 值(p)、置信区间(ci)或估计中生成的任意参数统计量(参见选项 aux() 的说明)，可以让 esttab 报告调整的 R²(ar2)，伪 R²(pr2)、AIC(aic)和 BIC(bic)模型选择标准。另外，可以通过选项 scalars() 添加估计结果中保存的任何标量统计量，比如 p 值、F 值、模型自由度(df_m)和残差自由度(df_r)等，如果想让 t 统计量或标准误差与系数并排排列，可以加上选项 wide，输出宽表。

```
. esttab, wide se ar2 scalars(F df_m df_r)
```

(1)
mpg
(2)
mpg
weight -0.00582*** (0.000653) -0.00678*** (0.000905)
price -0.0000935 (0.000175) 0.0000566 (0.000192)
foreign -1.856 (1.289)
_cons 39.44*** (2.016) 41.96*** (2.378)
N 74 74
adj. R-sq 0.643 0.649
F 51.14 45.93
df_m 2 3
df_r 71 70

Standard errors in parentheses
* p<0.05, ** p<0.01, *** p<0.001

可以看到，上表的输出结果系数小数位比较长，可以通过修改系数（b(fmt)）、标准误差（se(fmt)）、拟合优度（ar2(fmt)）等参数输出格式改变输出小数位数。可以通过 nostar 选项禁止标出 *，也可以通过 star() 选项改变星号所代表的显著性水平，还可以通过加选项 mtitle() 改变列标题。

```
. esttab, b(%12.4f) se(%5.4f) ar2(%12.4f) star(* .10 ** .05 *** .01) > mtitle("OLS 1" "OLS 2") wide
```

(1)
OLS 1
(2)
OLS 2
weight -0.0058*** (0.0007) -0.0068*** (0.0009)
price -0.0001 (0.0002) 0.0001 (0.0002)
foreign -1.8559 (1.2891)
_cons 39.4397*** (2.0163) 41.9595*** (2.3777)
N 74 74
adj. R-sq 0.6434 0.6487

Standard errors in parentheses
* p<0.10, ** p<0.05, *** p<0.01

可以通过加 label 选项，让 esttab 报告变量的标签而非变量名，可以通过 nonumbers 去掉每列的数字，可以通过选项 title() 加表格标题，addnote() 增加脚注。有时需要添加一些 esttab 选项中没有定义的参数，可以利用命令 estadd 添加，利用 stats() 选项调用 estadd 构造的宏变量，用 scalars() 调用 es-
tadd 产生的标量。esttab 也可以将表格输出到文件中，利用 using 选项，可以将表格保存为 Excel 文件(.csv)、Word 文件(.rtf) 或 LaTeX 文件(.tex)。

```
. qui reg mpg weight price, vce(robust)
. estadd local Control_foreign = "No" //Control_foreign 显示是否控制 foreign
added macro:
    e(Control_foreign) : "No"
. eststo m1
. qui reg mpg weight price foreign, vce(robust)
. estadd local Control_foreign = "Yes" //Control_foreign 显示是否控制 foreign
added macro:
    e(Control_foreign) : "Yes"
. eststo m2
. esttab m1 m2, b(%12.4f) se(%5.4f) star(* .10 * * .05 * * * .01) mtitles ("OLS 1" " OLS 2") label stats(Control_foreign N r2_a, fmt(%3s %5.0f %5.4f)) title("A simple regression table") addnote("Source: auto.dta") wide
```

A simple regression table

|  | (1) OLS 1 |  | (2) OLS 2 |  |
|---|---|---|---|---|
| Weight (lbs.) | -0.0058*** | (0.0007) | -0.0068*** | (0.0007) |
| Price | -0.0001 | (0.0002) | 0.0001 | (0.0002) |
| Car type |  |  | -1.8559 | (1.4197) |
| Constant | 39.4397*** | (2.0163) | 41.9595*** | (1.7330) |
| Control_foreign | No |  | Yes |  |
| N | 74 |  | 74 |  |
| r2_a | 0.6434 |  | 0.6487 |  |

Standard errors in parentheses
Source: auto.dta
* p<.10, ** p<.05, *** p<.01

其中 stats(, [fmt()]) 调用 estadd 产生的参数和 esttab 默认的一些统计量，fmt() 设置对应参数的显示格式。如果加上 using filename.rtf 选项，上述结果将保存到文件 filename.rtf 中，不再显示在屏幕上，可以用 Word 打开 filename.rtf。下面的命令将上述表格保存在文件 table.rtf 中。
