# 网站已到期，目前无法使用
# 请求地址

```
http://lxmovie.vip/ppx
```

# 调用方式：POST GET

# 请求实例

```
http://lxmovie.vip/ppx?url=https://h5.pipix.com/s/LTXvKo9/
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
    cover: "https://p3-ppx-sign.byteimg.com/tos-cn-p-0076/b4d10bc011d547c597b575bea6346216_1645029034~tplv-f3gpralwbh-noop:720:1280.webp?x-expires=1676969932&x-signature=GnRDd8llgoRHQvkKPBtjQf1ZzdE%3D",
    author: "冰哥已奔三",
    type: "video",
    url: "http://v3-ppx.ixigua.com/a625141b2bf4e88379c6f9b4ac2063d3/62136265/video/tos/cn/tos-cn-ve-0076/00cbaba7e9a246a497eda15d731076e3/?a=1319&br=831&bt=831&cd=0%7C0%7C0%7C0&ch=0&cr=0&cs=0&cv=1&dr=3&ds=3&eid=2048&er=&ft=XEAS8qq3mrsPnzInz7VH16iZlfuWwPJiM5&l=2022022116585201021008814515B5AD31&lr=&mime_type=video_mp4&net=0&pl=0&qs=0&rc=MzNkMzU6ZnJsOzMzNGYzM0ApZWRkO2kzOmRnNzw7MzU4OmdrZ21mcjRfYDNgLS1kMTBzczQwYi00Y2MvX2FeYDJhNTM6Yw%3D%3D&vl=&vr=",
    content: "疫情防控人人有责，可天天在家睡也快睡傻了呢…"
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
|content|标题|string|N||


# 调用效果

移步：[lxmovie.vip](lxmovie.vip) 查看




