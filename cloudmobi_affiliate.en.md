CloudMobi Ad Network offline API
====

Catalogue
====

* [instruction](#instruction)
* [API response](#api response)
* [TrackingLink](#trackinglink)
* [POSTBACK](#postback)

Instruction
====

Cloudmobi provides access to offline API for getting offer, and we suggest that you call “HTTP GET” once every 15 minutes. API format: `http://feed.cloudmobi.net/feed/v1/${token}/get`. The parameter “${token}” will be offered by account manager.

Access for test: `http://feed.cloudmobi.net/feed/v1/5c263df85fc084ba15b550ce3887fb6d/get`

API response
====

Parameters on JSON format returned by API

| name | type | description |
| :--: | :--: | :--: |
| msg | string | the value of msg is ok, if the call succeeded. Otherwise, it shows failed reasons |
| data | the array of offer objects | every object corresponds to one offer’s description |


Offer object

| name | type | description |
| :--: | :--: | :--: |
| id | string | offer id |
| category | string array | APP type |
| package_name | string | package name |
| desc | string | Ad description |
| title | string | Ad title |
| star | float | APP’s star level |
| payout | float | offer’s payout |
| tracking_link | string | jump link(The caller should assemble monitoring parameter) |
| creatives | the array of picture object | One offer is one creation,choose one from them when you deliver ads |
| icons | the array of picture object | One object corresponds to one icon, choose one from them |
| targets | deliver oriented object | Every object shows the oriented condition of the offer for delivering |


Picture object
| name | type | description |
| :--: | :--: | :--: |
| height | integer | the height of the picture |
| width | integer | the width of the picture |
| url | string | URL of the picture |

Deliver oriented object

| name | type | description |
| :--: | :--: | :--: |
| country | string array | the country of oriented delivering(ISO 3166-1,alpha-2 code)|
| platform | string array | Android or iOS |
| osv | string array | operating system version |


TrackingLink
====

The `tracking_link` of the `Offer` object is the link access to Google Play or App Store. Please add url parameters below to the end of the `tracking_link`.

| name | required | description |
| :--: | :--: | :--: |
| place | required | placement ID |
| country | required |country code (ISO 3166-1,alpha-2 code) |
| app | suggested | app name |
| cm.aid | suggested | Android ID |
| cm.gaid | suggested | Google Advertising ID |
| cm.idfa | suggested | IDFA |
| aff_sub1 | optional | get user-defined parameter 1 from postback link |
| aff_sub2 | optional | get user-defined parameter 2 from postback link |
| aff_sub3 | optional | get user-defined parameter 3 from postback link |

*Notice*: For better results, please add at least one of these three parameters, cm.aid/cm.gaid/cm.idfa


POSTBACK
====

Please offer POSTBACK link which support macros below :

| name | description |
| :--: | :--: |
| ${click_id} | the only ID generated when click on link |
| ${payout} | offer conversion price |
| ${country} | country, (ISO 3166-1,alpha-2 code) |
| ${place} | placement ID |
| ${offer} | offer ID |
| ${aid} | Android ID |
| ${gaid} | GAID |
| ${idfa} | IDFA |
| ${aff_sub1} | User-defined parameter 1 |
| ${aff_sub2} | User-defined parameter 2 |
| ${aff_sub3} | User-defined parameter 3 |

Once the offer conversed, CloudMobi Affiliate will postback conversion information to POSTBACK link by using ”HTTP GET”

E.g.:
```
http://some.domain.com/postback/path?clid=${click_id}&payout=${payout}&country=${country}&slot=${place}&oid=${offer}&aid=${aid}&gaid=${gaid}&idfa=${idfa}&aff_sub1=${aff_sub1}&aff_sub2=${aff_sub2}&aff_sub3=${aff_sub3}
```

