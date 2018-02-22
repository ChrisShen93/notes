# 多用户 ssh-key

---

* 生成秘钥对

```shell
ssh-keygen -t rsa -c "[email_1]" -f ~/.ssh/xxx_rsa

ssh-keygen -t rsa -c "[email_2]" -f ~/.ssh/yyy_rsa
```

* 添加config文件

```shell
# ssh-key 1
Host [host]    # 别名
HostName [host_name]    # 主机名
Port [port]    # 端口号
User [user_name]    # 用户名
IdentityFile [rsa_file_path]    # 秘钥文件路径

# ssh-key 2
Host [host]
HostName [host_name]
User [user_name]
IdentityFile [rsa_file_path]
```

* 将秘钥添加进agent中

```shell
# *nix, mac
ssh-add ~/.ssh/[file_name]

# windows
# 必须在git-bash或Linux子系统中进行
exec ssh-agent bash
eval `ssh-agent -s`
ssh-add ~/.ssh/[file_name]
```

* 将公钥添加到远程服务器中

* Done

[ssh_config 文档](https://linux.die.net/man/5/ssh_config)

对于MacOS，可以添加两个配置项

```shell
UseKeychain yes
AddKeysToAgent yes
```