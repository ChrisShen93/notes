### 记一个问题

下午在一个已有的lerna代码仓库中添加一个新的package，然后添加一些依赖：

```shell
lerna add module1 --scope=@xx/yy
```

一直会返回一个异常结果：

```shell
lerna ERR! EFILTER No packages remain after filtering [ '@xx/yy' ]
```

执行

```shell
lerna list -a
```

打印出来的结果是不包含新增的 `@xx/yy` 的



#### 尝试解决

1. 删除所有依赖以及`yarn.lock`文件，并重新`lerna bootstrap`安装依赖。无效
2. 删除该package，并使用`lerna create [name]`命令重新生成。无效

#### 最终解决

经过一番查阅之后将目光重新放在配置文件上 `lerna.json`：

```json
{
  "npmClient": "yarn",
  "useWorkspaces": true,
  "packages": [
    'package1',
    'package2',
    'yyy'
  ]
}
```

这里的`useWorkspaces`就是问题的关键，它实际上和`yarn`的`workspaces`是相同的概念。当`lerna.json`中设为`true`后，`lerna`将不再从`lerna.json`中读取`packages`的配置，而是需要在项目的根路径中的`package.json`中去声明。即：

```json
{
  "name": "...",
  "private": true,
  "packages": [
    'package1',
    'package2',
    'yyy'
  ]
}
```

