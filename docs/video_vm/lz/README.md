
# 请求地址

```
http://lxmovie.vip/lz
```

# 调用方式：POST GET

# 请求实例

```
http://lxmovie.vip/lz?url=https://m.oasis.weibo.cn/v1/h5/share?sid=4738603966009645
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
    author: "郭亢Kay",
    type: "video",
    url: "http://oasis.video.weibocdn.com/u2/xz7GWQikgx07TSFiGs6c010412003sMA0E012.mp4?label=mp4_720p&template=796x596.24.0&trans_finger=0dec003e4dad885964301ff5a1db7715&ori=2&Expires=1645692304&ssig=MMKFO4og%2B2&KID=unistore,video",
    content: ""
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




