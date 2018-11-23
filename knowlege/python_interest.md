##### 日期操作　
> Python time strptime() 函数根据指定的格式把一个时间字符串解析为时间元组。

``` python
def QA_util_date_valid(date):
    """
    判断字符串是否是 1982-05-11 这种格式
    :param date: date 字符串str -- 格式 字符串长度10
    :return: boolean -- 格式是否正确
    """
    try:
        time.strptime(date, "%Y-%m-%d")
        return True
    except:
        return False

```
