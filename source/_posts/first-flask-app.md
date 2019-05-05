---
title: 第一次使用flask
date: 2018-11-19 15:42:21
categories: 技术分享
tags:
    - flask
    - python
---

# 前言

看了某公众号出的题目后，跟着写了个简单计算个税的Python程序，想着怎么把它做成页面呢。正好看到有关flask的文章，就熟悉下这个简单的web框架。

<!-- more -->

# 代码

有两部分组成，一部分是Flask框架的，一部分是HTML的模板

``` python
# -*- coding: utf-8 -*-

# 导入模块
import sys
from flask import Flask, render_template, request, url_for

DEBUG = True
app = Flask(__name__)

# 计算个税的函数
def tax_calc(deduct_insurance):
    threshold = 5000
    money = deduct_insurance - threshold
    tax = 0

    calc_tables = (
        (80000, 0.45, 15160),
        (55000, 0.35, 7160),
        (35000, 0.3, 4410),
        (25000, 0.25, 2660),
        (12000, 0.2, 1410),
        (3000, 0.1, 210),
        (0, 0.03, 0)
    )

    for table in calc_tables:
        if money > table[0]:
            tax = money * table[1] - table[2]

    after_tax = deduct_insurance - tax
    return tax, after_tax


@app.route('/', methods=['POST', 'GET'])
def index():
    if request.method == 'POST':
        # 获取index.html中输入的money值
        deduct_insurance = eval(request.form['money'])
        # 调用函数计算赋值
        tax, result = tax_calc(deduct_insurance)
        # 传递个税的参数，并重新渲染页面
        return render_template('index.html', TAX = tax, RESULT = result)
    return render_template('index.html')


if __name__ == "__main__":
    app.run()
```

HTML模板

``` html
<!DOCTYPE html>
<html lang="en">
<head>
    {% block head %}
    <title>{% block title %}{% endblock %} 个税计算 </title>
    {% endblock %}
</head>
<body>
    <form method="POST">
        请输入缴纳五险一金后的工资：<br>
        <input type="text" name="money">
        <input type="submit" value="提交"><br><br>
        需要缴纳的个税是：
        <input type="text" name="tax" readonly="readonly" value="{{ TAX }}"><br>
        税后工资是：
        <input type="text" name="result" readonly="readonly" value="{{ RESULT }}"><br>
    </form>
</body>
</html>
```

# 结语

虽说代码是正常跑起来了，但很多都是囫囵吞枣。
仍需努力啊。