# Ownthink Filtering Rules
## Filtering By Edge
### Filter triples by special edge type

# News Data Source
* https://36kr.com/newsflashes
* https://www.iyiou.com/breakfeed/
* https://www.lieyunwang.com/news
* https://www.iheima.com
* http://news.winshang.com/newsletter/
* https://www.pintu360.com/news.html
* https://www.newseed.cn/quicklook
* https://www.pedaily.cn/first/
* https://www.qifengle.com/first/p1
* http://www.donews.com/business & http://www.donews.com/company 
* https://www.tmtpost.com/nictation/1
* http://www.startup-partner.com/cat/4
* https://www.cyzone.cn/category/8/~~
* https://www.jiemian.com/lists/84.html

# Filtering data about services from Ownthink (formatted as entity, attribute, value)

## 1  Filter by attribute

### 1.1  Specified attributes

#### 1.1.1  Example

​	Such as 'symbol', 'expression', 'cause'.

​	This way you can delete data in the form of potassium, symbol, K

#### 1.1.2  Code

```python
def screen(f: str, f2: str, fw: set, i: int):
    """
    把f文件写入f2文件，根据fw所含的过滤词筛选i列
    """
    t1 = time.time()
    with open(f, 'r', encoding='utf-8') as f:
        with open(f2, 'w', encoding='utf-8') as w:
            line_f, line_w = 0, 0
            for line in f:
                line_f += 1
                try:
                    if line.split(',')[i] in fw:
                        continue
                except:
                    continue
                w.write(line)
                line_w += 1
                print('当前循环次数', line_f)

            print(line_f, line_w)
    print('结束:', time.time() - t1)
```

#### 1.1.3  Filtered words:

​	'拼音', '人口数量', '种', '属', '界', '门', '化学式'

​	'拼 音', '前一个节气', '后一个节气', '出处', '拼音代码','近义词', '反义词', '拍謝'

​	'符号', '表达式', '产生原因', '所属专辑', '歌曲原唱', '歌曲时长'

​	'存储方法', '分辨率', '存储媒介', '读音', '歌曲语言', '口感'

​	'色彩', '票房', '专业课', '专业基础课', '基础课', '修行方式'

​	'主要原料', '副作用', '常见病因', '常见症状', '高度', '重量'

​	'就诊科室', '多发人群', '常见发病部位', '公式', '词性', '方程式'

​	'所属山系', '所属地区', '其它腿型', '主要营养成分', '主要食用功效', '著名品种'

​	'储藏方法,', '发明地区', '发明者', '适宜人群', '起源', '表现形式'

​	'出现形式', '来源', '现状', '同义词', '习俗', '用法'

​	'禁忌', '词目', '节日活动', '节日意义', '节日类型', '节日起源'

​	'化学式', '【拼音】', '【释义】', '【词性】', '汉语表达形式', '英语表达形式'

### 1.2  Delete data with attributes as label

#### 1.2.1  Example

​	Basic situation : with attributes as label

​	Such as : A,label,geography

​			         A,label,tourism

​	Code improvement: delete all words in filter words

​	Such as : A,contain,geography

​			         A,label,tourism

#### 1.2.2 Code and improved code

```python
def screen(f: str, f2: str):
    t1 = time.time()
    flag = True
    entity = ''
    temp_list = []
    filter_words = {'标签', '包含', '常见问题', '外文名称', '基本解释', '日文', '日语', '朝代', '词条类型', '原译', '承受者', '诱导因素', '学科', '德文', '地点',
                    '适用领域', '别名', '中文名称', '原版名称', '中文读音', '现指', '词义', '定义', '性质', '健康度', '属性', '储藏方法', '正音', '词意',
                    '多发群体', '出现', '形状', '特点', '简写', '又名', '适用领域范围', '类似名词', '中文名', '别称', '俗称', '分布区域', '所属', '学名', '术语',
                    '作用', '结构', '适用范围', '应用领域', '俚语', '具体表现', '释义', '英文名', '又称', '蒙古语', '英文简称', '全称', '类型', '宗教', '原意',
                    '组成', '实质', '原因', '意义', '其他称谓', '包括', '外语缩写', '职业', '简称', '相关名词', '属于', '分类', '类别', '组成成分', '全名',
                    '俄文', '法文', '节日时间', '英文缩写', '繁体', '用于', '开发语言', '同义名', '代表人物', '用处', '解释', '出自人', '应用类别', '英文名称',
                    '日本名', '主要种类', '本名', '应用范围', '信仰', '外文名', '源于', '用途', '功能', '时间', '韩文名', '应用', '其它译名'}
    with open(f, 'r', encoding='utf-8') as f:
        with open(f2, 'w', encoding='utf-8') as w:
            line_f, line_w = 0, 0
            for line in f:
                line_f += 1
                temp = line
                temp = temp.split(',')
                # 只删除标签
                # if entity != temp[0] and temp[1] == '标签':
                #     flag = False
                #     continue
                # elif entity == temp[0] and not flag and temp[1] == '标签':
                #     continue
                # 删除标签+别的
                if entity != temp[0]:
                    temp_list = []
                    if temp[1] in filter_words:
                        temp_list.append(line)
                        flag = False
                    else:
                        flag = True
                        w.write(line)
                        line_w += 1
                elif entity == temp[0] and not flag and temp[1] in filter_words:
                    temp_list.append(line)
                elif entity == temp[0] and not flag and temp[1] not in filter_words:
                    for i in temp_list:
                        line_w += 1
                        for j in i:
                            w.write(j)
                    line_w += 1
                    w.write(line)
                    temp_list = []
                    flag = True

                else:
                    flag = True
                    w.write(line)
                    line_w += 1
                entity = temp[0]
                print('当前循环次数', line_f)

            print(line_f, line_w)
    print('结束:', time.time() - t1)
```

## 2  Filter by entity

### 2.1  Remove entities containing filter words

#### 2.1.1  Example

​	Such as : '[a]'

​	This way you can delete data in the form of A[a],b,c

#### 2.1.2  Code

```python
def screen(f: str, f2: str, i: int):
    """
    将i列中含有con中词语的全部删除
    """
    t1 = time.time()
    con = {'[网络流行词]', '[汉字]', '[文学体裁]', '东汉末年', '[中国历史]', '[中国古代神话传说]', '[气体]', '奇函数', '三国时期', '[词语释义]', '[成语]', '海伦公式',
         '[学科]', '[佛教术语]', '[职位名称]', '[汉语词语]', '[宗教术语]', '南宋', '[体育运 动]', '[佛教用语]', '春秋', '战国时期', '[网络用语]', '[]'}
    with open(f, 'r', encoding='utf-8') as f:
        with open(f2, 'w', encoding='utf-8') as w:
            line_f, line_w = 0, 0
            for line in f:
                line_f += 1
                flag = True
                try:
                    for a in con:
                        if a in line.split(',')[i]:
                            flag = False
                            continue
                except:
                    continue
                if flag:
                    w.write(line)
                    line_w += 1
                    print('当前循环次数', line_f)

            print(line_f, line_w)
    print('结束:', time.time() - t1)
```

### 2.2  Delete specified entity

#### 2.2.1 Example

​	Such as : A

​	This way you can delete data in the form of A,b,c

#### 2.2.1 Code

​	Quote 1.2

```python
screen_attribute.screen(filename, filename2, filter_words, 0)
```