# Windows SSH 资产要求

!!! info "Windows 资产测试连接, 获取硬件, 自动推送需要进行相关设置"
    注意: 按照下面的文档部署好 openssh 后, 在 web 的资产列表里面找到你的 windows 资产, 在协议组里面加入 rdp 3389, 再添加一个 ssh 22, 然后就可以使用 测试连接, 获取硬件, 自动推送 功能了

!!! tip "Win7/Win2008 需要升级 `powershell` 到 3.0 以上, 详情请参考 [ansible 客户端需求](https://docs.ansible.com/ansible/latest/user_guide/windows_setup.html)"

!!! tip ""
    [下载最新的 OpenSSH](https://github.com/PowerShell/Win32-OpenSSH/releases/latest)  
    解压后，重命名到 C:\Program Files\OpenSSH  
    通过管理员身份的方式打开 powershell , 并在 powershell 里面执行下面命令

### 1. 安装 OpenSSH

!!! tip ""
    ```powershell
    cd "C:\Program Files\OpenSSH"
    powershell.exe -ExecutionPolicy Bypass -File install-sshd.ps1
    ```

### 2. 设置防火墙

!!! tip ""
    ```powershell
    New-NetFirewallRule -Name sshd -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
    ```

    !!! question "如果 win7/win2008 执行上面的命令报错请执行此处的命令"
        ```powershell
        netsh advfirewall firewall add rule name=sshd dir=in action=allow protocol=TCP localport=22
        ```

### 3. 启动 OpenSSH

!!! tip ""
    ```powershell
    net start sshd
    ```

### 4. 设置 OpenSSH 自启

!!! tip ""
    ```powershell
    Set-Service sshd -StartupType Automatic
    ```

!!! info "ssh 登录的账户密码与登录 windows 系统的账户密码一致"
