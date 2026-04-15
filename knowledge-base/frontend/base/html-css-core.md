# 检索索引：本文档收录【HTML/CSS】【核心基础】知识点，内容100%来自MDN官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

### HTML
HTML（HyperText Markup Language）是用于创建网页的标准标记语言。它不是编程语言，而是一种标记语言，通过标记来描述网页的结构和内容。

### CSS
CSS（Cascading Style Sheets）是一种样式表语言，用于描述HTML或XML文档的呈现。CSS描述了如何在屏幕、纸张或其他媒体上呈现元素。

## 2. 核心知识点（结构化层级）

### HTML核心知识点
1. HTML文档结构
   - doctype声明
   - html根元素
   - head元素（元数据）
   - body元素（内容）

2. 语义化标签
   - 文档结构标签：header, nav, main, footer, article, section, aside
   - 文本语义标签：h1-h6, p, em, strong, mark, blockquote
   - 列表标签：ul, ol, li, dl, dt, dd

3. 表单元素
   - input（text, password, email, number, checkbox, radio, submit, etc.）
   - select, option, optgroup
   - textarea
   - label, fieldset, legend
   - button

4. 多媒体元素
   - img
   - audio
   - video
   - figure, figcaption

5. 链接与导航
   - a（锚点）
   - href属性
   - target属性

### CSS核心知识点
1. CSS选择器
   - 基础选择器：元素选择器、类选择器、ID选择器、通配符选择器
   - 属性选择器
   - 伪类选择器
   - 伪元素选择器
   - 组合选择器：后代选择器、子选择器、相邻兄弟选择器、通用兄弟选择器

2. CSS盒模型
   - 标准盒模型
   - IE盒模型
   - box-sizing属性
   - margin, border, padding, content

3. 布局模式
   - 正常流
   - Flex布局
   - Grid布局
   - 定位（position）
   - 浮动（float）

4. 颜色与背景
   - color属性
   - background相关属性
   - 颜色表示方法：关键字、RGB/RGBA、HSL/HSLA、十六进制

5. 文本与字体
   - font相关属性
   - text相关属性
   - @font-face规则

6. 过渡与动画
   - transition
   - animation
   - @keyframes规则

## 3. 官方标准语法 / API

### HTML文档结构标准语法
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>页面标题</title>
</head>
<body>
    <!-- 页面内容 -->
</body>
</html>
```

### CSS选择器语法
```css
/* 元素选择器 */
element { property: value; }

/* 类选择器 */
.class-name { property: value; }

/* ID选择器 */
#id-name { property: value; }

/* 后代选择器 */
ancestor descendant { property: value; }

/* 子选择器 */
parent > child { property: value; }

/* 相邻兄弟选择器 */
prev + next { property: value; }

/* 通用兄弟选择器 */
prev ~ siblings { property: value; }

/* 属性选择器 */
[attribute] { property: value; }
[attribute="value"] { property: value; }

/* 伪类 */
element:pseudo-class { property: value; }

/* 伪元素 */
element::pseudo-element { property: value; }
```

### Flex布局语法
```css
.container {
    display: flex;
    flex-direction: row | row-reverse | column | column-reverse;
    flex-wrap: nowrap | wrap | wrap-reverse;
    justify-content: flex-start | flex-end | center | space-between | space-around | space-evenly;
    align-items: flex-start | flex-end | center | stretch | baseline;
    align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}

