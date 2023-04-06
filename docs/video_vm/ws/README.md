# 网站已到期，目前无法使用
# 请求地址

```
http://lxmovie.vip/ws
```

# 调用方式：POST GET

# 请求实例

```
http://lxmovie.vip/ws?url=https://isee.weishi.qq.com/ws/app-pages/share/index.html?wxplay=1&id=7910Hn6FM1NlIl2Gi&spid=2878718815674211286&qua=v1_iph_weishi_8.58.0_665_app_a&chid=100081014&pkg=3670&attach=cp_reserves3_1000370011
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
    cover: "http://xp.qpic.cn/oscar_pic/0/1e274959a9dd4ae4957b750bac02pict/0",
    author: "央视频",
    type: "video",
    url: "http://v.weishi.qq.com/gzc_2651_1047_0b535qacoaaa54acakys4jrbp3aee7waaj2a.f70.mp4?dis_k=8369fb1ad30139cc281674647d30db77&dis_t=1645436230&fromtag=0&personid=h5&pver=1.0.0&wsadapt=h5_0221173710_393635000_196464653_0_0_0_27_2_0_0_0_0_0&qua=v1_ht5_qz_3.0.0_001_idc_new&wsadapt=h5_0221173710_393635000_196464653_0_0_0_27_0_0_0_0_0_0&qua=v1_ht5_qz_3.0.0_001_idc_new",
    content: "#北京冬奥会 花样滑冰表演滑 谢幕花絮名场面四：冰墩墩摔倒众人搭救，羽生结弦第一个冲上去，然而金博洋……#来央视频看冬奥"
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




