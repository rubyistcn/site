---
title: "本地运行 Ruby 网络服务器"
categories:
  - blog
tags:
  - 服务器
  - WEBrick
---

有些时候，需要临时在本地或者本机上运行网络服务器，进行某些服务调试等。在 Ruby 中，
就可以通过运行

```shell
ruby -run -e httpd . -p 4567
```

在当前目录内，以端口号 `4567` 跑服务器，访问 `http://localhost:4567` 可以查看当前
目录内的文件内容。

那么上面看似简单平常的调用包含那些玄机呢？我们一一破解：

1. 最开始的 `ruby` 毋庸置疑，是我们调用 Ruby 解释器，召唤 Ruby 的口令；
2. `-run` 就不简单了。首先 `-r` 为 Ruby 调用执行程序文件的参数，后面跟着的是要调用
的文件名，也就是说 `un` 是 `un.rb` 的简写形式，完整形式应该是 `-r un`，这里放
到一起了；
3. `-e` 用来执行后面跟随的代码或者方法，这里面就是后面的 `httpd` 方法；
4. 再往后的就是方法 `httpd` 的参数了：
   *. `.` 指的是服务器的根目录，这里使用当前目录为根目录
   *. `-p` 指定参数，这里使用的是 `4567`
  
以上是该行命令的所有部分的意义。那么这个神秘的 `un`——文件 `un.rb` 的庐山真
面目是什么样子呢？代码如下：

```ruby
#
# = un.rb
#
# Copyright (c) 2003 WATANABE Hirofumi <eban@ruby-lang.org>
#
# This program is free software.
# You can distribute/modify this program under the same terms of Ruby.

# [...]

##
# Run WEBrick HTTP server.
#
# ruby -run -e httpd -- [OPTION] DocumentRoot
#
# --bind-address=ADDR address to bind
# --port=NUM listening port number
# --max-clients=MAX max number of simultaneous clients
# --temp-dir=DIR temporary directory
# --do-not-reverse-lookup disable reverse lookup
# --request-timeout=SECOND request timeout in seconds
# --http-version=VERSION HTTP version
# -v verbose
#

def httpd
  setup("", "BindAddress=ADDR", "Port=PORT", "MaxClients=NUM", "TempDir=DIR",
        "DoNotReverseLookup", "RequestTimeout=SECOND", "HTTPVersion=VERSION") do
    |argv, options|
    require 'webrick'
    opt = options[:RequestTimeout] and options[:RequestTimeout] = opt.to_i
    [:Port, :MaxClients].each do |name|
      opt = options[name] and (options[name] = Integer(opt)) rescue nil
    end
    unless argv.size == 1
      raise ArgumentError, "DocumentRoot is mandatory"
    end
    options[:DocumentRoot] = argv.shift
    s = WEBrick::HTTPServer.new(options)
    shut = proc {s.shutdown}
    siglist = %w"TERM QUIT"
    siglist.concat(%w"HUP INT") if STDIN.tty?
    siglist &= Signal.list.keys
    siglist.each do |sig|
      Signal.trap(sig, shut)
    end
    s.start
  end
end
```

做为 Ruby 的御用库，其位置位于 Ruby 安装的 `lib` 目录下，属于 Ruby 默认加载目录，
所以可以直接调用 `un.rb`。在该文件的源代码内，可以看到一系列的参数，我们只使用了
其中的两个。

所以，在 Ruby 中，有很多很奇特的用法，用起来感觉很顺手，很自然，但是其背后有
复杂的工程系统。
