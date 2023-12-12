# 小说的数据表设计

## 摘要

拆分为两部分，一部分是小说系列，一部分是书本。

都存PostgreSQL里面。

同一本书的各种资源在对象存储中放一起。

小说系列和书本的主键都是自增整数，方便管理对象存储。

## 表结构

### 小说系列

放PostgreSQL

| 字段名         | 类型        | 备注       |
| ----------- | --------- | -------- |
| seriesId    | INT       | 自增主键     |
| title       | STRING    | 小说标题     |
| otherTitles | STRING\[] | 别名       |
| author      | STRING    |          |
| describe    | TEXT      |          |
| tags        | STRING\[] | 标签       |
| bookIds     | INT\[]    | 书本主键数组   |
| cover       | STRING    | 对象存储路径   |
| customProps | JSON      | 可拓展自定义属性 |

系列封面以哈希命名，单独保存在seriesCover目录。

### 书本

也是放PostgreSQL

| 字段名          | 类型        | 备注             |
| ------------ | --------- | -------------- |
| bookId       | INT       | 自增主键           |
| cover        | STRING    | 对象存储路径         |
| partName     | STRING    | 本卷名称           |
| chapterNames | STRING\[] | 章节名称           |
| capterPath   | STRING\[] | 章节对应的html文件的路径 |

书中的图片（包括封面）以哈希方式保存，和书本章节同目录

之后会提供epub直接下载的服务，提前生成好的epub文件与书本同目录，命名为系列名称+本卷名称