.item {
    flex-grow: <number>;
    flex-shrink: <number>;
    flex-basis: <length> | auto;
    flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ];
    align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```

## 4. 官方示例代码（可运行）

### HTML语义化页面示例
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>语义化页面示例</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            line-height: 1.6;
            margin: 0;
            padding: 0;
        }
        header { background: #333; color: white; padding: 1rem; }
        nav a { color: white; margin-right: 1rem; text-decoration: none; }
        main { padding: 2rem; max-width: 1200px; margin: 0 auto; }
        article { margin-bottom: 2rem; }
        footer { background: #333; color: white; text-align: center; padding: 1rem; margin-top: 2rem; }
    </style>
</head>
<body>
    <header>
        <h1>网站标题</h1>
        <nav>
            <a href="#home">首页</a>
            <a href="#about">关于</a>
            <a href="#contact">联系</a>
        </nav>
    </header>

    <main>
        <article>
            <h2>文章标题</h2>
            <p>这是文章的第一段内容。<em>强调文字</em>和<strong>重要文字</strong>。</p>
            <p>这是文章的第二段内容。</p>
        </article>

        <section>
            <h3>相关内容</h3>
            <ul>
                <li>列表项1</li>
                <li>列表项2</li>
                <li>列表项3</li>
            </ul>
        </section>
    </main>

    <footer>
        <p>&copy; 2024 网站版权信息</p>
    </footer>
</body>
</html>
```

### CSS Flex布局示例
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flex布局示例</title>
    <style>
        .flex-container {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background-color: #f0f0f0;
            padding: 20px;
            flex-wrap: wrap;
            gap: 10px;
        }

        .flex-item {
            background-color: #4CAF50;
            color: white;
            padding: 20px;
            text-align: center;
            flex: 1 1 200px;
        }

        .flex-item:nth-child(2) {
            flex: 2 1 200px;
            background-color: #2196F3;
        }
    </style>
</head>
<body>
    <div class="flex-container">
        <div class="flex-item">项目 1</div>
        <div class="flex-item">项目 2</div>
        <div class="flex-item">项目 3</div>
    </div>
</body>
</html>
```

### HTML表单示例
```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>表单示例</title>
    <style>
        form {
            max-width: 500px;
            margin: 2rem auto;
            padding: 2rem;
            border: 1px solid #ddd;
            border-radius: 8px;
        }
        .form-group {
            margin-bottom: 1rem;
        }
        label {
            display: block;
            margin-bottom: 0.5rem;
        }
        input, select, textarea {
            width: 100%;
            padding: 0.5rem;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        button {
            background-color: #4CAF50;
            color: white;
            padding: 0.75rem 1.5rem;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <form action="/submit" method="post">
        <fieldset>
            <legend>个人信息</legend>
            
            <div class="form-group">
                <label for="name">姓名：</label>
                <input type="text" id="name" name="name" required>
            </div>
            
            <div class="form-group">
                <label for="email">邮箱：</label>
                <input type="email" id="email" name="email" required>
            </div>
            
            <div class="form-group">
                <label for="gender">性别：</label>
                <select id="gender" name="gender">
                    <option value="">请选择</option>
                    <option value="male">男</option>
                    <option value="female">女</option>
                </select>
            </div>
        </fieldset>
        
        <div class="form-group">
            <label for="message">留言：</label>
            <textarea id="message" name="message" rows="4"></textarea>
        </div>
        
        <button type="submit">提交</button>
    </form>
</body>
</html>
```

## 5. 官方注意事项 / 最佳实践

### HTML最佳实践
1. 使用语义化标签提高可访问性和SEO
2. 始终添加doctype声明
3. 使用lang属性指定文档语言
4. 正确使用meta标签（charset、viewport等）
5. 为img标签添加alt属性
6. 使用label标签关联表单控件
7. 避免使用已废弃的标签和属性

### CSS最佳实践
1. 使用外部样式表而非内联样式
2. 遵循特定的命名约定（如BEM、OOCSS等）
3. 保持选择器的特异性低
4. 使用CSS变量管理重复值
5. 优先使用Flex和Grid进行布局
6. 考虑响应式设计，使用媒体查询
7. 避免使用!important除非必要

## 6. 官方文档参考链接

- HTML官方文档：https://developer.mozilla.org/zh-CN/docs/Web/HTML
- CSS官方文档：https://developer.mozilla.org/zh-CN/docs/Web/CSS
- HTML元素参考：https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element
- CSS属性参考：https://developer.mozilla.org/zh-CN/docs/Web/CSS/Reference
- Web开发指南：https://developer.mozilla.org/zh-CN/docs/Learn
