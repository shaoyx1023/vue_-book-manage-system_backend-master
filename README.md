## 项目简介

+ 主要使用Vue2和SpringBoot2实现
+ 项目权限控制分别为：用户借阅，图书管理员，系统管理员
+ 开发工具：IDEA2022.2.3

+ 用户账号密码：  syx 123456
+ 图书管理员账号密码:   admin 123456
+ 系统管理员账号密码:   root 123456

### 用户模块功能介绍

+ 图书查询功能：分页构造器缓解数据过大压力，后端可设置请求数防止爬虫请求数过大，服务器负载过大。模糊查询进行字段搜索。表格均可导出PDF和EXCEL。
+ 读者规则功能：查询现有的借阅规则，借阅规则包括：借阅编号，可借阅图书数量，可借阅天数，可借阅图书馆，过期扣费/天。
+ 查看公告: 可以查询图书管理员发布的公告列表，文字滑动

+ 个人信息: 可以查看个人的借阅证编号，借阅证姓名，规则编号，状态，可以修改个人账户的密码。

+ 借阅信息: 可以查看自身借阅过的图书记录和归还情况
+ 违章信息: 可以查询自身归还的图书是否有违章信息
+ 读者留言: 实现留言功能并以弹幕形式显示

### 图书管理员模块功能介绍

+ 借阅图书: 图书管理员输入借阅证号(用户)和要借的图书编号和当前的时间，点击借阅。
+ 归还图书: 输入图书编号查看图书是否逾期，并且可以设置违规信息，然后选择是否归还图书
+ 借书报表: 用于查询已经借阅并归还的书籍列表，同样使用分页构造器和模糊查询字段，显示借阅证编号，图书编号，借阅日期，截止日期，归还日期，违章信息，处理人。
+ 还书报表: 用于查询已经借阅但是还未归还的书籍列表，显示借阅证编号，图书编号，借阅日期，截止日期。

+ 发布公告: 可以查询当前发布的公告列表，并进行删除，修改，增加功能，分页构造器用于缓解数据量大的情况。

### 系统管理员模块功能介绍

+ 书籍管理: 可以查询当前的所有图书，显示图书编号，图书昵称，作者，图书馆，分类，位置，状态，描述。可以进行添加，修改，删除图书。利用分页构造器实现批量查询。利用模糊查询实现图书搜索功能。利用插件实现PDF和EXCEL导出。
+ 书籍类型: 显示查询当前的所有图书类型，可以进行添加，修改，删除图书类型，利用分页构造器实现批量查询，缓解数据压力。
+ 借阅证管理: 可以查询当前的所有借阅证列表，也就是用户数量，可以进行添加，修改，删除操作。同样实现分页。
+ 借阅信息查询: 可以查询当前已经完成借阅和归还的记录，显示借阅证号，书籍编号，借阅日期，截止日期，归还日期，违章信息，处理人。分页功能，PDF和EXCEL导出。
+ 借阅规则管理: 可以查询当前所有的借阅规则，显示限制借阅天数，限制本数，限制图书馆，逾期费用，可以进行添加、删除、修改操作。
+ 图书管理员管理: 显示当前的图书管理员列表，显示账号，姓名，邮箱，可以进行添加、删除、修改操作。
+ 系统管理: 可以查询一个月内的借阅量，以一周为时间间隔，计算借阅量，用Echarts实现折线图的展示。

## ️数据库表设计

### t_users表

| 列名        | 数据类型以及长度 | 备注                                              |
| ----------- | ---------------- | ------------------------------------------------- |
| user_id     | int(11)          | 主键 非空 自增 用户表的唯一标识                   |
| username    | varchar(32)      | 用户名 非空                                       |
| password    | varchar(32)      | 密码(MD5加密) 非空                                |
| card_name   | varchar(10)      | 真实姓名 非空                                     |
| card_number | Bigint(11)       | 借阅证编号 固定 11位随机生成 非空(后文都改BigInt) |
| rule_number | int(11)          | 规则编号 可以自定义 也就是权限功能                |
| status      | int(1)           | 1表示可用 0表示禁用                               |
| create_time | datetime         | 创建时间 Java注解 JsonFormatter                   |
| update_time | datetime         | 更新时间 Java注解 JsonFormatter                   |

### t_admins表

| 列名        | 数据类型以及长度 | 备注                              |
| ----------- | ---------------- | --------------------------------- |
| admin_id    | int(11)          | 主键 非空 自增 管理员表的唯一标识 |
| username    | varchar(32)      | 用户名 非空                       |
| password    | varchar(32)      | 密码(MD5加密) 非空                |
| admin_name  | varchar(10)      | 管理员真实姓名 非空               |
| status      | int(1)           | 1表示可用 0表示禁用               |
| create_time | datetime         | 创建时间 Java注解 JsonFormatter   |
| update_time | datetime         | 更新时间 Java注解 JsonFormatter   |

### t_book_admins表

| 列名            | 数据类型以及长度 | 备注                            |
| --------------- | ---------------- | ------------------------------- |
| book_admin_id   | int(11)          | 主键 非空 自增 管理表的唯一标识 |
| username        | varchar(32)      | 用户名 非空                     |
| password        | varchar(32)      | 密码(MD5加密)非空               |
| book_admin_name | varchar(10)      | 图书管理员真实姓名 非空         |
| status          | int(1)           | 1表示可用 0表示禁用             |
| email           | varchar(255)     | 电子邮箱                        |
| create_time     | datetime         | 创建时间 Java注解 JsonFormatter |
| update_time     | datetime         | 更新时间 Java注解 JsonFormatter |

