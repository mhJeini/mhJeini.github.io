
# 请求地址

```
http://lxmovie.vip/mtxx
```

# 调用方式：POST GET

# 请求实例

```
http://lxmovie.vip/mtxx?url=https://show.meitu.com/detail?feed_id=6892103085407095966&root_id=1017918304&lang=zh-Hans&stat_gid=1044257784&stat_uid=1017918304
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
    url: [
      {
        photo_height: "1688",
        photo_url: "https://xximg1.meitudata.com/2kNv85xSdb0gRDGR6leCev24odVax.jpg",
        photo_width: "1125"
      },
      {
        photo_height: "1688",
        photo_url: "https://xximg1.meitudata.com/kwbdWZXcQV9mb59W3d4CaBZq5lkNx.jpg",
        photo_width: "1125"
      },
      {
        photo_height: "1687",
        photo_url: "https://xximg1.meitudata.com/JYkoJ10sqzwRW95oJgdC0ZaXEdzBk.jpg",
        photo_width: "1125"
      },
      {
        photo_height: "1920",
        photo_url: "https://xximg1.meitudata.com/3o4v5EZHw9pnW8Dw0bznTLb2a5wZn3.jpg",
        photo_width: "1440"
      }
    ],
    content: "制作超级简单的照片 拍一个照片然后进行抠图 然后加入贴纸、添加文字 选择一个边框就可以了"
  }
}
```

# 返回结果data参数说明

|字段名称       |字段说明         |类型            |必填            |备注     |
| -------------|:--------------:|:--------------:|:--------------:| ------:|
|type|图文/视频|string|Y||
|url|返回结果|string|N||
|content|标题|string|N||

# 返回结果url参数说明

|字段名称       |字段说明         |类型            |必填            |备注     |
| -------------|:--------------:|:--------------:|:--------------:| ------:|
|photo_url|链接|string|Y||
|photo_width|宽|string|N||
|photo_height|高|string|N||


# 调用效果

移步：[lxmovie.vip](lxmovie.vip) 查看




