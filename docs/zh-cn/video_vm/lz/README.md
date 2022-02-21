
# 请求地址

```
http://lxmovie.vip/lz
```

# 调用方式：POST GET

# 请求实例

```
http://lxmovie.vip/lz?url=https://m.oasis.weibo.cn/v1/h5/share?sid=4737451002167563
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
    "author":"舌甘木叔",
    "type":"images",
    "url":"http://f.video.weibocdn.com/o0/1SdJYcctlx07R06TJq2s01041200mRbX0E010.mp4?label=vertical_video_h5&template=720x1280.24.0&trans_finger=2cf6914e1e7bc17a87c3ed927bea12bd&ori=2&Expires=1645451651&ssig=4pBRa6%2BWui&KID=unistore,video",
    "content":"你万圣节扮什么啊？我扮恶心的肌肉女"
  }
}
```

# 返回结果data参数说明

|字段名称       |字段说明         |类型            |必填            |备注     |
| -------------|:--------------:|:--------------:|:--------------:| ------:|
|cover|封面链接|string|N||
|author|作者|string|N||
|type|图文/视频|string|Y||
|url|视频链接|string|N||


# 调用效果

移步：[lxmovie.vip](lxmovie.vip) 查看




