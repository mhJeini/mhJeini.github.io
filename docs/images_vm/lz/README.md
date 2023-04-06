# 网站已到期，目前无法使用
# 请求地址

```
http://lxmovie.vip/lz
```

# 调用方式：POST GET

# 请求实例

```
http://lxmovie.vip/lz?url=https://m.oasis.weibo.cn/v1/h5/share?sid=4736401122265639
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
    author: "你家green大姐姐原创",
    type: "images",
    url: [
      {
        photo_url: "https://lz.sinaimg.cn/nmw690/8d84366egy1gzbssrkd6dj20u01407bb.jpg"
      },
      {
        photo_url: "https://lz.sinaimg.cn/nmw690/8d84366egy1gzbsyp4pbbj20u0140n4i.jpg"
      },
      {
        photo_url: "https://lz.sinaimg.cn/nmw690/8d84366egy1gzbsyqycmqj20u0140n3x.jpg"
      },
      {
        photo_url: "https://lz.sinaimg.cn/nmw690/8d84366egy1gzbssu8c1qj20u01400xq.jpg"
      }
    ],
    content: "粉嗲粉嗲的少女单品，这颜色敢出的不多，偶尔一些亮色会增加不少活跃感，小面积，大效果，会自留的一款[兔子][兔子][兔子][兔子]"
  }
}
```

# 返回结果data参数说明

|字段名称       |字段说明         |类型            |必填            |备注     |
| -------------|:--------------:|:--------------:|:--------------:| ------:|
|type|图文/视频|string|Y||
|url|返回结果|string|N||
|content|标题|string|N||
|author|作者|string|N||
|cover|封面|string|N||

# 返回结果url参数说明

|字段名称       |字段说明         |类型            |必填            |备注     |
| -------------|:--------------:|:--------------:|:--------------:| ------:|
|photo_url|链接|string|Y||
|photo_width|宽|string|N||
|photo_height|高|string|N||


# 调用效果

移步：[lxmovie.vip](lxmovie.vip) 查看




