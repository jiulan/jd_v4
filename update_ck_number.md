# V4解除限制12个ck

step:1

- 修改定时设定 jup替换为
```
jup scripts
```


step:2
- 进入v4容器
```
wget -q https://ghproxy.com/https://raw.githubusercontent.com/jiulan/jd_v4/main/update_ck_number.sh
```

step:3
- 修改是否在运行jup后再额外运行你自己写的 shell 脚本
- 变量 EnableJupDiyShell="true"

step:4
- 自定义脚本添加执行修改脚本

```
sh update_ck_number.sh
```

  
