# 网站已到期，目前无法使用
# 请求地址

```
http://lxmovie.vip/api/nocode
```

# 调用方式：POST GET

# 请求实例

```
http://lxmovie.vip/api/nocode?url=https://v.douyin.com/LsJQwyq/
支持列表：抖音/皮皮虾/火山/微视/微博/绿洲/最右/轻视频/instagram/哔哩哔哩/快手/全民小视频/皮皮搞笑
全民k歌/巴塞电影/陌陌/Before避风/开眼/Vue Vlog/小咖秀/西瓜视频/逗拍
```

# 请求参数

|字段名称       |字段说明         |类型            |必填            |备注     |
| -------------|:--------------:|:--------------:|:--------------:| ------:|
|url|需要解析的视频链接|string|Y||

# 返回数据

```json
{"code":200,
 "msg":"解析成功",
 "data":{
   "cover":"https://p6-sign.douyinpic.com/tos-cn-p-0015/e457e61f9e674205b35930ace2b1e789_1641551728~tplv-dy-360p.jpeg?x-expires=1646546400&x-signature=VwSUNrhjFERNE9U1JlDMRcBJKVs%3D&from=4257465056&s=&se=false&sh=&sc=&l=20220220144247010210057040107C5AEE&biz_tag=feed_cover",
   "msg":"解析成功",
   "music":{
     "author":"13z",
     "url":"https://sf6-cdn-tos.douyinstatic.com/obj/ies-music/7011804161452854047.mp3"
   },
   "code":200,
   "author":"米叮Z",
   "type":"video",
   "url":"https://aweme.snssdk.com/aweme/v1/play/?video_id=v0d00fg10000c7c1dlbc77u38e0bnet0&ratio=720p&line=0","content":"pk一下觉得上一条跟这个更喜欢哪个啊"
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




