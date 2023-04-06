# 网站已到期，目前无法使用
# 请求地址

```
http://lxmovie.vip/ks
```

# 调用方式：POST GET

# 请求实例

```
http://lxmovie.vip/ks?url=https://v.kuaishou.com/gKoTnR
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
    cover: "https://p2.a.yximgs.com/upic/2022/01/13/16/BMjAyMjAxMTMxNjUwMTlfODQ2MDM2ODczXzY0NzkzNDUzMTY5XzFfMw==_B824260dc4bb617ede919605ee964e846.jpg?clientCacheKey=3x93rvg7t4aggfc.jpg&di=7ccf0917&bp=13380",
    author: "小蚊崽🦟",
    type: "video",
    url: "https://alimov2.a.kwimgs.com/upic/2022/01/13/16/BMjAyMjAxMTMxNjUwMTlfODQ2MDM2ODczXzY0NzkzNDUzMTY5XzFfMw==_b_Bd11e2b26b70899845410a66fbef068a9.mp4?clientCacheKey=3x93rvg7t4aggfc_b.mp4&tt=b&di=7ccf0917&bp=13380",
    content: "知道你不爱看，艾特你儿子来看"
  }
}
```


# 返回结果data参数说明

|字段名称       |字段说明         |类型            |必填            |备注     |
| -------------|:--------------:|:--------------:|:--------------:| ------:|
|type|图文/视频|string|Y||
|url|返回结果|string|N||
|music|返回结果|string|N||
|content|标题|string|N||
|author|作者|string|N||
|cover|封面|string|N||

# 返回结果music参数说明

|字段名称       |字段说明         |类型            |必填            |备注     |
| -------------|:--------------:|:--------------:|:--------------:| ------:|
|url|链接|string|Y||
|author|作者|string|N||


# 调用效果

移步：[lxmovie.vip](lxmovie.vip) 查看




