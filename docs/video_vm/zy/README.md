# 网站已到期，目前无法使用
# 请求地址

```
http://lxmovie.vip/zy
```

# 调用方式：POST GET

# 请求实例

```
http://lxmovie.vip/zy?url=https://share.xiaochuankeji.cn/hybrid/share/post?pid=250588154&zy_to=applink&share_count=1&m=2971da49d3b22f99c4e171f8f82715cd&d=3ef201eef6020a5c4948d22736b7e944b8b64a5d5103b54e781ea0c18e9edc8b2ccf064ae1f3930fb02086e5b31295e6&app=zuiyou&recommend=r0&name=n0&title_type=t0
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
    url: "http://video.izuiyou.com/zyvqwz/5a/79/fc30-322b-11ec-a99f-00163e0e67b8",
    content: "社 交 牛 A 症"
  }
}
```

# 返回结果data参数说明

|字段名称       |字段说明         |类型            |必填            |备注     |
| -------------|:--------------:|:--------------:|:--------------:| ------:|
|cover|封面链接|string|N||
|music|BGM|json|N||
|author|作者|string|N||
|type|图文/视频|string|Y||
|url|视频链接|string|N||
|content|标题|string|N||


# 调用效果

移步：[lxmovie.vip](lxmovie.vip) 查看




