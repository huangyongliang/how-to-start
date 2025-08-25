# how to start the git


1️⃣ 检查是否已有 SSH 密钥

打开 PowerShell 或 Git Bash，输入：

ls ~/.ssh


如果看到 id_rsa / id_ed25519 和对应的 .pub 文件，就说明你已有密钥，可以直接用（跳到 第 3 步）。

2️⃣ 生成新的 SSH 密钥

推荐使用 Ed25519 算法（比 RSA 更安全且短）。

ssh-keygen -t ed25519 -C "你的 GitHub 邮箱"


如果提示 unknown key type ed25519，改用：

ssh-keygen -t rsa -b 4096 -C "你的 GitHub 邮箱"


一路回车，密钥会生成在：

C:\Users\<你的用户名>\.ssh\

3️⃣ 启动并添加到 ssh-agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519


（如果用 RSA，就替换成 id_rsa）

在 Windows PowerShell 下，可能需要：

Start-Service ssh-agent
ssh-add $env:USERPROFILE\.ssh\id_ed25519

4️⃣ 复制公钥内容
clip < ~/.ssh/id_ed25519.pub


这样公钥就复制到了剪贴板。

5️⃣ 在 GitHub 添加 SSH Key

登录 GitHub

右上角 → Settings → SSH and GPG keys

New SSH key → Title 随便填

Key 类型选择 Authentication（默认）

把刚才复制的公钥粘贴进去 → Add SSH key

6️⃣ 测试连接
ssh -T git@github.com


如果第一次连接会提示：

The authenticity of host 'github.com' can't be established...


输入 yes 回车。
成功会看到：

Hi <你的 GitHub 用户名>! You've successfully authenticated...



1️⃣ 检查 ssh-agent 服务状态

在 PowerShell（管理员模式）中运行：

Get-Service ssh-agent


如果显示 Stopped 或 Disabled，就需要手动启用。

2️⃣ 启用并启动 ssh-agent
Set-Service -Name ssh-agent -StartupType Automatic
Start-Service ssh-agent


这会让 ssh-agent 开机自动启动，并立即运行。

3️⃣ 添加 SSH 私钥

启用服务后，添加你的密钥：

ssh-add $env:USERPROFILE\.ssh\id_ed25519


（如果用 RSA，就改成 id_rsa）

4️⃣ 验证是否添加成功
ssh-add -l


应该能看到类似：

256 SHA256:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx user@example.com (ED25519)


git config --global user.name "你的名字"
git config --global user.email "你的邮箱地址"


git config --global --list
