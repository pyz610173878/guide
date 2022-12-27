## 1. 安装git

```
sudo pacman -S git #安装Git 
sudo是启动Linux管理员模式的命令。

git --V            #检查git版本，验证是否安装成功。
```



## 2. 在本地设置ssh密钥，并在GitHub个人账户中进行存储。

### 1. 生成密钥

```
ssh-keygen #生成密钥

#接下来会问你，存放密钥的位置。
Enter file in which to save the key (/root/.ssh/id_rsa):

如果直接按`enter`会存放在默认位置(/user/.ssh/id_rsa)
```

### 2. 建立连接

````
#把生成的密钥添加到列表中
ssh-add <keyfile> (密钥路径地址)(默认是/.ssh/id_rsa)
````

输入`ssh-add -l`指令查看如何添加成功.顺便提一下，如果你想删除已经添加成功的密钥，可以输入以下代码。`ssh-add -d <keyfile>` 删除所有密钥为`ssh-add -D`



### 3. 添加到仓库中

我们假设现在已经添加成功了。我们可以关闭命令行重新打开一个新窗口，这时候你所处的位置(如果没有改bash的指定位置)就处在根目录.

```
cd ~/.ssh  #打开ssh文件夹
```

我们使用`ls`命令查看当前目录下的文件夹。如果发现有`authorized_keys`  `id_rsa`  `id_rsa.pub`  `known_hosts`这4个文件。打开`id_rsa.pub`. 

```
vim id_rsa.pub  #用Linux内置文本编辑器打开
```

vim是Linux自带的文本编辑器。 他的所有的操作是不需要鼠标进行的，这是vim最大的区别。大家如果有兴趣可以自己去了解下。

[这是官方文档](https://missing.csail.mit.edu/2020/editors/)打开以后，我们复制那段ssh密钥。然后打开GitHub仓库找到`settings`,找到`SSH AND gpskeys`。点击`new SSH key`
把那串密钥复制进去，(记住不要添加任何多余的字符)点击保存。



## 3. 验证

我们现在可以验证一下，看是否成功。随便在一个目录下新建一个文件。使用 `git init`初始化仓库。我们随便打开一个云端仓库。

找到`Code`并打开。点击`SSH`复制那串地址。回到本地，使用`git clone` + 仓库地址。

<img src="Linux%E7%B3%BB%E7%BB%9F%E5%A6%82%E4%BD%95%E9%80%9A%E8%BF%87ssh%E5%AF%86%E9%92%A5%E5%85%B3%E8%81%94GitHub%E4%B8%AA%E4%BA%BA%E4%BB%93%E5%BA%93.assets/image-20221227225308471-16721527893213.png" alt="image-20221227225308471" style="zoom:50%;" />