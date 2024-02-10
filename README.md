# Search libc function offset



## 来源

这是[原仓库](https://github.com/lieanu/LibcSearcher)的一个 fork；LibcSearcher 是一个好用的工具，可惜原仓库已经数年没有更新了，而原作者也已数年在 GitHub 没有任何活动，似乎已经放弃维护了。

## 简介

这是针对 CTF 比赛所做的小工具。

在泄露了目标系统 libc 中的某一个函数地址后，往往需要通过手动对比来判断目标系统使用的 libc 版本，并进一步计算出其它函数的地址；该工具实现了这一麻烦过程的脚本化。

推荐 [libc-database](https://github.com/niklasb/libc-database) 的数据库。

## 安装

```bash
git clone https://github.com/runshell/LibcSearcher.git
cd LibcSearcher
python setup.py develop
```

在此之后，请修改 LibcSearcher 目录下的 `config.py`，将变量 `libcs_path` 改为你存放各版本 libc 的目录；如果你使用的是 [libc-database](https://github.com/niklasb/libc-database)，应当改为 libc-database 目录下子目录 db 的路径。**注意，路径请以`/`结尾。**

## 示例

```python
from LibcSearcher import *

# 第 2 个参数为已泄露的实际地址或最后 12 位(比如 0xd90)
libc = LibcSearcher("fgets", 0X7ff39014bd90)

libc.dump("system")        # 函数 system 的偏移
libc.dump("str_bin_sh")    # 字符串 /bin/sh 的偏移
libc.dump("__libc_start_main_ret")
```

如果遇到返回多个libc版本库的情况，可以通过 `add_condition(leaked_func, leaked_address)` 来添加限制条件，也可以手动选择其中一个libc版本（如果你确定的话）。

