### 在Mac上使用ssh同时管理Gitlab和Github的账户

# 背景

在日常工作中，公司的代码都是保密的，所以常规的手段是内网部署一个私有的`gitlab`服务，然后为我们域账户添加访问权限。同样有的代码是需要开源的，所以我们也会提交代码到`github`上，此时我们就需要去使用两个`ssh-key`来管理不同的仓库。如下我将介绍如何在同一台`mac`电脑上同时使用`ssh`管理`gitlab`和`github`账户。

## 生成sshkey

```powershell
# 在用户目录下创建.ssh目录，如果有，请忽略该步骤
mkdir ~/.ssh

ssh-keygen -t rsa -C "personal@mail.com" -f ~/.ssh/id_rsa_gitlab
ssh-keygen -t rsa -C "personal@mail.com" -f ~/.ssh/id_rsa_github
```

现在会生成如下四个文件

```
~/.ssh/id_rsa_github
~/.ssh/id_rsa_github.pub
~/.ssh/id_rsa_gitlab
~/.ssh/id_rsa_gitlab.pub
```

## 在网页上进行配置

### Github配置公钥

```shell
# 拷贝生成的公钥到剪切板
pbcopy < ~/.ssh/id_rsa_github.pub
```

在`github`登录你的账号，进入如下`Settings > SSH and GPG Keys > New SSH key`位置，粘贴公钥。

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8356e285eeb347fdad747defee411da6~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

### Gitlab配置公钥

```shell
# 拷贝生成的公钥到剪切板
pbcopy < ~/.ssh/id_rsa_gitlab.pub
```

登录`Gitlab`账户，进入`Settings > SSH Keys`,粘贴公钥使用即可。

## 使用ssh-agent管理ssh keys

```shell
ssh-add ~/.ssh/id_rsa_github
ssh-add ~/.ssh/id_rsa_gitlab
```

## 创建ssh配置文件

```shell
# 创建文件
touch ~/.ssh/config
# 编辑文件
vi ~/.ssh/config
```

以如下内容为模板更改配置

```
# Personal github account
Host github.com
   HostName github.com
   User git
   IdentityFile ~/.ssh/id_rsa_github
# Personal gitlab account
Host gitlab.com
   HostName gitlab.com
   User bgit
   IdentityFile ~/.ssh/id_rsa_gitlab
```

配置好后可以使用如下命令去测试

```shell
ssh -T git@github.com
```

如果成功你就可以看到下面的提示啦~

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/28d6480bb59d4a7fb3a4bf750be4aeef~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