### t_books表

| 列名             | 数据类型以及长度 | 备注                            |
| ---------------- | ---------------- | ------------------------------- |
| book_id          | int(11)          | 主键 自增 非空 图书表的唯一标识 |
| book_number      | int(11)          | 图书编号 非空 图书的唯一标识    |
| book_name        | varchar(32)      | 图书名称 非空                   |
| book_author      | varchar(32)      | 图书作者 非空                   |
| book_library     | varchar(32)      | 图书所在图书馆的名称 非空       |
| book_type        | varchar(32)      | 图书类别 非空                   |
| book_location    | varchar(32)      | 图书位置 非空                   |
| book_status      | varchar(32)      | 图书状态(未借出/已借出)         |
| book_description | varchar(100)     | 图书描述                        |
| create_time      | datetime         | 创建时间 Java注解 JsonFormatter |
| update_time      | datetime         | 更新时间 Java注解 JsonFormatter |

### t_books_borrow表

| 列名        | 数据类型以及长度 | 备注                                                         |
| ----------- | ---------------- | ------------------------------------------------------------ |
| borrow_id   | int(11)          | 主键 自增 非空 借阅表的唯一标识                              |
| card_number | int(11)          | 借阅证编号 固定 11位随机生成 非空 用户与图书关联的的唯一标识 |
| book_number | int(11)          | 图书编号 非空 图书的唯一标识                                 |
| borrow_date | datetime         | 借阅日期 Java注解 JsonFormatter                              |
| close_date  | datetime         | 截止日期 Java注解 JsonFormatter                              |
| return_date | datetime         | 归还日期 Java注解 JsonFormatter                              |
| create_time | datetime         | 创建时间 Java注解 JsonFormatter                              |
| update_time | datetime         | 更新时间 Java注解 JsonFormatter                              |

### t_notice表

| 列名            | 数据类型以及长度 | 备注                                |
| --------------- | ---------------- | ----------------------------------- |
| notice_id       | int(11)          | 主键 非空 自增 公告表记录的唯一标识 |
| notice_title    | varchar(32)      | 公告的题目 非空                     |
| notice_content  | varchar(255)     | 公告的内容 非空                     |
| notice_admin_id | int(11)          | 发布公告的管理员的id                |
| create_time     | datetime         | 创建时间 Java注解 JsonFormatter     |
| update_time     | datetime         | 更新时间 Java注解 JsonFormatter     |

### t_violation表

| 列名               | 数据类型以及长度 | 备注                                |
| ------------------ | ---------------- | ----------------------------------- |
| violation_id       | int(11)          | 主键 非空 自增 违章表记录的唯一标识 |
| card_number        | int(11)          | 借阅证编号 固定 11位随机生成 非空   |
| book_number        | int(11)          | 图书编号 非空 图书的唯一标识        |
| borrow_date        | datetime         | 借阅日期 Java注解 JsonFormatter     |
| close_date         | datetime         | 截止日期 Java注解 JsonFormatter     |
| return_date        | datetime         | 归还日期 Java注解 JsonFormatter     |
| violation_message  | varchar(100)     | 违章信息 非空                       |
| violation_admin_id | int(11)          | 违章信息管理员的id                  |
| create_time        | datetime         | 创建时间 Java注解 JsonFormatter     |
| update_time        | datetime         | 更新时间 Java注解 JsonFormatter     |

### t_comment表

| 列名                  | 数据类型以及长度 | 备注                                |
| --------------------- | ---------------- | ----------------------------------- |
| comment_id            | int(11)          | 主键 非空 自增 留言表记录的唯一标识 |
| comment_avatar        | varchar(255)     | 留言的头像                          |
| comment_barrage_style | varchar(32)      | 弹幕的高度                          |
| comment_message       | varchar(255)     | 留言的内容                          |
| comment_time          | int(11)          | 留言的时间(控制速度)                |
| create_time           | datetime         | 创建时间 Java注解 JsonFormatter     |
| update_time           | datetime         | 更新时间 Java注解 JsonFormatter     |

### t_book_rule表

| 列名               | 数据类型以及长度 | 备注                                  |
| ------------------ | ---------------- | ------------------------------------- |
| rule_id            | int(11)          | 主键 非空 自增 借阅规则记录的唯一标识 |
| book_rule_id       | int(11)          | 借阅规则编号 非空                     |
| book_days          | int(11)          | 借阅天数 非空                         |
| book_limit_number  | int(11)          | 限制借阅的本数 非空                   |
| book_limit_library | varchar(255)     | 限制的图书馆 非空                     |
| book_overdue_fee   | double           | 图书借阅逾期后每天费用 非空           |
| create_time        | datetime         | 创建时间 Java注解 JsonFormatter       |
| update_time        | datetime         | 更新时间 Java注解 JsonFormatter       |

### t_book_type表

| 列名         | 数据类型以及长度 | 备注                                  |
| ------------ | ---------------- | ------------------------------------- |
| type_id      | int(11)          | 主键 非空 自增 图书类别记录的唯一标识 |
| type_name    | varchar(32)      | 借阅类别的昵称 非空                   |
| type_content | varchar(255)     | 借阅类别的描述 非空                   |
| create_time  | datetime         | 创建时间 Java注解 JsonFormatter       |
| update_time  | datetime         | 更新时间 Java注解 JsonFormatter       |