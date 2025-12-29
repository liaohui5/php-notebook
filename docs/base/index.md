## 介绍

PHP 是动态类型的编程语言, 专注与 web 开发, 其语法简单易懂, 性能也尚可(与Java比),
适合做一些小型项目或者个人博客什么的, 开发速度和体验都还不错, 总之是一个值得学习了解的脚本语言

> 用大白话说吧:

1. PHP 是 web 开发界的老前辈
2. 历史包袱重, 环境配置复杂
3. 但由于新版本 swoole/swow 扩展对协程的支持, 让 PHP 请求吞吐量要略微高于 node.js
4. 社区不如 node.js, 无奈有些老项目就是用的 PHP, 重构成本太大, 就一直用 PHP
5. PHP 语法和 node.js 一样, 都是类C(`C-Like`)的编程语言

总而言之: 虽然有各种各样的缺点, 但是语法简洁易懂, 容易上手, 是个值得学习的编程语言

## 线程模型

默认情况下, `PHP 是单线程的`, 且不支持多线程/多进程/多协程 进行异步操作, 需要手动去安装扩展模块:

- [pthreads](https://pecl.php.net/package/pthreads) 让 PHP 可以使用多线程(PHP7.4 以前的版本)
- [parallel](https://pecl.php.net/package/parallel) 让 PHP 可以使用多线程(PHP7.4 以及高于 7.4 的版本)
- `pcntl` 让 PHP 可以使用多进程(内置扩展, 但仅限 CLI 环境, Web 环境不可用)
- `fiber/swoole/swow` 让PHP支持协程(这些都是 C/C++ 扩展, 学习成本太高, 有这个时间 Go/Rust 语言都学会了)
  - [fiber](https://www.php.net/manual/zh/language.fibers.php) 透明非阻塞 I/O 标准库实现, 这完成度太低了, 很难用, 至少也得像 JS 的 `Promise` API 一样的完成度才方便在业务代码中使用
  - [swow](https://github.com/swow/swow) C 编写的库, 对标 swoole, 但是还比较新
  - [swoole](https://www.swoole.com/) 久经考验的社区扩展协程实现, 直接让 PHP 性能超越 node.js, 但是上手难度比较高

## 内置函数 & 扩展模块

PHP 的解释器主要是用 C 编写的, 所以很多的扩展模块也是需要单独的安装并且在 `php.ini` 中配置
才会有的, 并不是和 python 那样, 只要安装就是内置的, 不需要配置

- 搜索/下载需要的扩展: https://pecl.php.net/
- 官方维护的扩展管理器: https://github.com/php/pie

### php.ini

用于控制 php 解释器行为的配置文件

### 如何查看 php 解释器的相关信息

```php
<?php
  phpinfo(); // 查看 php 解释器相关信息, 比如: 版本/加载了哪些模块等
?>
```

## 启动运行

- 开发模式: 使用[内置的服务器](https://www.php.net/manual/zh/features.commandline.webserver.php)解析运行 php 脚本
- 线上模式: 使用 apache/nginx 解析运行 php 脚本, 需要配置 fpm

```sh
# 使用 php 内置服务器运行
# 0. 这是一个辅助开发用的内置 web-sever
# 1. 不支持多线程
# 2. 功能并不完备(相比: nginx/apache/canddy-server)
php -S localhost:3000
```

## 专注于 Web 开发

PHP 被设计出来的初衷就是做Web开发的, PHP 的全称 `Hypertext Preprocessor`, 是一个递归缩写,
翻译为中文就是: `超文本预处理器`

- 高情商: 专注于 web 开发
- 低情商: 只能做 web 开发 🤡

简单易学, 语法便捷: `君以此兴, 必以此亡` PHP 能做的, Node.js 和 python 都能做,
而且这些语言还能用作其他领域, 再者: 太过便捷的语法, 只会导致更多的耦合更可怕的维护地狱,
因为开发人员的参差不齐, 太灵活的语法只能造就一堆可怕的难以阅读的垃圾

## 语言特色

- 高情商: 语言特色
- 低情商: 历史包袱太重, 根本改不动

不得不说的一大特色就是, 它可以和 HTML 混写, 虽然现在的框架都极力阻止这么做, 但我还是见过一些骨灰级项目是这样做的

```php
<?php
  $title = 'php-demo';
?>
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title><?= $title ?></title>
  </head>
  <body>
    <?php
      $content = "hello world"
    ?>
    <h2><?php echo $content; ?></h2>
  </body>
</html>
```

🤬 不得不说, 这特性真的令人不适, 及其恶心, 这语法就是 php 沉重的历史包袱之一,
为了便捷性, 强行将逻辑和视图全部耦合到一起, 要我说就应该在一个主要版本发布的时候,
开一个 break change, 给这个恶心的玩意去掉, 哪怕内置一个模板引擎呢... 🤡

- `<?= $title ?>` WTF 这是什么?
- `<?php echo $title;?>` 为什么又能这样写?

对于上面的代码, 总而言之: 在html中 `<?php ?>` 或 `<?= ?>` 这样的标签就会被 php
的解释器解释运行, 其他会被当作字符串正常输出到缓冲区然后响应给客户端 `<?= $title>`
是 `<?php echo $title; >` 的语法糖, 其功能是一样的
