# 滇菌王客评系统 短信接口说明

## 请求路径

```bash
path = http://<ip>/Comment/index
```

### 获取服务器时间接口

```bash
method = get
url = <path>/getTime
```

返回

```json
{
    "code":0,
    "msg":1573107266,
    "count":null,
    "data":[]
}
```

### 发送短信接口

```bash
method = get
url = <path>/dostAdds?time=1573109218&tell=13888794319&type=6&sign=972e0ecd1d2f54e9942a145b462fbd28
```

参数说明：

* time: 服务器时间，每次请求前从 `getTime` 接口获取，超过15秒后该时间戳过期。

* tell: 手机号

* type: 店铺类型

* sign: 数字签名，详见下文

返回

```json
{
    "code":0,
    "msg":"\u6210\u529f",
    "count":null,
    "data":{
        "error":[],
        "success":["13888794319"]
    }
}
```

错误代码说明

|参数|值|说明|备注|
|--|--|--|--|
|code|-1|请求过期|请重新获取时间戳|
|code|-2|签名错误|--|
|code|0|请求成功|--|
|data.error|['13***']|发送失败的电话号码|--|
|data.success|['13***']|发送成功的电话号码|--|

店铺类型参考

|店铺|type|
|--|--|
|请选择分店|0|
|宏兴店|1|
|银海店|2|
|宝海店|3|
|金康园店|4|
|常之足休闲中心|5|
|丽江滇菌王JP希丽酒店|6|
|火塘不落|7|
|丽江滇菌王JP希丽酒店餐饮部|8|
|楚雄滇菌王JP希丽酒店|9|
|楚雄滇菌王JP希丽酒店餐饮部|10|
|北京元大都店|11|
|楚雄滇菌王JP希丽酒店会议部|12|
|丽江滇菌王JP希丽酒店会议部|13|
|蜜宴|14|

### 签名规则

将请求参数按照：`tell+type+time+signKey` 拼接得到字符串使用 MD5 小写加密，得到结构添加到 `sign` 字段。

如参数 `time=1573109218&tell=13888794319&type=6`，拼接参数得到 `1388879431961573109218` 使用 `md5(1388879431961573109218 + <signKey>)`，得到 `972e0ecd1d2f54e9942a145b462fbd28`，重新拼接参数得到 `time=1573109218&tell=13888794319&type=6&sign=972e0ecd1d2f54e9942a145b462fbd28`。
