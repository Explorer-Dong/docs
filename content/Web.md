# 1、前端

## 1.1 HTML5

### 简介

<img src="https://www.runoob.com/wp-content/uploads/2013/06/02A7DD95-22B4-4FB9-B994-DDB5393F7F03.jpg" alt="img" style="zoom: 33%;" />

```
<p>左边是开始标签，右边是结束标签</p>
```

### 编辑器

- VS Code：https://code.visualstudio.com/
- Sublime Text：http://www.sublimetext.com/
- 在线编辑器：https://c.runoob.com/front-end/61/

### 基础

- 标题`<h1>这是一个标题</h1>`
- 段落`<p>这是一个段落。</p>`
- 链接`<a href="https://www.runoob.com">这是一个链接</a>`
- 图像`<img decoding="async" src="/images/logo.png" width="258" height="39" />`

### 元素

- 小写

### 属性

- 属性和属性值，尽量小写，本来这样做也方便些。
- class 属性可以多用 **class=" "** （引号里面可以填入多个class属性）
- id 属性只能单独设置 **id=" "**（只能填写一个，多个无效）
- [属性参考列表](https://www.runoob.com/tags/ref-standardattributes.html)

### 标题 & 水平线 & 注释 & 查看源代码

- `<h1>这是一个标题。</h1>`
- 浏览器会自动地在标题的前后添加空行
- 水平线`<hr>`
- 注释`<!-- 这是一个注释 -->`
- 查看源码`只需要单击右键，然后选择"查看源文件"（IE）或"查看页面源代码"（Firefox）`

### 段落

- 换行`<br/>`
- 多余的空格回车都会被识别为一个空格

### 文本格式化

- 加粗、上下标...

### 链接

- 基本语法

  ```html
  <a href="url">链接文本</a>
  <a href="https://www.runoob.com/">访问菜鸟教程</a>
  ```

- target属性

  ```html
  <a href="https://www.runoob.com/" target="_blank">新开一面展示相关内容</a>
  <a href="http://www.runoob.com/" target="_top">跳出框架，返回主页</a> 
  ```

- id属性（内部链接）

### 头部

- 使用 `<title>` 标签定义`HTML`文档的标题

- 在 `<head>`元素中你可以插入脚本（scripts）, 样式文件（CSS），及各种meta信息。

  可以添加在头部区域的元素标签为: `<title>, <style>, <meta>, <link>, <script>, <noscript> 和 <base>`。

- head 标签用于定义文档头部，它是所有头部元素的容器。<head> 中的元素可以引用脚本、指示浏览器在哪里找到样式表、提供元信息等等。

  如：

  ```html
  <html>
    <head>
       <title>文档标题</title>
    </head>
  </html>
  ```

  header 标签用于定义文档的页眉（介绍信息）。

  如：

  ```html
  <html>
    <body>
      <header>
          <p>段落</p>
          <h1>一级标题</h1>
      </header>
    </body>
  </html>
  ```

  注意千万不要弄混。

- `<base>`元素，类似于全局设定前缀

  ```html
  <!DOCTYPE html>
  <html>
  <head>
  <meta charset="utf-8"> 
  <title>菜鸟教程(runoob.com)</title> 
  <base href="https://www.runoob.com/images/" target="_blank">
  </head>
  
  <body>
  <img src="logo.png"> - 注意这里我们设置了图片的相对地址。能正常显示是因为我们在 head 部分设置了 base 标签，该标签指定了页面上所有链接的默认 URL，所以该图片的访问地址为 "https://www.runoob.com/images/logo.png"
  <br><br>
  <a href="https://www.runoob.com">菜鸟教程</a> - 注意这个链接会在新窗口打开，即便它没有 target="_blank" 属性。因为在 base 标签里我们已经设置了 target 属性的值为 "_blank"。
  
  </body>
  </html>
  ```

- `<link>`元素

  1. `<link>`标签定义了文档与外部资源之间的关系

  2. `<link> `标签通常用于链接到样式表

  3. 比如

     ```html
     <head>
     <link rel="stylesheet" type="text/css" href="mystyle.css">
     </head>
     ```

- `<meta>`元素

  1. meta元素通常用于指定网页的描述，关键词，文件的最后修改时间，作者，和其他元数据。

  2. 例子

     - 为搜索引擎定义关键词:

     ```html
     <meta name="keywords" content="HTML, CSS, XML, XHTML, JavaScript">
     ```

     - 为网页定义描述内容:

     ```html
     <meta name="description" content="免费 Web & 编程 教程">
     ```

     - 定义网页作者:

     ```html
     <meta name="author" content="Runoob">
     ```

     - 每30秒钟刷新当前页面:

     ```html
     <meta http-equiv="refresh" content="30">
     ```

- `<style>`元素

  1. 定义了文档的样式文件CSS引用地址

- `<script>`元素

  1. 用于加载脚本文件，如`JavaScript`


### CSS

CSS 可以通过以下方式添加到HTML中:

- 内联样式

  > 在HTML元素中使用`"style"` **属性**

- 内部样式表

  > 在HTML文档头部 `<head>` 区域使用`<style>` **元素** 来包含CSS

- 外部引用

  > 使用外部 CSS **文件**



### pink老师

- \<h1>这中间是放置标题，与Markdown类似有六级标题\</h1>

- \<p>放置段落，每个段落之间会有默认的空行\</p>

- \<br />强制换行

- \<strong>加粗\</strong>

- \<em>倾斜\</em>

- \<del>删除\</del>

- \<ins>下划线\</ins>

- \<div>一整行布局\</div>

- \<span>一小块一小块布局\</span>

- \<img src="img.jpg" alt="这是一张照片哦" title="这是代码部分" width="100" border="10" />

    > `src`属性是图片在文件夹中的路径
    >
    > - 相对路径
    >     - 同级路径直接图片名即可
    >     - 下一级路径加上文件夹名即可，如`images/img.jpg`
    >     - 上一级路径加上`../`跳出即可，如`../images/img.jpg`
    > - 绝对路径
    >     - 网址
    >
    > `alt`属性是图片无法正常显示时显示的替换文字
    >
    > `title`属性是鼠标移动到图片上时显示的小字
    >
    > `width`属性是调整图片宽度的，一般与`height`对应，只调一个，另一个等比例缩放
    >
    > `border`属性一般通过`CSS`文件设置

- \<a href="https://www.csdn.net/" target="_blank">显示的元素\</a>

    > `href`属性是链接地址，可以是**外部链接**，也可以是**内部**其他`html`文件的链接地址，也可以是**空链接**`#`表示待完善的地址，也可以是**下载链接**，一般放置文件作为链接地址，点击时直接下载
    >
    > `target`属性是页面跳转属性，一般是跳转到新的页面，设置为`_blank`
    >
    > `显示的元素`可以是**文字、图片**等等

- 锚点链接\<a href="#name">这里写锚点，一般是页面内目录位置\</a>

    > 对应的标签在想要跳转的内容标签中加一个`id="name"`即可

- \<!--注释内容-->快捷键老样子`Ctrl+/`

- 特殊字符

    <img src="https://s2.loli.net/2023/08/27/wUzX3LYuhyvJHnj.png" alt="image-20230515202739453" style="zoom:50%;" />  

- 表格

    >     表格==标签==（展示数据而非布局页面）
    >
    >     ```html
    >     <table>
    >         <tr>
    >             <td>学院</td>
    >             <td>年级</td>
    >             <td>姓名</td>
    >         </tr>
    >         <tr>
    >             <td> 计电院 </td>
    >             <td> 2022 </td>
    >             <td> 董文杰 </td>
    >         </tr>
    >     </table>
    >     ```
    >
    >     ​	![image-20230515205740156](https://s2.loli.net/2023/08/27/J8tnqiK2uszG6kb.png)
    >
    >     ---
    >
    >     表头`table head`单元格\<th>元素\</th>自动实现**加粗**与**居中**
    >
    >     ​	![image-20230516160701767](https://s2.loli.net/2023/08/27/QWnGO8brteV21oR.png)
    >
    >     ---
    >
    >     表格==属性==\<table align="center" border="1" cellpadding="5" cellspacing="0" width="500">
    >
    >     ​    <img src="https://s2.loli.net/2023/08/27/tCvUGEzbg35ncfN.png" alt="image-20230516161402112" style="zoom:50%;" />
    >
    >     ---
    >
    >     表格头部\<thead>放置表头标签\</thead>与身体\<tbody>放置表体标签\</tbody>的结构
    >
    >     ---
    >
    >     ==合并单元格==
    >
    >     1. 确定跨行`rowspan="格子数"`还是跨列`colspan="格子数"`
    >     2. 在最左侧或最上侧的单元格中加入上述属性
    >     3. 删除多余的单元格

- 列表

    > **布局页面**
    >
    > 1. **==无序列表==**
    >
    >     > ​	<img src="https://s2.loli.net/2023/08/27/IJRxLmgQiPsk4WV.png" alt="image-20230516164938661" style="zoom:50%;" />
    >     >
    >     > ​	<img src="https://s2.loli.net/2023/08/27/elRiE8ZbhuS1GJD.png" alt="image-20230516165006697" style="zoom:50%;" />
    >
    > 2. ==有序列表==
    >
    >     > 有序列表只是将\<ul>\</ul>写成\<ol>\</ol>
    >
    > 3. ==自定义列表==
    >
    >     > ```html
    >     > <h4>这是一个自定义列表</h4>
    >     > <dl>
    >     >  <dt>这是大哥</dt>
    >     >  <dd>这是第一个小弟</dd>
    >     >  <dd>这是<a href="404.html">第二个小弟</a></dd>
    >     >  <dd>这是第三个小弟</dd>
    >     > </dl>
    >     > ```
    >     >
    >     > ​	<img src="https://s2.loli.net/2023/08/27/RzLJVlCqUwtcYBQ.png" alt="image-20230516170158910" style="zoom:50%;" />

- 表单（收集用户信息）

    > ​	<img src="https://s2.loli.net/2023/08/27/EgaCbeLGvxT5HJ1.png" alt="image-20230518222333590" style="zoom:50%;" />
    >
    > 构成
    >
    > ​	<img src="https://s2.loli.net/2023/08/27/G5BQfMJy3mPNaYL.png" alt="image-20230518222523325" style="zoom: 33%;" />
    >
    >  1. 表单域
    >
    >     ```html
    >     <form action="demo.php" method="post" name="name1">
    >     
    >     </form>
    >     ```
    >
    >     - action标签代表传输数据的URL地址
    >     - method标签代表传输方式
    >     - name标签代表表单名称
    >
    >  2. 表单控件
    >
    >     - 输入表单元素
    >
    >         <img src="https://s2.loli.net/2023/08/27/dNILSOgeJXyUVta.png" alt="image-20230518224354386" style="zoom: 50%;" />
    >
    >         ```html
    >         <form>
    >             用户名：<input type="text"> <br />
    >             密码：<input type="password">
    >         </form>
    >         ```
    >
    >         ​	<img src="https://s2.loli.net/2023/08/27/pS5AmckiDTn9WVN.png" alt="image-20230518224717331" style="zoom:50%;" />
    >
    >         <img src="https://s2.loli.net/2023/08/27/2mpfHC1gOdDVGWY.png" alt="image-20230518230011931" style="zoom: 67%;" />
    >
    >     - 下拉表单元素
    >
    > 
    >
    >     - 文本域元素
    >
    >  3. 提示信息
    >
    >     TODO（由于要与数据库后端相关联，故先搁置）

- 查阅资料

    > 菜鸟教程

## 1.2 CSS3

## 1.3 JavaScript



# 2、后端

## 2.1 flask

# 3、数据库

## 3.1 ElasticSearch



ELK组件

![image-20230604104859769](https://s2.loli.net/2023/08/27/1jne3Ioh24b6wFg.png)

---

正向索引与倒排索引

![image-20230604111402280](https://s2.loli.net/2023/08/27/FusazwLeWoimb4T.png)

- 正向索引：

    1. 存储结构：按照每一个信息段，整条存储为一个文档

    2. 搜索形式：根据用户输入的词条在存储的每一个文档中进行匹配，记录符合的id信息

- 倒排索引：

    1. 存储结构：首先将每一个信息段进行语义分割（中文按照词义，英文按照空格），然后将每一个分割的单元创建唯一的文档，同时记录原始文档的ID（数据量小可以用哈希表，数据量大可以用B+树实现）
    2. 搜索形式：首先将用户输入的进行语义分割，将分割出来的所有关键词进行倒排索引，拿一个分割出来的单词举例->首先将这个单词在文档中进行检索（盲猜$O(1)$，比如$term[华为]$，就可以检索了，然后记录返回值，即相应的文档id即可）

- 个人小结：感觉正向索引就相当于朴素遍历，而倒排索引就是舍弃存储空间建立一个哈希表，最后根据哈希值进行用户展示

---

架构：ES与数据库的关系

![image-20230604113740076](https://s2.loli.net/2023/08/27/sxKrHwWOC8pf21g.png)

---

技术框架：
1. Flask作为Web框架，处理前端请求和后端逻辑
2. Elasticsearch作为数据库，存储诗文数据
3. 使用Flask-SQLAlchemy作为ORM,连接Elasticsearch
4. 使用HTML、CSS、JavaScript等前端技术展示搜索结果

流程设计：
1. 安装所需库：Flask、Flask-SQLAlchemy、Elasticsearch等
```bash
pip install flask flask-sqlalchemy elasticsearch
```
2. 创建Flask应用，配置数据库连接
```python
from flask import Flask, render_template, request, jsonify
from flask_sqlalchemy import SQLAlchemy
import elasticsearch

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'elasticsearch://localhost:9200/my_index'
db = SQLAlchemy(app)
es = elasticsearch.Elasticsearch()
```
3. 定义诗文模型，映射到Elasticsearch中的索引
```python
class Poem(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(100), nullable=False)
    content = db.Column(db.Text, nullable=False)
    es_index = db.Column(db.String(50), nullable=False)
    es_doc_type = db.Column(db.String(50), nullable=False)
```
4. 创建路由，处理用户输入的查询信息，调用Elasticsearch进行检索，返回相应的列表展示
```python
@app.route('/search', methods=['GET'])
def search():
    query = request.args.get('query')
    if not query:
        return jsonify([])
    res = es.search(index='my_index', body={"query": {"match": {"content": query}}})
    results = [{'title': hit['_source']['title'], 'content': hit['_source']['content']} for hit in res['hits']['hits']]
    return jsonify(results)
```
5. 运行Flask应用，访问`http://localhost:5000/search?query=关键词`进行搜索。





## 3.2 MySQL

### 检查FLask是否连接到了mysql数据库

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy
from sqlalchemy import text  # 导入text函数

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql+pymysql://root:123456@localhost:3306/poem'
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
db = SQLAlchemy(app)

def check_database_connection():
	try:
		# 使用text函数明确声明SQL文本表达式
		db.session.execute(text("SELECT 1"))
		return True  # 如果连接成功，则返回True
	except Exception as e:
		print("Database connection failed:", str(e))
		return False  # 如果连接失败，则返回False

@app.route('/')
def index():
	if check_database_connection():
		return "Connected to MySQL database!"
	else:
		return "Failed to connect to MySQL database!"

if __name__ == "__main__":
	app.run(debug=True)
```

# 4、服务器

## 4.1 笔记

### 4.1.1 云服务器与实例

一个云服务器相当于一个抽象类的类，在其中购买配置了指定的实例后相当于实例化一个类，从而一个云服务器对应一个实例

### 4.1.2 单服务器+单域名=多网站

一台云服务器可以【通过添加多个文件夹】挂载多个网站，像这样

![image-20230826011328462](https://s2.loli.net/2023/08/27/fwd8NXg2hDPvLsK.png)

而多个网站又可以【通过不同的主机记录值】只依赖一个主域名，像这样

<img src="https://s2.loli.net/2023/08/27/odjR69UYpuwLOG4.png" alt="image-20230826011106105" style="zoom:50%;" />

从而实现了【一台云服务器 + 一个域名 = 多功能网站】

### 4.1.3 关于域名解析延时与80端口

在给购买好的域名进行解析的时候，即指向自己服务器的公网IP的时候，可能会有一段时间的延时。但其实可能是没有给云服务器放通80端口导致的

### 4.1.4 关于SSL证书

- http协议默认使用的是80端口，
- 而申请了SSL证书后，通过https协议访问的网站默认使用的是443端口，因此需要提前在实例的**安全组**中，放通443端口

### 4.1.5 关于SSL证书的签发

- 如果是基于bt面板操作的话。可以通过ssl选项中“Let's Encrypt”的选项免费申请三个月的用量

- 如果想要通过官方渠道获取。可以在阿里云或者腾讯云等直接免费领取一定额度的ssl证书

- 由于是基于bt面板管理，需要打开**强制通过https进入**这个选项，从而直接默认使用https协议访问网站

### 4.1.6 关于SSL证书的部署

获得已签发的ssl证书后，下载下来，再通过bt面板进行部署

- 证书文件：pem文件
- 密钥文件：key文件

![image-20230826104235878](https://s2.loli.net/2023/08/27/qnCeXZwEHIh35fd.png)

### 4.1.7 关于Linux的发行版本

CentOS、Ubuntu和Deepin都是不同的Linux发行版本

### 4.1.8 遇上实例无法远程连接服务器的情况

直接停止实例，用快照备份一下，重装操作系统，配置一样即可

### 4.1.9 关于LNMP和LAMP

- LNMP是一组Linux操作系统下的Nginx、MySQL、PHP和Perl的组合安装包，常用于构建高性能的Web服务器。通过使用LNMP，可以快速搭建一个功能强大的网站系统
- LAMP是指Linux、Apache、MySQL和PHP的组合，它是一个开源的Web开发平台。这个组合通常被用来构建高性能的Web应用程序

### 4.1.10 关于bt面板安装环境耗费的时间

<img src="https://s2.loli.net/2023/08/27/Qf4eCdbNDRjJBgs.png" alt="image-20230826190642441" style="zoom:67%;" />

### 4.1.11 备案

需要材料

### 4.1.12 二级域名与子域名

二级域名就比如“dwj601.com.cn”域名结构中，“.com”此时是置于国家顶级域名“.cn”下的二级域名

子域名就比如“docs.dwj601.cn”、“www.dwj601.cn”就是“dwj601.cn”的子域名

## 4.2 我的服务器 Dwjdemo1

### 4.2.1 服务器配置

- 操作系统：Alibaba Cloud Linux 3.2104 LTS 64位

- 运行内存：1核2G

- 系统内存：40G

### 4.2.2 服务器信息

```
[root@DwjDemo1 ~]# cat /etc/os-release

NAME="Alibaba Cloud Linux"								发行版的名称
VERSION="3 (Soaring Falcon)"							发行版的版本号
ID="alinux"												唯一的标识符
ID_LIKE="rhel fedora centos anolis"						一些类似的发行版
VERSION_ID="3"											发行版的版本编号
PLATFORM_ID="platform:al8"								平台的标识符
PRETTY_NAME="Alibaba Cloud Linux 3 (Soaring Falcon)"	可读的发行版名称和版本号
ANSI_COLOR="0;31"										ANSI终端输出的颜色: "0;31"，通常用于表示错误或警告信息
HOME_URL="https://www.aliyun.com/"						发行版的官方网站链接
```

### 4.2.3 BT面板登录

- username: dwjadmin
- password: dwjadmin

# 5、Docsify

## 5.1 什么是Node.js

Node.js是一个基于 Chrome V8 引擎的 JavaScript 运行环境
