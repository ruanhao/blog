---
title: 使用 Emacs 作为Python IDE
date: 2016-08-24T17:40:15+08:00
comments: true
categories: python
---

## 前言

终端编辑器很适合编写脚本类语言，编辑速度快，Ctrl-Z 就能立马调试运行看到效果。
并不是建议大家一定要使用 Emacs 或者 Vim ，
如果是开发大型工程，或者是团队合作，请根据实际情况选择合适的开发工具，比如 PyCharm 。

不过，如果只是一些随写随用的脚本，何不尝试下 Emacs 呢？



## 插件配置

试了下几种常见 Emacs Python 插件，感觉 [Elpy](https://github.com/jorgenschaefer/elpy) 不论是配置还是使用，
都比较简单和轻量化。

在 ~/.emacs.d/init.el 文件中添加以下配置代码：（需事先安装 use-package）

``` lisp
(use-package elpy
  :defer t
  :init (with-eval-after-load 'python (elpy-enable)))

(add-hook 'elpy-mode-hook
          (lambda ()
            (define-key elpy-mode-map (kbd "M-.")     'elpy-goto-definition)
            (define-key elpy-mode-map (kbd "C-x M-.") 'elpy-goto-definition-other-window)
            (define-key elpy-mode-map (kbd "M-,")     'pop-tag-mark)
            (define-key elpy-mode-map (kbd "M-RET")   'elpy-company-backend)))
```

这样就可以了，使用 `Meta-.` 和 `Meta-,` 查看函数定义和回退，使用 `Meta-Enter` 调出代码提示。


## 效果

![](/images/tech/emacs_py_env.gif)
