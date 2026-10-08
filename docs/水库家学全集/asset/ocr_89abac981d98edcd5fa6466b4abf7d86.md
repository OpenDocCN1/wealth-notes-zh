老何原本花费一整天的时间去搜索各种资料，而同事输入十几行代码，瞬间唰唰唰全下载到了桌面上！

```
import re

def read_excel(path):
    """
    读取Excel文件
    :param path: Excel文件路径
    :return: Excel中的数据
    """
    # 打开Excel
    workbook = xlrd.open_workbook(path)
    data_sheet = workbook.sheets()[0]
    row_num = data_sheet.nrows # sheet行数
    col_num = data_sheet.ncols # sheet列数
    list1 = []
    for i in range(1, row_num):
        rowlist = []
        for j in range(col_num):
            if j == 1:
                cell_value = xls.workbook_sheets[0].data_sheet.row_data_cell(i, j).datavalue
                rowlist.append(re.sub('<span>(.*?)</span>', r'\1', str(cell_value)))
            else:
                rowlist.append(data_sheet.cell_value(i, j))
            list1.append(rowlist)
    # 输出所有有效数据的内容
    return list1

def rs_word(xls, list1):
```

当老何还在被杂乱的销售数据折磨，同事倒杯水的功夫就自动生成。

![](bbox=[0.24, 0.39, 0.75, 0.64])

老何那一刻才醒悟：

他的加班，只是一场自我感动的狂欢。所谓忙碌，一直都是低效的挡箭牌。

聪明人早就学会用高效的工具了。就像走路走十天也不可能比开车快。

一次很偶然的机会，老何听到同事跟别人介绍，这东西叫Python。

就这样，他推掉了所有局，一心沉浸在Python中。这次再见面时，感觉对比过往他就像换了个人般，精神焕发。

他告诉我们，他现在很少加班，也有更多的时间去陪家人和学习新知识。

而且他工作的做事方法和效率反而比之前更好了，虽然这次无法晋升，但是技不如人没办法，好好争取下一次就行。