# typora 自动添加序号

> 到 typora 文件-偏好设置-外观，打开主题文件夹，新建 文件 `base.user.css`
>
> 大纲同时自动显示序号，这款为**主流设置**

```css
/** initialize css counter */
#write, .sidebar-content,.md-toc-content {
    counter-reset: h1
}

#write h1, .outline-h1, .md-toc-item.md-toc-h1  {
    counter-reset: h2
}

#write h2, .outline-h2, .md-toc-item.md-toc-h2 {
    counter-reset: h3
}

#write h3, .outline-h3, .md-toc-item.md-toc-h3 {
    counter-reset: h4
}

#write h4, .outline-h4, .md-toc-item.md-toc-h4 {
    counter-reset: h5
}

#write h5, .outline-h5, .md-toc-item.md-toc-h5 {
    counter-reset: h6
}

/** put counter result into headings */
#write h1:before, 
h1.md-focus.md-heading:before,
.outline-h1>.outline-item>.outline-label:before,
.md-toc-item.md-toc-h1>.md-toc-inner:before{
    counter-increment: h1;
    content: counter(h1) " "
}

#write h2:before, 
h2.md-focus.md-heading:before,
.outline-h2>.outline-item>.outline-label:before,
.md-toc-item.md-toc-h2>.md-toc-inner:before{
    counter-increment: h2;
    content: counter(h1) "." counter(h2) " "
}

#write h3:before,
h3.md-focus.md-heading:before,
.outline-h3>.outline-item>.outline-label:before,
.md-toc-item.md-toc-h3>.md-toc-inner:before {
    counter-increment: h3;
    content: counter(h1) "." counter(h2) "." counter(h3) " "
}

#write h4:before,
h4.md-focus.md-heading:before,
.outline-h4>.outline-item>.outline-label:before,
.md-toc-item.md-toc-h4>.md-toc-inner:before  {
    counter-increment: h4;
    content: counter(h1) "." counter(h2) "." counter(h3) "." counter(h4) " "
}

#write h5:before,
h5.md-focus.md-heading:before,
.outline-h5>.outline-item>.outline-label:before,
.md-toc-item.md-toc-h5>.md-toc-inner:before  {
    counter-increment: h5;
    content: counter(h1) "." counter(h2) "." counter(h3) "." counter(h4) "." counter(h5) " "
}

#write h6:before,
h6.md-focus.md-heading:before,
.outline-h6>.outline-item>.outline-label:before,
.md-toc-item.md-toc-h6>.md-toc-inner:before {
    counter-increment: h6;
    content: counter(h1) "." counter(h2) "." counter(h3) "." counter(h4) "." counter(h5) "." counter(h6) " "
}

/** override the default style for focused headings */
#write>h3.md-focus:before,
#write>h4.md-focus:before,
#write>h5.md-focus:before,
#write>h6.md-focus:before,
h3.md-focus:before,
h4.md-focus:before,
h5.md-focus:before,
h6.md-focus:before {
    color: inherit;
    border: inherit;
    border-radius: inherit;
    position: inherit;
    left:initial;
    float: none;
    top:initial;
    font-size: inherit;
    padding-left: inherit;
    padding-right: inherit;
    vertical-align: inherit;
    font-weight: inherit;
    line-height: inherit;
}
```



>去除序号后面 . 号的版本

```css
/** initialize css counter */
#write {
counter-reset: h1
}
h1 {
counter-reset: h2
}
h2 {
counter-reset: h3
}
h3 {
counter-reset: h4
}
h4 {
counter-reset: h5
}
h5 {
counter-reset: h6
}
/** put counter result into headings */
#write h1:before {
counter-increment: h1;
content: counter(h1) " "
}#write h2:before {
counter-increment: h2;
content: counter(h1) "." counter(h2) " "
}
#write h3:before,
h3.md-focus.md-heading:before /** override the default style for focused headings */ {
counter-increment: h3;
content: counter(h1) "." counter(h2) "." counter(h3) " "
}
#write h4:before,
h4.md-focus.md-heading:before {
counter-increment: h4;
content: counter(h1) "." counter(h2) "." counter(h3) "." counter(h4) " "
}
#write h5:before,
h5.md-focus.md-heading:before {
counter-increment: h5;
content: counter(h1) "." counter(h2) "." counter(h3) "." counter(h4) "." counter(h5) " "
}
#write h6:before,
h6.md-focus.md-heading:before {
counter-increment: h6;
content: counter(h1) "." counter(h2) "." counter(h3) "." counter(h4) "." counter(h5) "." counter(h6) " "
}
/** override the default style for focused headings */
#write>h3.md-focus:before,
#write>h4.md-focus:before,
#write>h5.md-focus:before,
#write>h6.md-focus:before,
h3.md-focus:before,
h4.md-focus:before,
h5.md-focus:before,
h6.md-focus:before {
color: inherit;
border: inherit;
border-radius: inherit;
position: inherit;
left:initial;
float: none;
top:initial;
font-size: inherit;
padding-left: inherit;
padding-right: inherit;
vertical-align: inherit;
font-weight: inherit;
line-height: inherit;
}
```

