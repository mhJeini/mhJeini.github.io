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
  "msg":"操作成功",
  "code":0,
  "data":{
    "author":"小白金金M",
    "type":"images",
    "url":[
      {
        "photo_url":"https://lz.sinaimg.cn/nmw690/78de7e23ly1gze5i43bubj21zm2niqv5.jpg"
      },
      {
        "photo_url":"https://lz.sinaimg.cn/nmw690/78de7e23ly1gze5hubl5fj21ns27p1kx.jpg"
      },
      {
        "photo_url":"https://lz.sinaimg.cn/nmw690/78de7e23ly1gze5hqgz2dj21xs2l0qv5.jpg"
      },
      {
        "photo_url":"https://lz.sinaimg.cn/nmw690/78de7e23ly1gze5hynxfuj22d91rxb29.jpg"
      },
      {
        "photo_url":"https://lz.sinaimg.cn/nmw690/78de7e23ly1gze5hw77lqj21jn2277wh.jpg"
      },
      {
        "photo_url":"https://lz.sinaimg.cn/nmw690/78de7e23ly1gze5ib6mj5j22t823xqv5.jpg"
      },
      {
        "photo_url":"https://lz.sinaimg.cn/nmw690/78de7e23ly1gze5htc8s2j21mu26hhdt.jpg"
      },
      {
        "photo_url":"https://lz.sinaimg.cn/nmw690/78de7e23ly1gze5i83419j22c0340e82.jpg"
      },
      {
        "photo_url":"https://lz.sinaimg.cn/nmw690/78de7e23ly1gze5i143c1j222o2rk4qq.jpg"
      }
    ],
    "content":"节日快乐～ 来，我看看哪个小机灵鬼能数清楚我身上到底多少个蝴蝶结[偷乐] #约会穿搭 #完美新年计划 #虎年开运穿搭"
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




