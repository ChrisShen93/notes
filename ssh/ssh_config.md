# ���û� ssh-key

��ǩ���ո�ָ����� ssh

---

* ������Կ�ԣ�
```shell
ssh-keygen -t rsa -c "[email_1]" -f ~/.ssh/xxx_rsa

ssh-keygen -t rsa -c "[email_2]" -f ~/.ssh/yyy_rsa
```

* ���config�ļ�
```
# ssh-key 1
Host [host]    # ����
HostName [host_name]    # ������
Port [port]    # �˿ں�
User [user_name]    # �û���
IdentityFile [rsa_file_path]    # ��Կ�ļ�·��

# ssh-key 2
Host [host]
HostName [host_name]
User [user_name]
IdentityFile [rsa_file_path]
```

* ����Կ��ӽ�ssh agent��
```shell
# *nix, mac
ssh-add ~/.ssh/[file_name]

# windows
# ������git-bash��Linux��ϵͳ�н���
exec ssh-agent bash
eval `ssh-agent -s`
ssh-add ~/.ssh/[file_name]
```

* ����Կ��ӵ�Զ�̷�������

* Done

[ssh_config �ĵ�](https://linux.die.net/man/5/ssh_config)

����MacOS������������������

```
UseKeychain yes
AddKeysToAgent yes
```





