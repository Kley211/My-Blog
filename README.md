# My Person Blog System
1.搭建 Spring Boot 项目：

使用 Spring Initializr 生成项目。

关键依赖：Spring Web (提供MVC), MyBatis Framework, MySQL Driver, Spring Data Redis, Spring Security (用于后台登录), Thymeleaf (如果你用它来渲染前台页面)。

将生成的项目文件解压到你本地的 personal-blog-system 文件夹中。

2.设计数据库 (MySQL)：

article (文章表)：id, title, content (用 TEXT 或 LONGTEXT), status (状态, 如 'draft', 'published'), create_time, update_time。

category (分类表)：id, name。

tag (标签表)：id, name。

(可能还需要关联表，如 article_category_relation)

3.先实现后台 (Admin)：

为什么先做后台？ 因为没有后台，前台就没数据可展示。

Step 1: 配置 Spring Security。先花点时间，让 /admin/ 路径受保护，并实现一个最简单的登录功能。

Step 2: 实现文章管理。编写 "发布新文章" 的 Controller, Service, Mapper(MyBatis)，确保你能登录后台，并通过一个表单成功向 MySQL 数据库插入一篇文章。

4.再实现前台 (Public)：

编写首页的 Controller，从数据库查询所有“已发布”的文章，并在页面上展示出来。

实现文章详情页。

最后集成 Redis：

在实现“前台”展示时，加入缓存逻辑。例如，当访问一篇文章时：

先去 Redis 查 article:1 (key)

如果 Redis 有，直接返回。

如果 Redis 没有，去 MySQL 查，查到后，存入 Redis (并设置一个过期时间，比如1小时)，再返回。

当你“修改”文章时（在后台），必须记得删除 Redis 中对应的缓存 (article:1)，以保证数据一致性。
