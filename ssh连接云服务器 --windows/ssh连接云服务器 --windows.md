## 1. 下载 power shell 

[GitHub的下载地址](https://github.com/PowerShell/PowerShell)，使用`ctrl` + `F`搜索`Get powershell`,你会看到各个操作系统的下载地址，选择对应的系统，进行下载。这里简单说一下，几个版本的区别。

`LTS`LTS 是长期支持版本的缩写，通常指的是软件发布的长期支持版本。这些版本通常会提供更长的支持期限，一般都是一年甚至更久，也就是说长时间的不更新也不会影响程序的运行，他的稳定性是最好的，但功能的更新上会没有`stable`那么快。通俗点讲，企业版。

`stable`stable 版本通常是软件的正式发布版本，它们提供了稳定的使用体验和较高的质量。这些版本通常是为普通用户准备的，它们提供了完整的功能和较少的 bug    正式版。

`preview`版本通常是软件的预览版本，它们提供了未来正式发布版本的功能预览。这些版本通常是为开发人员和极具冒险精神的用户准备的，它们可能存在 bug 和不稳定的使用体验  开发版。

## 2. 管理员模式运行power shell

查看是否已经安装了open ssh，在命令行中输入`Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*'`。

open ssh有两个应用需要安装，分别是client和server。`NotPresent`表示没有安装，如果显示`installed`表示已安装。

```
Name  : OpenSSH.Client~~~~0.0.1.0
State : NotPresent

Name  : OpenSSH.Server~~~~0.0.1.0
State : NotPresent
```



## 3. 安装服务器

输入`Add-WindowsCapability -Online -Name OpenSSH.Client`.如果两者都安装成功。则都会输出

```
Path          :
Online        : True
RestartNeeded : False
```

## 4. 安装客户端组件

`Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0`



## 5. 启动并配置 OpenSSH 服务器

```
#启动ssh服务
Start-Service sshd

# 更改名字为sshd，并把服务更改为自动启动。
Set-Service -Name sshd -StartupType 'Automatic'

# 自动创建防火墙规则设置端口号为22，使得可以通过 SSH 协议连接云端服务器。
if (!(Get-NetFirewallRule -Name "OpenSSH-Server-In-TCP" -ErrorAction SilentlyContinue | Select-Object Name, Enabled)) {
    Write-Output "Firewall Rule 'OpenSSH-Server-In-TCP' does not exist, creating it..."
    New-NetFirewallRule -Name 'OpenSSH-Server-In-TCP' -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
} else {
    Write-Output "Firewall rule 'OpenSSH-Server-In-TCP' has been created and exists."
}
```



## 连接到 OpenSSH 服务器

重新打开一个power shell，记住要以管理员模式。

```
ssh username@servername
```

第一个参数是云服务器的名字，如果没有更改过默认设置，一般都是服务器的系统名字。比如我的系统是Linux系统的发行版本`ubuntu`，在购买的云服务器管理实例里面一般都会找到用户名。参数2为服务器对应的**公网**`IP`记住是公网，而不是内网。连接后，会收到如下所示的消息，选择是否进行连接服务器。

```
The authenticity of host 'servername (10.00.00.001)' can't be established.
ECDSA key fingerprint is SHA256:(<a large string>).
Are you sure you want to continue connecting (yes/no)?
```

输入完毕后，系统会提示你输入密码，作为安全预防措施，密码在键入的过程中不会显示。

