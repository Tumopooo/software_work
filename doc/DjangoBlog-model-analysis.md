# DjangoBlog 数据模型分析

## 分析范围

- 项目：`liangliangyy/DjangoBlog`
- 目标：梳理主要数据库表与 Django Model，绘制第 3 周类图初稿。
- 文件：`doc/DjangoBlog-model.drawio`

## 核心实体

| 实体 | 作用 | 关键关系 |
| --- | --- | --- |
| `BlogUser` | 博客用户与作者 | 继承 `AbstractUser`，拥有文章和评论 |
| `Article` | 博客文章 | 属于作者和分类，与标签多对多 |
| `Category` | 文章分类 | 可包含父分类，支持树状层级 |
| `Tag` | 文章标签 | 与文章多对多 |
| `Comment` | 文章评论 | 属于用户和文章，可关联父评论 |
| `CommentReaction` | 评论互动 | 关联评论和用户，记录反应类型 |
| `OAuthUser` | 第三方账号 | 可选关联一个 `BlogUser` |

## 关系说明

1. `BlogUser 1:N Article`：一个用户可发布多篇文章。
2. `BlogUser 1:N Comment`：一个用户可发表多条评论。
3. `Category 1:N Article`：一个分类可包含多篇文章，文章分类允许为空。
4. `Article N:M Tag`：多篇文章可共享多个标签，由 Django 自动生成中间表。
5. `Article 1:N Comment`：一篇文章可包含多条评论。
6. `Comment 1:N CommentReaction`：一条评论可拥有多种用户反应。
7. `Comment 自关联`：通过 `parent_comment` 支持评论回复。
8. `Category 自关联`：通过 `parent_category` 支持分类层级。

## 数据库建模说明

- `AbstractUser` 与 `BaseModel` 是抽象基类，字段会进入具体模型表中，不会单独建表。
- 显式外键以 UML 关联线表示；Django 自动生成的多对多中间表只在图中标注，不展开为独立类。
- `OAuthUser.author` 可以为空，位于图右侧的配置和运维模型没有发现显式外键。
- 类图初稿只展示主要字段，后续详细设计可继续补充默认值、索引和删除策略。
