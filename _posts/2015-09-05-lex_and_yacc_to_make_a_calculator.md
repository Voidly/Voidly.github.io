---
layout: post
title: "用lex和yacc制作简单计算器"
description: 
modified: 2015-09-05
category: Compilation
tags: [Compilation, lex, yacc]
comments: true
mathjax: 
---
{% highlight linenos %}
{% endhighlight %}
##简介
最近想玩下编译，那么在这之前先了解了下脚手架，其中包括了lex和yacc，下面简单介绍下lex和yacc。
###lex
Lex是LEXical compiler的缩写，是Unix环境下非常著名的工具,主要功能是生成一个词法分析器(scanner)的C源码,描述规则采用正则表达式(regular expression)。
###yacc
yacc(Yet Another Compiler Compiler)，是一个经典的生成语法分析器的工具。是Unix/Linux上一个用来生成编译器的编译器（编译器代码生成器）。yacc生成的编译器主要是用C语言写成的语法解析器（Parser），需要与词法解析器Lex一起使用，再把两部份产生出来的C程序一并编译。yacc本来只在Unix系统上才有，但现时已普遍移植往Windows及其他平台。

好了，今天打算先用这两个简单的做个计算器，只是简单的实现一下四则运算而已，一则了解下工具lex和yacc，二则练练手。

##准备工作

环境：Mac OS

* 安装lex:`brew install flex`
* 安装yacc：`brew install byacc`

PS：其中`flex`是`lex`的改进版本，`byacc`是伯克利分校的`yacc`，用法同`yacc`。

##实现
首先建立个文件夹，名为`calcu`。接下来写`lex`文件，后缀名是`.l`，这里命名为`calc.l`。

{% highlight c linenos %}
%{
#include <stdio.h>
#include "y.tab.h"

int 
yywrap(void)
{
    return 1;
}
%}

%%

"+"     return ADD;
"-"     return SUB;
"*"     return MUL;
"/"     return DIV;
"\n"    return CR;
([1-9][0-9]*)|0|([0-9]+\.[0-9]*) {
    double temp;
    sscanf(yytext, "%lf", &temp);
    yylval.double_value = temp;
    return DOUBLE_LITERAL;
}
[ \t];
. {
    fprintf(stderr, "lexical error.\n");
    exit(1);
}
%%

{% endhighlight %}

接下来是`yacc`文件的编写，后缀名为`.y`,这里命名为`calc.y`,具体代码如下:

{% highlight c linenos %}
%{
#include <stdio.h>
#include <stdlib.h>
#define YYDEBUG 1
%}
%union {
    int          int_value;
    double       double_value;
}
%token <double_value>      DOUBLE_LITERAL
%token ADD SUB MUL DIV CR
%type <double_value> expression term primary_expression
%%
line_list
    : line
    | line_list line
    ;
line
    : expression CR
    {
        printf(">>%lf\n", $1);
    }
expression
    : term
    | expression ADD term
    {
        $$ = $1 + $3;
    }
    | expression SUB term
    {
        $$ = $1 - $3;
    }
    ;
term
    : primary_expression
    | term MUL primary_expression 
    {
        $$ = $1 * $3;
    }
    | term DIV primary_expression
    {
        $$ = $1 / $3;
    }
    ;
primary_expression
    : DOUBLE_LITERAL
    ;                 
%%
int
yyerror(char const *str)
{
    extern char *yytext;
    fprintf(stderr, "parser error near %s\n", yytext);
    return 0;
}

int main(void)
{
    extern int yyparse(void);
    extern FILE *yyin;

    yyin = stdin;
    if (yyparse()) {
        fprintf(stderr, "Error ! Error ! Error !\n");
        exit(1);
    }
}


{% endhighlight %}

##代码解析

**忙(懒)而未写，有待补充**


##参考资料

> * <http://dinosaur.compilertools.net/>