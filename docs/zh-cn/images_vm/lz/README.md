
# 请求地址

```
http://lxmovie.vip/lz
```

# 调用方式：POST GET

# 请求实例

```
http://lxmovie.vip/lz?url=https://m.oasis.weibo.cn/v1/h5/share?sid=4737140254572956
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
|content|标题|string|N||
|author|作者|string|N||

# 返回结果url参数说明

|字段名称       |字段说明         |类型            |必填            |备注     |
| -------------|:--------------:|:--------------:|:--------------:| ------:|
|photo_url|链接|string|Y||


# 调用效果

移步：[lxmovie.vip](lxmovie.vip) 查看




