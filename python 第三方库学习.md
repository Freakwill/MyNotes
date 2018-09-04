# 第三方库学习

[TOC]

## 网络

### wikipedia

#### 构造page类

```python
import wikipedia as wk
# construct <WikipediaPage 'Monotonic function'>
page = wk.WikipediaPage('Monotonic function')

print(page.summary)   # get the summary
print(page.content)   # get the content
print(page.section('See also'))  # the the content of one section
```



### bs4

#### classes

    Tag
    NavigableString
    BeautifulSoup (a spcial Tap)
    Comment (<NavigableString)

#### attributions

contens
children
descendants

#### find_all/find

```python
tag.find_all(name, attrs, recursive, text, **kwargs)
find_all() 方法搜索当前tag的所有tag子节点（符合过滤器的条件）
1）name 参数, 查找所有名字为 name 的tag
A.传字符串

soup.find_all('b')
# [<b>The Dormouse's story</b>]
 
soup.find_all('a')
#[<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>, <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>, <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

B.传正则表达式
Beautiful Soup 会通过正则表达式的 match() 来匹配内容. 下面例子中找出所有以b开头的标签, 这表示<body>和<b>标签都应该被找到
 
import re
for tag in soup.find_all(re.compile("^b")):
  print(tag.name)
# body
# b

C.传列表
soup.find_all(["a", "b"])
# [<b>The Dormouse's story</b>,
# <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
# <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
# <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

D.传 True
True 可以匹配任何值,下面代码查找到所有的tag, 但是不会返回字符串节点
 
for tag in soup.find_all(True):
  print(tag.name)
# html
# head
# title
# body
# p
# b
# p
# a
# a

E.传方法
方法只接受一个元素参数, 如果这个方法返回 True 表示当前元素匹配并且被找到

# 如果包含 class 属性却不包含 id 属性,那么将返回 True
def has_class_but_no_id(tag):
  return tag.has_attr('class') and not tag.has_attr('id')
 
soup.find_all(has_class_but_no_id)
# [<p class="title"><b>The Dormouse's story</b></p>,
# <p class="story">Once upon a time there were...</p>,
# <p class="story">...</p>]

2）keyword 参数
如果一个参数不是搜索内置（find_all参数）的参数名, 搜索时会把该参数当作指定名字tag的属性来搜索。

包含参数 id, Beautiful Soup 会搜索每个tag的 “id”属性
soup.find_all(id='link2')
# [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]

使用多个参数可以同时过滤tag的多个属性
soup.find_all(href=re.compile("elsie"), id='link1')
# [<a class="sister" href="http://example.com/elsie" id="link1">three</a>]

用 class 过滤
soup.find_all("a", class_="sister")  # 加下划线
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
# <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
# <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

有些tag属性在搜索不能使用, 比如HTML5中的 data-* 属性, 可以通过 attrs 参数定义一个字典参数来搜索包含特殊属性的 tag
data_soup.find_all(attrs={"data-foo": "value"})
# [<div data-foo="value">foo!</div>]

3）text 参数
搜搜文档中的字符串内容. 与 name 参数的可选值一样, text 参数接受字符串, 正则表达式, 列表, True

soup.find_all(text="Elsie")
# ['Elsie']

soup.find_all(text=["Tillie", "Elsie", "Lacie"])
# ['Elsie', 'Lacie', 'Tillie']

soup.find_all(text=re.compile("Dormouse"))
["The Dormouse's story", "The Dormouse's story"]

4）limit 参数
如果我们不需要全部结果, 可以使用 limit 参数限制数量. 

soup.find_all("a", limit=2)
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
# <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]
```



| find                    | find_all                        |
| ----------------------- | ------------------------------- |
| find_parent             | find_parents                    |
| find_next_sibling       | find_next_siblings              |
| find_previous_sibling   | find_previous_siblings          |
| find_next/find_previous | find_all_next/find_all_previous |

#### 遍历

```python
.next/previous_element  # 下一个节点
next/previous_sibling
contents  -> List
children  -> Generator
descendants
string
strings/stripped_strings
parent(s), next/previous_sibling(s)
```



#### CSS选择器

```python
soup.select("title")
# [<title>The Dormouse's story</title>]

soup.select("p nth-of-type(3)")
# [<p class="story">...</p>]
```



### feedparser

