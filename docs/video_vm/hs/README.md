# 网站已到期，目前无法使用
# 请求地址

```
http://lxmovie.vip/hs
```

# 调用方式：POST GET

# 请求实例

```
http://lxmovie.vip/hs?url=https://share.huoshan.com/hotsoon/s/Yo6SdG8c3q8/
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
    cover: "https://p26-sign.huoshanimg.com/tos-cn-i-0000c1009/af596168b25f4b3387d13cd27cdd50cb~tplv-hs-large.jpg?x-expires=1645448400&x-signature=XWfiYaDaYGNCuyf7JsfPGb8xaG0%3D&from=244475895",
    url: "http://v3-default.ixigua.com/9b723d87b21782e4e071fea009686256/6213496d/video/tos/cn/tos-cn-ve-0012/5c2ebf245157480380754e1b3877c9bc/?a=0&br=869&bt=869&cd=0%7C0%7C0%7C0&ch=0&cr=0&cs=0&cv=1&dr=3&ds=6&er=&ft=sYRSY33Gnz7ThEP~9DXq&l=20220221151213010212063027071D5782&lr=&mime_type=video_mp4&net=0&pl=0&qs=0&rc=M285Nzs6Zmt5OzMzNGYzM0ApMzM4ZWQzPDtmN2g2aTpoN2djLXNkcjRnNTVgLS1kLS9zc2JeYzIuMy01Ni4vNWAvXi46Yw%3D%3D&vl=&vr="
  }
}
```

# 返回结果data参数说明

|字段名称       |字段说明         |类型            |必填            |备注     |
| -------------|:--------------:|:--------------:|:--------------:| ------:|
|cover|封面链接|string|N||
|type|图文/视频|string|Y||
|url|视频链接|string|N||


# 调用效果

移步：[lxmovie.vip](lxmovie.vip) 查看




