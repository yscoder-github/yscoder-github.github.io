从英文意思上看，encode和decode分别指编码和解码。在python中，Unicode类型是作为编码的基础类型，即：

decode           encode  
str ---------> str(Unicode) ---------> str


``` python
u = '中文'                 # 指定字符串类型对象u

>>> str1 = u.encode('gb2312')  # 以gb2312编码对u进行编码，获得bytes类型对象
>>> print(str1)
b'\xd6\xd0\xce\xc4'

>>> str2 = u.encode('gbk')     # 以gbk编码对u进行编码，获得bytes类型对象
>>> print(str2)
b'\xd6\xd0\xce\xc4'
>>> str3 = u.encode('utf-8')   # 以utf-8编码对u进行编码，获得bytes类型对象
>>> print(str3)
b'\xe4\xb8\xad\xe6\x96\x87'

>>> u1 = str1.decode('gb2312') # 以gb2312编码对字符串str进行解码，获得字符串类型对象
>>> print('u1')
'中文'

>>> u2 = str1.decode('utf-8')  # 报错，因为str1是gb2312编码的
UnicodeDecodeError: 'utf-8' codec can't decode byte 0xd6 in position 0: invalid continuation byte
```
