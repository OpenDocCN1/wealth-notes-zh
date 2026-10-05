# 17.1 Excel相关库简介

Python中用于处理Excel的库共有五个，具体如下。

+   - OpenPyxl推荐使用的库，可以处理Excel2010版本以上的文件。\n- xlsxwrite另一个库，可以处理Excel2010版本以上的文件。\n- xlrd读取旧版本的Excel（2003版及之前的版本）。\n- xlwt写较旧版本的Excel（2003版及之前版本）。\n- xlutils基于xlrd和xlwt的库，集成了部分相关函数。

为了统一标准，笔者建议，尽量使用OpenPyxl和Excel 2010以上的版本来进行数据处理的工作。除非必要情况下（比如要读取大量的Excel 2003文件），否则尽量不要使用其他的库。对于少量的Excel 2003文件，可以使用新版本的Excel转换为2010以上的版本，再做处理。