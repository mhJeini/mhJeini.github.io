
# 请求地址

```
http://lxmovie.vip/ks
```

# 调用方式：POST GET

# 请求实例

```
http://lxmovie.vip/ks?url=https://v.kuaishou.com/eDJB76
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
    cover: "https://p1.a.yximgs.com/upic/2021/11/19/22/BMjAyMTExMTkyMjUwNDlfMTQwMjA5NTkwMV82MTE2OTcwMzk5MF8yXzY=_Bccd49a0dc8addb881fb2ec90e8e2cd10.jpg?clientCacheKey=3x8dg4xxvj4taqk.jpg&di=7ccf0917&bp=13380",
    author: "萌萌.壁纸文案",
    type: "images",
    url: [
      {
        photo_height: "964",
        photo_url: "https://p2.a.yximgs.com/ufile/atlas/NTIxMzQ3OTU4MTM1MDk3MjQ3Nl8xNjM3MzMzNDUwNjky_17.jpg",
        photo_width: "1080"
      },
      {
        photo_height: "948",
        photo_url: "https://p2.a.yximgs.com/ufile/atlas/NTIxMzQ3OTU4MTM1MDk3MjQ3Nl8xNjM3MzMzNDUwNjky_18.jpg",
        photo_width: "1080"
      }
    ],
    content: "#治愈系图片 #治愈系壁纸 #风景图片 Don"t let the best of youself down at the best of age(别再最好的年纪辜负最好的自己)"
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




