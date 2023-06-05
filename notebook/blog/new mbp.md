### 代理 镜像
For Android Studio，Auto-detect proxy settings 走系统代理

mirrors.neusoft.edu.cn 80

清华Mirror: https://mirror.tuna.tsinghua.edu.cn/
阿里云Mirror: https://developer.aliyun.com/mirror/

### homebrew item2 zsh

`.zshrc` 配置

- homebrew镜像
- ANDROID_HOME、JAVA_HOME

```shell
export ANDROID_HOME="$HOME/Library/Android/sdk"

export HOMEBREW_BOTTLE_DOMAIN="https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles/bottles"
// etc...
```

### vscoe

配置settings.json 同步

### git

配置账号，`.ssh`文件夹可拷过来

```shell
git config --global user.name "eyedeng"
git config --global user.email "dengyan1912@outlook.com"
```

git加速，使用本地代理（clashX: export https_proxy=http://127.0.0.1:7890 http_proxy=http://127.0.0.1:7890 all_proxy=socks5://127.0.0.1:7890）

```shell
git config --global http.https://github.com.proxy socks5h://127.0.0.1:7890
git config --global https.https://github.com.proxy socks5h://127.0.0.1:7890
```

.ssh/config配置ssh代理

```shell
Host github.com
ProxyCommand nc -v -x 127.0.0.1:7890 %h %p
```

如果git push走的是https方式（git remote -v检查），改为ssh方式

```
git remote set-url origin git@github.com:eyedeng/eyedeng.github.io.git
```

