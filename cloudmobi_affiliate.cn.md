CloudMobi 网盟离线API
====

目录
====

* [说明](#说明)
* [API响应](#api响应)
* [TrackingLink](#trackinglink)
* [POSTBACK](#postback)

说明
====

Cloudmobi提供离线API接口，提供网盟offer，建议每15分钟调用（HTTP GET）该接口一次。API格式：`http://feed.cloudmobi.net/feed/v1/${token}/get`，其中${token}请向cloudmobi的账户经理索取。

测试接口：`http://feed.cloudmobi.net/feed/v1/5c263df85fc084ba15b550ce3887fb6d/get`

API响应
====

API返回JSON格式的数据

| 字段名 | 字段类型 | 字段描述 |
| :--: | :--: | :--: |
| msg | string | 调用成功，msg的值为ok，否则为错误原因 |
| data | Offer对象数组 | 每个对象为一个offer的描述 |


Offer对象

| 字段名 | 字段类型 | 字段描述 |
| :--: | :--: | :--: |
| id | 字符串 | offer id |
| category | 字符串数组 | APP类型 |
| package_name | 字符串 | 包名 |
| desc | 字符串 | 广告描述 |
| title | 字符串 | 广告标题 |
| star | 浮点数 | 应用星级 |
| tracking_link | 字符串 | 跳转链接（需要调用者拼接监测参数） |
| creatives | 图片对象数组 | 每个对象为表示一个创意，投放时选择一个合适的创意即可 |
| icons | 图片对象数组 | 每个对象为表示一个icon，投放时选择一个合适的icon即可 |
| targets | 投放定向对象 | 每个对象表示该offer的投放定向条件 |


图片对象

| 字段名 | 字段类型 | 字段描述 |
| :--: | :--: | :--: |
| height | 整数 | 图片高 |
| width | 整数 | 图片宽 |
| url | 字符串 | 图片URL |

投放定向对象

| 字段名 | 字段类型 | 字段描述 |
| :--: | :--: | :--: |
| country | 字符串数组 | 定向投放的国家（ISO 2字母缩写） |
| platform | 字符串数组 | Android或iOS |
| osv | 字符串数组 | 操作系统版本号 |


TrackingLink
====

`Offer`对象中的`tracking_link`是点击跳转到GooglePlay或AppStore的链接，您需要在该链接后面带以下url参数

| 参数名 | 是否必须 | 描述 |
| :--: | :--: | :--: |
| place | 必须 | 广告位ID |
| country | 必须 | ISO标准的2字母国家代码 |
| app | 建议 | app名称 |
| cm.aid | 建议 | Android ID |
| cm.gaid | 建议 | Google Advertising ID |
| cm.idfa | 建议 | IDFA |
| aff_sub1 | 可选 | 用户自定义参数1，在postback中回传 |
| aff_sub2 | 可选 | 用户自定义参数2，在postback中回传 |
| aff_sub3 | 可选 | 用户自定义参数3，在postback中回传 |

*注意*：为保证广告效果，cm.aid/cm.gaid/cm.idfa三者中至少需要带一个


POSTBACK
====

请提供POSTBACK链接，POSTBACK链接中支持以下宏：

| 参数名 | 描述 |
| :--: | :--: |
| ${click_id} | 点击时生成的唯一ID |
| ${payout} | offer转化价格 |
| ${country} | 国家，ISO 2字母代码 |
| ${place} | 广告位ID |
| ${offer} | offer ID |
| ${aid} | Andrdoi ID |
| ${gaid} | GAID |
| ${idfa} | IDFA |
| ${aff_sub1} | 用户自定义参数1 |
| ${aff_sub2} | 用户自定义参数2 |
| ${aff_sub3} | 用户自定义参数3 |

一旦监测到offer发生转化，CloudMobi Affiliate会立刻使用HTTP GET方式向POSTBACK链接回传转化信息

示例：

```
http://some.domain.com/postback/path?clid=${click_id}&payout=${payout}&country=${country}&slot=${place}&oid=${offer}&aid=${aid}&gaid=${gaid}&idfa=${idfa}&aff_sub1=${aff_sub1}&aff_sub2=${aff_sub2}&aff_sub3=${aff_sub3}
```

