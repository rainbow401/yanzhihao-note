# Dokcerfile



## 固定首条命令 From

格式：

```  dockerfile
FROM <image> 
或者 
FROM <image>:<tag>
```

如果找不到该镜像则返回错误

## ENV

为镜像声明环境变量

格式：

```dockerfile
ENV <key> <value> 
或者 
ENV <key>=<value> ...
```

其他指令使用环境变量时，使用格式为：

```dockerfile
$variable_name
或者
${variable_name}
```

如`\$foo`或者`\${foo}`将会被转换为`$foo`和`${foo}`，而不是环境变量所保存的值

