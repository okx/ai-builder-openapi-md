---
site: turkey
lang: zh
name: 获取经济日历数据
method: GET
path: /api/v5/public/economic-calendar
module: public
rest: https://tr.okx.com
ws: wss://ws.okx.com:8443
supported: true
---

### 获取经济日历数据 [public-data-rest-api-get-economic-calendar-data]

<aside class="notice">
该接口需验证后使用。仅支持实盘服务。
</aside>

获取过去三个月的宏观经济日历数据。三个月前的历史数据仅开放给交易费等级VIP1及以上的用户。

#### 限速：1次/5s
#### 限速规则：IP

#### HTTP请求

`GET /api/v5/public/economic-calendar`

> 请求示例

```shell
GET /api/v5/public/economic-calendar
```

#### 请求参数

| 参数名 | 类型   | 是否必须 | 描述                                                   |
| --- | --- | --- | --- |
| region | string | 否 | 国家，地区或实体 <br>`afghanistan`, `albania`, `algeria`, `andorra`, `angola`, `antigua_and_barbuda`, `argentina`, `armenia`, `aruba`, `australia`, `austria`, `azerbaijan`, `bahamas`, `bahrain`, `bangladesh`, `barbados`, `belarus`, `belgium`, `belize`, `benin`, `bermuda`, `bhutan`, `bolivia`, `bosnia_and_herzegovina`, `botswana`, `brazil`, `brunei`, `bulgaria`, `burkina_faso`, `burundi`, `cambodia`, `cameroon`, `canada`, `cape_verde`, `cayman_islands`, `central_african_republic`, `chad`, `chile`, `china`, `colombia`, `comoros`, `congo`, `costa_rica`, `croatia`, `cuba`, `cyprus`, `czech_republic`, `denmark`, `djibouti`, `dominica`, `dominican_republic`, `east_timor`, `ecuador`, `egypt`, `el_salvador`, `equatorial_guinea`, `eritrea`, `estonia`, `ethiopia`, `euro_area`, `european_union`, `faroe_islands`, `fiji`, `finland`, `france`, `g20`, `g7`, `gabon`, `gambia`, `georgia`, `germany`, `ghana`, `greece`, `greenland`, `grenada`, `guatemala`, `guinea`, `guinea_bissau`, `guyana`, `hungary`, `haiti`, `honduras`, `hong_kong`, `hungary`, `imf`, `indonesia`, `iceland`, `india`, `indonesia`, `iran`, `iraq`, `ireland`, `isle_of_man`, `israel`, `italy`, `ivory_coast`, `jamaica`, `japan`, `jordan`, `kazakhstan`, `kenya`, `kiribati`, `kosovo`, `kuwait`, `kyrgyzstan`, `laos`, `latvia`, `lebanon`, `lesotho`, `liberia`, `libya`, `liechtenstein`, `lithuania`, `luxembourg`, `macau`, `macedonia`, `madagascar`, `malawi`, `malaysia`, `maldives`, `mali`, `malta`, `mauritania`, `mauritius`, `mexico`, `micronesia`, `moldova`, `monaco`, `mongolia`, `montenegro`, `morocco`, `mozambique`, `myanmar`, `namibia`, `nepal`, `netherlands`, `new_caledonia`, `new_zealand`, `nicaragua`, `niger`, `nigeria`, `north_korea`, `northern_mariana_islands`, `norway`, `opec`, `oman`, `pakistan`, `palau`, `palestine`, `panama`, `papua_new_guinea`, `paraguay`, `peru`, `philippines`, `poland`, `portugal`, `puerto_rico`, `qatar`, `russia`, `republic_of_the_congo`, `romania`, `russia`, `rwanda`, `slovakia`, `samoa`, `san_marino`, `sao_tome_and_principe`, `saudi_arabia`, `senegal`, `serbia`, `seychelles`, `sierra_leone`, `singapore`, `slovakia`, `slovenia`, `solomon_islands`, `somalia`, `south_africa`, `south_korea`, `south_sudan`, `spain`, `sri_lanka`, `st_kitts_and_nevis`, `st_lucia`, `sudan`, `suriname`, `swaziland`, `sweden`, `switzerland`, `syria`, `taiwan`, `tajikistan`, `tanzania`, `thailand`, `togo`, `tonga`, `trinidad_and_tobago`, `tunisia`, `turkey`, `turkmenistan`, `uganda`, `ukraine`, `united_arab_emirates`, `united_kingdom`, `united_states`, `uruguay`, `uzbekistan`, `vanuatu`, `venezuela`, `vietnam`, `world`, `yemen`, `zambia`, `zimbabwe`|
| importance | string | 否 | 重要性 <br>`1`: 低 <br>`2`: 中等 <br>`3`: 高 |
| before | String | 否 | 查询发布日期(date)之后的内容，值为时间戳，Unix 时间戳为毫秒数格式，如 `1597026383085` |
| after | String | 否 | 查询发布日期(date)之前的内容，值为时间戳，Unix 时间戳为毫秒数格式，如 `1597026383085` <br>默认值为请求时刻的时间戳 |
| limit | String | 否 | 分页返回结果的数量，最大为100，默认100条 |

> 返回结果

```json
{
    "code": "0",
    "data": [
        {
            "actual": "7.8%",
            "calendarId": "330631",
            "category": "Harmonised Inflation Rate YoY",
            "ccy": "",
            "date": "1700121600000",
            "dateSpan": "0",
            "event": "Harmonised Inflation Rate YoY",
            "forecast": "7.8%",
            "importance": "1",
            "prevInitial": "",
            "previous": "9%",
            "refDate": "1698710400000",
            "region": "Slovakia",
            "uTime": "1700121605007",
            "unit": "%"
        }
    ],
    "msg": ""
}

```

#### 返回参数

| 参数名          | 类型 | 描述                                              |
|--------------| --- |-------------------------------------------------|
| calendarId   | string | 经济日历ID                                          |
| date         | string | actual字段值的预期发布时间，Unix时间戳的毫秒数格式，如 `1597026383085` |
| region       | string | 国家，地区或实体                                        |
| category     | string | 类别名                                             |
| event        | string | 事件名                                             |
| refDate      | string | 当前事件指向的日期                                       |
| actual       | string | 事件实际值                                           |
| previous     | string | 当前事件上个周期的最新实际值。<br>若发生数据修正，该字段存储上个周期修正后的实际值。    |
| forecast     | string | 由权威经济学家共同得出的预测值                                 |
| dateSpan     | string | `0`：事件的具体发生时间已知<br>`1`：事件的具体发生日期已知，但时间未知        |
| importance   | string | 重要性 <br>`1`: 低 <br>`2`: 中等 <br>`3`: 高           |
| uTime        | string | 当前事件的最新更新时间，Unix时间戳的毫秒数格式，如 `1597026383085`     |
| prevInitial  | string | 该事件上一周期的初始值<br>仅在修正发生时有值                        |
| ccy          | string | 事件实际值对应的货币                                      |
| unit         | string | 事件实际值对应的单位                                      |