See [feedparser](https://pythonhosted.org/feedparser/common-rss-elements.html)

```python
# The channel elements are available in d.feed.
>>> import feedparser
>>> d = feedparser.parse('http://feedparser.org/docs/examples/rss20.xml')
>>> d.feed.title
'Sample Feed'
>>> d.feed.link
'http://example.org/'
>>> d.feed.description
'For documentation <em>only</em>'
>>> d.feed.published
'Sat, 07 Sep 2002 00:00:01 GMT'
>>> d.feed.published_parsed
(2002, 9, 7, 0, 0, 1, 5, 250, 0)

# The items are available in d.entries, which is a list
>>> d.entries[0].title
'First item title'
>>> d.entries[0].link
'http://example.org/item/1'
>>> d.entries[0].description
'Watch out for <span>nasty tricks</span>'
>>> d.entries[0].published
'Thu, 05 Sep 2002 00:00:01 GMT'
>>> d.entries[0].published_parsed
(2002, 9, 5, 0, 0, 1, 3, 248, 0)
>>> d.entries[0].id
'http://example.org/guid/1'
```



### furl

```python
>>> f=furl('www.baidu.com')  # furl obj
>>> f.add(path='path')       # furl('www.baidu.com/path')
>>> f.path
Path('www.baidu.com/path')
>>> f.path.segments
['www.baidu.com', 'path']
```



```python
f.url == f.tostr() == str(f)
```



## 计算

### pyahp

```python
from pyahp import *
model = json.load(open(pathto/television.json))  # or define it as a dict directly
ahp_model = parse(model)   # dict -> AHP Model Class
ahp_model.get_priorities() # get priorities
```



### scikit-fuzzy

#### import

```python
import skfuzzy as fuzz
```

#### fuzzy membership

```python
trimf(x, [])
trapmf(x, [])
smf
sigmf
gaussmf
pmf
zmf
...
```

#### fuzzy operation

```python
z, hz = fuzzy_add(x, fx, y, gy) # (x, fx) represents fuzzy set f
fuzz_sub/div/mult
fuzzy_min/max
fuzzy_and/or/not
...

fuzzy_compare
```

#### fuzzy logic

```python
classic_relation(a,b) # a->b
```

#### fuzzy control

```python
import numpy as np
import skfuzzy as fuzz
import skfuzzy.control as fuc

# New Antecedent/Consequent objects hold universe variables and membership functions

quality = fuc.Antecedent(np.arange(0, 11, 1), 'quality')
service = fuc.Antecedent(np.arange(0, 11, 1), 'service')
tip = fuc.Consequent(np.arange(0, 26, 1), 'tip')

# Auto-membership function population is possible with .automf(3, 5, or 7)
quality.automf(3)
service.automf(3)

# Custom membership functions can be built interactively with a familiar, Pythonic API
tip['poor'] = fuzz.trimf(tip.universe, [0, 0, 13])  # x is a poor tip/ tip x is poor
tip['average'] = fuzz.trimf(tip.universe, [0, 13, 25])
tip['good'] = fuzz.trimf(tip.universe, [13, 25, 25])

# You can see how these look with .view()
quality['average'].view()
service.view()
tip.view()

# Rule objects connect one or more antecedent membership functions with
# one or more consequent membership functions, using 'or' or 'and' to combine the antecedents.
#   * rule1: "If food is poor OR services is poor, then tip will be poor
#   * rule2: "If service is average, then tip will be average
#   * rule3: "If service is good OR food is good, then tip will be good
rule1 = fuzz.Rule(quality['poor'] | service['poor'], tip['poor'])
rule2 = fuzz.Rule(service['average'], tip['average'])
rule3 = fuzz.Rule(service['good'] | quality['good'], tip['good'])

# Create a new ControlSystem with these rules added
tipping_ctrl = fuzz.ControlSystem([rule1, rule2, rule3])

# View the whole system
tipping_ctrl.view()

tipping = fuzz.ControlSystemSimulation(tipping_ctrl)

# Pass inputs to the ControlSystem using Antecedent labels with Pythonic API
# Note: if you like passing many inputs all at once, use .inputs(dict_of_data)
tipping.input['quality'] = 6.5
tipping.input['service'] = 9.8

# Crunch the numbers
tipping.compute()

# Output available as a dict, for arbitrary number of consequents
print(tipping.output)
tipping.print_state()
# Viewing the Consequent again after computation shows the calculated system
tip.view(sim=tipping)
quality.view(sim=tipping)

###############
# More sophesticated system

# Inputs: qualtiy, service, decor
# Outpus: Tip
# Intermediary: ambiance
decor = fuzz.Antecedent(np.arange(0, 11, 1), 'decor')
decor.automf(3)

ambiance = fuzz.Consequent(np.arange(0, 11, 1), 'ambiance')
ambiance.automf(3)

# If service is poor and decor is not good, ambiance is poor
rule4 = fuzz.Rule(service['poor'] & ~decor['good'], ambiance['poor'])
rule2.view()

# If ambiance is poor, tip is poor, but at a 75% weight
rule5 = fuzz.Rule(ambiance['poor'], tip['poor']%.75)

sys2 = fuzz.ControlSystem([rule1, rule4, rule5])
sys2_sim = fuzz.ControlSystemSimulation(sys2)
sys2_sim.input['quality'] = 6.5
sys2_sim.input['service'] = 2.9
sys2_sim.input['decor'] = 3.5
sys2_sim.compute()

sys2.view()
sys2_sim.print_state()

a = 5
```



## 工具

### genanki

#### Model - Note Type

```python
my_model = genanki.Model(1607392319,      # unique model id
  'Simple Model',  # model name
  fields=[         # fields
    {'name': 'Question'},
    {'name': 'Answer'},
  ],
  templates=[    # or yaml format
    {
      'name': 'Card 1',
      'qfmt': '{{Question}}',
      'afmt': '{{FrontSide}}<hr id="answer">{{Answer}}',
    },
  ])
```



#### Deck

```python
my_deck = genanki.Deck(2059400110, 'Country Capitals')

my_deck.add_note(my_note)

genanki.Package(my_deck).write_to_file('output.apkg')
# then load output.apkg into Anki using File -> Import...
```



#### Note

```python
class MyNote(genanki.Note):
    @property
    def guid(self):
        # uniquely identifies the note, by default, the GUID is a hash of all the field values.
        return genanki.guid_for(self.fields[0], self.fields[1])
```



### Jieba

#### 切词

```python
jieba.cut  # 切词

# 词性标注
jieba.posseg.POSTokenizer(tokenizer=None) # 新建自定义分词器，tokenizer: 内部使用的 jieba.Tokenizer 分词器

>>> import jieba.posseg as pseg
>>> words = pseg.cut("我爱北京天安门")
>>> for word, flag in words:
...    print('%s %s' % (word, flag))
...
我 r
爱 v
北京 ns
天安门 ns

result = jieba.tokenize('永和服装饰品有限公司') -- mode='search' 搜索模式
for tk in result:
    print("word %s\t\t start: %d \t\t end:%d" % (tk[0],tk[1],tk[2]))
    

```

#### 手动调整

```python
# P(台中) ＜ P(台)×P(中)，“台中”词频不够导致其成词概率较低
# 解决方法：强制调高词频

jieba.add_word('台中') 或者 jieba.suggest_freq('台中', True)

# 解决方法：强制调低词频

jieba.suggest_freq(('今天', '天气'), True)

# 或者直接删除该词
jieba.del_word('今天天气')

# 解决方法：关闭新词发现

jieba.cut('丰田太省了', HMM=False) jieba.cut('我们中出了一个叛徒', HMM=False)

# 加在自定义字典
jieba.load_userdict("user.dict")
```





### jinja

#### 过滤器

```python
def mylen(arg):
    # 实现一个可以求长度的函数
    return len(arg)

def interval(test_str, start, end):
    # 该函数实现给定一个区间返回区间的内容
    # 过滤器中传递多个参数，第一个参数为被过滤的内容，第二第三个参数需要自己传入
    return test_str[int(start):int(end)]

# 注册自定义过滤器
env = app.jinja_env
env.filters['mylen'] = mylen
env.filters['interval'] = interval

```



#### 模板继承

你可以创建一个base.html作为基模板，把导航栏、页脚、flash消息、js或css文件等等**需要在每一个页面中显示的内容**放在基模板里，并**添加一个空的块**用来放置其他子模板的内容：

```jinja2
<!DOCTYPE html>
<html lang="en">
<head>
    {% block head %}
    <link rel="stylesheet" href="style.css" />
    <title>{% block title %}{% endblock %} - My Webpage</title>
    {% endblock %}
</head>
<body>
    <div id="content">{% block content %}{% endblock %}</div>
    <div id="footer">
        {% block footer %}
        &copy; Copyright 2008 by <a href="http://domain.invalid/">you</a>.
        {% endblock %}
    </div>
</body>
</html>
```

然后在其他的模板（子模板）里使用 `extends`继承它，并放置相应的内容到基模板里定义过的空块：

```jinja2
{% extends "base.html" %}
{% block title %}Index{% endblock %}
{% block head %}
    {{ super() }}
    <style type="text/css">
        .important { color: #336699; }
    </style>
{% endblock %}
{% block content %}
    <h1>Index</h1>
    <p class="important">
      Welcome to my awesome homepage.
    </p>
{% endblock %}
```

如果想添加内容到在父模板内已经定义的块，可以使用super函数, 避免覆盖父块的内容。



### nltk

```python

```



### pylatex

```python
# pylatex 利用函数实现escape
dumps_list(l, escape=True)  # 实现字符escape
dumps_list(l, escape=False)  # 禁止字符escape
```

