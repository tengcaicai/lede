大神coolsnowwolf/lede的源代码中选取包含config-4.14编译v4.14.180内核的最后一个版本R20.7.2，修改为v4.14内核编译给hc5962使用（极路由4或B70），使用旧内核的原因是极路由的硬件转发在v5.4内核时居然只有400MB不到，不能满足500MB宽带的要求，如果用v4.14内核估计能到600MB以上，在lean的R21.6.1版本有4.14.195的内核patch，但是没有config-4.14，这里的版本就是把R20.7.2的内核版本升级为4.14.195了。

lean的原来版本的链接地址：https://github.com/coolsnowwolf/lede
-
注意：
-
1. 不要用 **root** 用户 git 和编译！！！
2. 国内用户编译前最好准备好梯子
3. 默认登陆IP 192.168.1.1, 密码 password

编译命令如下:
-
1. 首先装好 Ubuntu 64bit，推荐  Ubuntu 20.04 LTS x64

2. 命令行输入 `sudo apt-get update` ，然后输入
`
sudo apt-get -y install build-essential asciidoc binutils bzip2 gawk gettext git libncurses5-dev libz-dev patch python3.5 python2.7 unzip zlib1g-dev lib32gcc1 libc6-dev-i386 subversion flex uglifyjs git-core gcc-multilib p7zip p7zip-full msmtp libssl-dev texinfo libglib2.0-dev xmlto qemu-utils upx libelf-dev autoconf automake libtool autopoint device-tree-compiler g++-multilib antlr3 gperf wget
`

3. 使用 `git clone https://gitee.com/tengcai/lede` 命令下载好源代码，然后 `cd lede` 进入目录

4. ```bash
   ./scripts/feeds update -a
   ./scripts/feeds install -a
   make menuconfig
   ```

5. `make -j8 download V=s` 下载dl库（国内请尽量全局科学上网）


6. 输入 `make -j1 V=s` （-j1 后面是线程数。第一次编译推荐用单线程）即可开始编译你要的固件了。


二次编译：
```bash
cd lede
git pull
./scripts/feeds update -a && ./scripts/feeds install -a
make defconfig
make -j8 download
make -j$(($(nproc) + 1)) V=s
```

如果需要重新配置：
```bash
rm -rf ./tmp && rm -rf .config
make menuconfig
make -j$(($(nproc) + 1)) V=s
```

编译完成后输出路径：/lede/bin/targets
