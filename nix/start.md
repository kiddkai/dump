nixos
=====

> 为什么要用 nix，软件包升级无忧。

主要亮点功能
------------

### 系统配置简单

系统配置在 /etc/nixos/configuration.nix 里面，更新了以后运行

```
$ nixos-rebuild switch
```

进行更新

### 可信赖的升级机制


### Atomic 升级


### 回滚机制


### 可重用的系统配置


### 可被测试的更新


### binaries 结合Source-based 模式

### 一致性

### 多用户软件包管理


nixos是怎么运作的？
--------------------

NixOS 基于一个叫做 nix 的软件包管理工具，一个“纯函数式包管理工具”。所有的nix包都是各自独立的例如一个软件包会放在
下面这个目录里：

```
/nix/store/5rnfzla9kcx4mj5zdc7nlnv8na1najvg-firefox-3.5.4/
```

`5rnfzla9kcx4mj5zdc7nlnv8na1najvg` 代表这个软件包构建以后的hash（感觉和前端hash js差不多）。软件包在构建以后永远
不会被覆盖。所以无论你重新构建多少次软件包，它都会放在 `/nix/store` 里的单独文件夹，老的软件包还是会在原来的目录
里。

