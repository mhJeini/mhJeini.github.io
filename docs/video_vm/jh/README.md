# 网站已到期，目前无法使用

# 请求地址

```
http://lxmovie.vip/api/nocode
```

# 调用方式：POST GET

# 请求实例

```
http://lxmovie.vip/api/nocode?url=https://m.oasis.weibo.cn/v1/h5/share?sid=4735582318101222
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
    cover: "https://p9-sign.douyinpic.com/tos-cn-p-0015/809e6efa67844b388757986f9a8eacdc_1645359813~tplv-dy-360p.jpeg?x-expires=1646636400&x-signature=A5lYLv9XvKIJJmdUYzFDjTugQqY%3D&from=4257465056&s=&se=false&sh=&sc=&l=202202211509230102081631652E0309F2&biz_tag=feed_cover",
    music: {
      author: "人民日报",
      url: ""
    },
    author: "人民日报",
    type: "video",
    url: "https://aweme.snssdk.com/aweme/v1/play/?video_id=v0200fg10000c89351jc77u64mnj3krg&ratio=720p&line=0",
    content: "北京冬奥会闭幕式，升国旗，奏唱国歌！"
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




