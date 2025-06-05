---
Type:
  - Page
aliases: 
Status:
  - archive
tags: 
modifiedDate: 星期一, 五月 26日 2025, 8:24:55 晚上
---
[本科毕业设计](本科毕业设计.md)

## 输入

**news_path**, news.tsv文件的位置
**args**,  全局参数
**tokenizer**, 分词器
**mode** 模式选择

## 处理流程

定义变量

```python
news = {} # 新闻字典
categories = [] # 种类
subcategories = [] # 子标题
domains = [] # 领域
news_index = {} # 字典
index = 1 #索引
```

按行读取文件
index += 1
\\t\\n分割每行，得到dco_id(nid),category, subcategory, title, abstract, body,\_, \_
url=""默认置空
news_index[doc_id] = index 按新闻id保存字典，键为id，值为index

```python
if 'title' in args.news_attributes:
    title = title.lower()
    title = tokenizer(title, max_length=args.num_words_title, \
    pad_to_max_length=True, truncation=True)
else:
    title = [] #若不想使用title
```

将title属性转为小写，设定最长token长度，允许截断
结果是一个list，存储着vector
对body，abstract操作相同

```python
if 'category' in args.news_attributes:
    categories.append(category)
else:
    category = None
```

对 categories,subcategory,domain（处理提取域名），添加到列表中

`news[doc_id] = [title, abstract, body, category, domain, subcategory]`
news字典：键为doc_id，值为新闻的各个属性
title，abstract，body为分词后的序列
catgory、domain、subcategory为字符串

### 如果mode = Train

```python
categories = list(set(categories))
category_dict = {}
index = 1
for x in categories:
    category_dict[x] = index
    index += 1
```

构建 categories字典（剔除重复值）

> [!NOTE]-
> 此代码段执行以下操作：
> 
> 1. 从 `categories` 列表中删除重复项，并将其存储在 `categories` 中。
> 2. 创建一个空字典 `category_dict`。
> 3. 遍历 `categories` 列表中的每个类别 `x`。
> 4. 将类别 `x` 作为键添加到 `category_dict` 中，并将 `index` 作为值。
> 5. 将 `index` 递增 1。
> 
> 结果是 `category_dict`，其中键是类别名称，值是唯一的数字索引。
> 
> 例如，如果 `categories` 列表包含以下类别：
>
> ```
> ['科技', '商业', '体育', '科技', '政治']
> ```
>
> 则 `category_dict` 将如下所示：
>
> ```
> {'科技': 1, '商业': 2, '体育': 3, '政治': 4}
> ```
>
> 此代码段对于机器学习或信息检索应用程序非常有用，因为它们允许你将类别名称转换为数字索引。这在计算和存储方面更有效，并且可以提高模型的性能。
> 
> 请注意，此代码段假定 `categories` 列表中的类别名称是唯一的。如果你有重复的类别名称，则需要修改代码以处理这种情况。

subcategories和domains操作相同

`return news, news_index, category_dict, domain_dict, subcategory_dict`

### 如果mode = Test

`return news, news_index`
