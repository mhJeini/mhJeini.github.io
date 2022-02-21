
# 请求地址

```
http://lxmovie.vip/zy
```

# 调用方式：POST GET

# 请求实例

```
http://lxmovie.vip/zy?url=https://share.xiaochuankeji.cn/hybrid/share/post?pid=266827216&zy_to=applink&share_count=1&m=caf0f0bd79c8334b00d12f1ee04cfd4d&d=3ef201eef6020a5c4948d22736b7e944b8b64a5d5103b54e781ea0c18e9edc8b2ccf064ae1f3930fb02086e5b31295e6&app=zuiyou&recommend=r0&name=n0&title_type=t0
```

# 请求参数

|字段名称       |字段说明         |类型            |必填            |备注     |
| -------------|:--------------:|:--------------:|:--------------:| ------:|
|url|需要解析的视频链接|string|Y||

# 返回数据

```json
{
    "msg":"操作成功",
    "code":0,
    "data":{
        "cover":"http://tbfile.izuiyou.com/img/frame/id/1817974536?w=540&xcdelogo=0",
        "type":"video",
        "url":"http://video.izuiyou.com/zyvqwz/264/6d/fb/3978-8a13-11ec-9894-00163e0e67b8",
        "content":"深得真传"
    }
}
```

# 返回结果data参数说明

|字段名称       |字段说明         |类型            |必填            |备注     |
| -------------|:--------------:|:--------------:|:--------------:| ------:|
|cover|封面链接|string|N||
|type|图文/视频|string|Y||
|url|视频链接|string|N||
|content|标题|string|N||


# 调用效果

移步：[lxmovie.vip](lxmovie.vip) 查看




