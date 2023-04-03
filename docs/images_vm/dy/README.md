
# 请求地址

```
http://lxmovie.vip/dy
```

# 调用方式：POST GET

# 请求实例

```
http://lxmovie.vip/dy?url=https://v.douyin.com/LTCMg88/
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
    cover: "",
    music: {
      author: "蒋雪儿",
      url: "https://sf6-cdn-tos.douyinstatic.com/obj/tos-cn-ve-2774/bce6c7a28d7d4bf28cc5462ef7024465"
    },
    author: "小AI剪辑（动漫）",
    type: "images",
    url: [
      {
        photo_height: "2260",
        photo_url: "https://p3-sign.douyinpic.com/tos-cn-i-0813/386348748a7d43a090c66c8c7ee14e73~noop.webp?x-expires=1648022400&x-signature=VtK8AAfPY8dry%2FavucJwNNua%2Bko%3D&from=4257465056&s=PackSourceEnum_DOUYIN_REFLOW&se=false&sh=&sc=&l=2022022116184501019404521248008D90&biz_tag=aweme_images",
        photo_width: "1087"
      },
      {
        photo_height: "2260",
        photo_url: "https://p26-sign.douyinpic.com/tos-cn-i-0813/35674c5a441b43d2885d4c5df4e86713~noop.webp?x-expires=1648022400&x-signature=ZNGsm6wX1mysVcOSebL5DUK5oe8%3D&from=4257465056&s=PackSourceEnum_DOUYIN_REFLOW&se=false&sh=&sc=&l=2022022116184501019404521248008D90&biz_tag=aweme_images",
        photo_width: "1017"
      }
    ],
    content: "#龙蛇演义 #唐紫尘 #国漫壁纸 唐紫尘背影真的又美又飒，简直绝绝子，后面还有旗袍裙，职业装期待中。@抖音小助手 "
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

# 返回结果url参数说明

|字段名称       |字段说明         |类型            |必填            |备注     |
| -------------|:--------------:|:--------------:|:--------------:| ------:|
|photo_url|链接|string|Y||
|photo_width|宽|string|N||
|photo_height|高|string|N||

# 返回结果music参数说明

|字段名称       |字段说明         |类型            |必填            |备注     |
| -------------|:--------------:|:--------------:|:--------------:| ------:|
|url|链接|string|Y||
|author|作者|string|N||


# 调用效果

移步：[lxmovie.vip](lxmovie.vip) 查看




