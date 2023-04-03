
# 请求地址

```
http://lxmovie.vip/ppgx
```

# 调用方式：POST GET

# 请求实例

```
http://lxmovie.vip/ppgx?url=https://h5.pipigx.com/pp/post/574134352027
```

# 请求参数

|字段名称       |字段说明         |类型            |必填            |备注     |
| -------------|:--------------:|:--------------:|:--------------:| ------:|
|url|需要解析的视频链接|string|Y||

# 返回数据

```json
{
  msg: "操作成功",
  code: 0,
  data: {
    type: "video",
    url: "http://tbvideo.ixiaochuan.cn/zyvqwz/78/c8/cd83-696e-11ec-a438-00163e020689",
    content: "群里看到的，尼玛 笑死我了"
  }
}
```

# 返回结果data参数说明

|字段名称       |字段说明         |类型            |必填            |备注     |
| -------------|:--------------:|:--------------:|:--------------:| ------:|
|type|图文/视频|string|Y||
|url|视频链接|string|N||
|content|标题|string|N||


# 调用效果

移步：[lxmovie.vip](lxmovie.vip) 查看




