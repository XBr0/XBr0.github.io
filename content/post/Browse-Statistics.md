---
title: "【转载】Hugo PaperMod主题添加不蒜子Busuanzi浏览统计"
date: 2025-09-28T21:57:09+08:00
categories: ["通用技术"]
tags: ["博客搭建"]
---

> **转载声明**
>
> - **原文作者：** [0x4b404ec](https://github.com/0x4b404ec)
> - **原文链接：** [Hugo PaperMod主题添加不蒜子Busuanzi浏览统计](https://0x4b404ec.github.io/posts/hugo-papermod%E4%B8%BB%E9%A2%98%E6%B7%BB%E5%8A%A0%E4%B8%8D%E8%92%9C%E5%AD%90busuanzi%E6%B5%8F%E8%A7%88%E7%BB%9F%E8%AE%A1/)
> - **版权声明：** 本文转载自网络文章，转载目的仅为个人收藏与知识分享。若存在任何侵权问题，请随时与我联系，我会立即处理。如果您觉得这篇文章及相关项目对您有所帮助，不妨前往项目地址为原作者点个 star ，以表支持与鼓励 。

> **在安装原文设置后出现了些问题，本文对原文对footer.html和single.html进行了修改**

# 不蒜子Busuanzi浏览统计

> [Busuanzi](https://busuanzi.ibruce.info/)

我是使用了Git的子模块进行主题的管理，所以为了保持子模块的完整性，所有修改都在Hugo框架中进行。

## head.html

复制

`head.html`文件位于`[Hugo_blog]/themes/PaperMod/layouts/partials/head.html` 

至

`[Hugo_blog]/layouts/partials/head.html`

**(如没有文件夹需要自己创建)**

搜索`{{- /* Styles */}}` 在其上一行添加代码：

```html
{{- if .Site.Params.busuanzi.enable -}}
  <script async src="//busuanzi.ibruce.info/busuanzi/2.3/busuanzi.pure.mini.js"></script>
  <meta name="referrer" content="no-referrer-when-downgrade">
{{- end -}}
```



## footer.html

复制

`footer.html`文件位于`[Hugo_blog]/themes/PaperMod/layouts/partials/footer.html` 

至

`[Hugo_blog]/layouts/partials/footer.html`

搜索`{{- if not (.Param "hideFooter") }}`,并将其的代码进行替换

```html
{{- if not (.Param "hideFooter") }}
<footer class="footer">
    <div>
        {{- if not site.Params.footer.hideCopyright }}
            {{- if site.Copyright }}
            <span>{{ site.Copyright | markdownify }}</span>
            {{- else }}
            <span>&copy; {{ now.Year }} <a href="{{ "" | absLangURL }}">{{ site.Title }}</a></span>
            {{- end }}
            {{- print " · "}}
        {{- end }}
        
        {{- with site.Params.footer.text }}
            {{ . | markdownify }}
            {{- print " · "}}
        {{- end }}
        <span>
            Powered by
            <a href="https://gohugo.io/" rel="noopener noreferrer" target="_blank">Hugo</a> &
            <a href="https://github.com/adityatelange/hugo-PaperMod/" rel="noopener" target="_blank">PaperMod</a>
        </span>
    </div>
    {{- if .Site.Params.busuanzi.enable -}}
    <div>
        <span class="busuanzi-footer">
            <span id="busuanzi_container_site_pv">
                · 本站总访问量 <span id="busuanzi_value_site_pv"></span> 次
            </span>
            <span id="busuanzi_container_site_uv">
                · 本站访客数 <span id="busuanzi_value_site_uv"></span> 人次
            </span>
        </span>
    </div>
    {{- end }}
</footer>
{{- end }}
```



## single.html

复制

`single.html`文件位于`[Hugo_blog]/themes/PaperMod/layouts/_default/single.html` 

至

`[Hugo_blog]/layouts/_default/single.html`

**(如没有文件夹需要自己创建)**

搜索`{{- partial "post_canonical.html" . -}}`在其下一行添加代码：

```html
{{- if .Site.Params.busuanzi.enable -}}
    <span class="meta-item">&nbsp;·&nbsp;
        <span id="busuanzi_container_page_pv">本文阅读量<span id="busuanzi_value_page_pv"></span>次</span>
    </span>
{{- end }}
```



## Config.yaml

启动Busuanzi统计功能，返回本目录在配置文件params中添加参数

```yaml
params:  
    busuanzi:
        enable: true # 是否启动Busuanzi统计
```