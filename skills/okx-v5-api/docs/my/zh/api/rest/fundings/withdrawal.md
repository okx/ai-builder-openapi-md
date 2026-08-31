---
site: my
lang: zh
name: 提币
method: POST
path: /api/v5/asset/withdrawal
module: asset
rest: https://eea.okx.com
ws: wss://wseea.okx.com:8443
supported: true
---

### 提币 [funding-account-rest-api-withdrawal]

支持资金账户资产提币。普通子账户不支持提币。

<aside class="notice">
API只能提币到免认证地址/账户上，通过 WEB/APP 可以设置免认证地址。
</aside>

<aside class="notice">
关于标签<br>
某些币种如XRP充币时同时需要一个充值地址和标签（又名memo/payment_id），标签是一种保证您的充币地址唯一性的数字串，与充币地址成对出现并一一对应。请您务必遵守正确的充值步骤，在提币时输入完整信息，否则将面临丢失币的风险！<br>
对于有标签的币种，如果是OKX用户间的提币，请走内部转账不要走链上提币。
</aside>

<aside class="notice">
欧洲经济区用户 API 提币受限<br>
由于欧洲经济区的数字货币转账规则更新，欧洲经济区用户无法使用 API 提币到私人钱包，需要通过网页端或 App进行提币操作；提币到交易所钱包，需要传入接收方信息。更多详情参考：https://my.okx.com/docs-v5/log_zh/#2025-01-14-withdrawal-api-adjustment-for-eea-entity-users
</aside>

#### 限速：6次/s
#### 限速规则：User ID

#### HTTP请求

`POST /api/v5/asset/withdrawal`

> 请求示例

```shell
# 提币至交易所钱包
POST /api/v5/asset/withdrawal
body
{
    "amt": "1000",
    "dest": "4",
    "ccy": "USDT",
    "toAddr": "TYW7C5qjhQtuhjz5qLQtA9bzaeEFo6ueg7",
    "chain": "USDT-TRC20",
    "rcvrInfo": {
        "walletType": "exchange",
        "exchId":"did:ethr:0x401c4bc0241bc787f64510f21a4ae7ec3603487c",
        "rcvrFirstName":"Bruce",
        "rcvrLastName":"Wayne",
        "rcvrCountry":"United States",
        "rcvrStreetName":"Clementi Avenue 1",
        "rcvrTownName":"San Jose",
        "rcvrCountrySubDivision":"California"
    }
}
```

```python
import okx.Funding as Funding

# API 初始化
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # 实盘: 0, 模拟盘: 1

fundingAPI = Funding.FundingAPI(apikey, secretkey, passphrase, False, flag)

# 提币
result = fundingAPI.withdrawal(
    ccy="USDT",
    toAddr="TXtvfb7cdrn6VX9H49mgio8bUxZ3DGfvYF",
    amt="100",
    dest="4",
    chain="USDT-TRC20"
)
print(result)
```

#### 请求参数

| 参数名                      | 类型     | 是否必须    | 描述                                                                                                                                                                                     |
|:-------------------------|:-------|:--------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ccy                      | String | 是       | 币种，如 `USDT`                                                                                                                                                                            |
| amt                      | String | 是       | 提币数量<br>该数量不包含手续费。提币时需预留足够的手续费。<br>链上提币所需网络手续费可以通过接口 [获取币种列表](/docs-v5/zh/#funding-account-rest-api-get-currencies) 获取<br>内部转账无需手续费                                                    |
| dest                     | String | 是       | 提币方式<br>`3`：内部转账 <br>`4`：链上提币                                                                                                                                                          |
| toAddr                   | String | 是       | `toAddr`必须是认证过的地址/账户。如果选择链上提币，某些数字货币地址格式为`地址:标签`，如 `ARDOR-7JF3-8F2E-QUWZ-CAN7F:123456`<br>如果选择内部转账，`toAddr`必须是接收方地址，可以是UID（仅白名单用户）、邮箱、手机或者账户名（只有子账户才有账户名）。                                         |
| toAddrType               | String | 否        | 地址类型<br>`1`: 钱包地址、邮箱、手机号、登录账户名<br>`2`: UID（仅适用于 dest=`3`） |
| chain                    | String | 可选      | 币种链信息<br>如`USDT`下有`USDT-ERC20`，`USDT-TRC20`多个链<br>如果不填此参数，则默认为主链<br>对于无效资产提币，不填此参数，则默认为唯一的提币链<br>适用于`链上提币`，链信息可以通过接口 [获取币种列表](/docs-v5/zh/#funding-account-rest-api-get-currencies) 获取 |
| areaCode                 | String | 可选      | 手机区号，如 `86`<br>当`toAddr`为手机号时，该参数必填<br>适用于`内部转账`                                                                                                                                       |
| rcvrInfo                 | Object | 可选      | 接收方信息<br>特定主体用户做`链上提币`/`闪电网络提币` 需要提供此信息                                                                                                                                                |
| > walletType             | String | 是       | 钱包类型<br>`exchange`：提币到交易所钱包<br>`private`：提币到私人钱包<br>对于钱包接收方为公司的，`rcvrFirstName`可以填公司名称，`rcvrLastName`可以填"N/A"，地址信息可以填写公司注册地址。    |
| > exchId                 | String | 可选      | 交易所 ID<br>可以通过 [获取交易所列表（公共）](/docs-v5/zh/#funding-account-rest-api-get-exchange-list-public) 接口查询支持的交易所<br>如果交易所不在支持的交易所列表中，该字段填`0`<br>适用于walletType=`exchange`                        |
| > rcvrFirstName          | String | 可选      | 接收方名字，如 `Bruce`                                                                                                                                          |
| > rcvrLastName           | String | 可选      | 接收方姓氏，如 `Wayne`                                                                                                                                     |
| > rcvrCountry            | String | 可选      | 接收方所在国家，如 `United States`<br>必须输入英文国家名称，或者两字母国家代码(ISO 3166-1)。输入内容参考下方国家信息表中`国家名称(英)`，`国家代码`                                                              |
| > rcvrCountrySubDivision | String | 可选      | 接收方所在州/省，如 `California`                                                                                                                                   |
| > rcvrTownName           | String | 可选      | 接收方所在城镇，如 `San Jose`                                                                                                                                  |
| > rcvrStreetName         | String | 可选      | 接收方所在街道地址，如 `Clementi Avenue 1`                                                                                                                            |
| clientId                 | String | 否       | 客户自定义ID<br>字母（区分大小写）与数字的组合，可以是纯字母、纯数字且长度要在1-32位之间。                                                                                                                                     |

> 返回结果

```json
{
	"code": "0",
	"msg": "",
	"data": [{
		"amt": "0.1",
		"wdId": "67485",
		"ccy": "BTC",
		"clientId": "",
		"chain": "BTC-Bitcoin"
	}]
}
```

#### 返回参数

| 参数名 | 类型   | 描述       |
| :----- | :----- | :--------- |
| ccy    | String | 提币币种   |
| chain  | String | 币种链信息<br>有的币种下有多个链，必须要做区分，如`USDT`下有`USDT-ERC20`，`USDT-TRC20`多个链 |
| amt    | String | 提币数量   |
| wdId   | String | 提币申请ID |
| clientId  | String | 客户自定义ID   |

#### 国家信息表

| 国家名称(英)                                      | 国家名称(中)             | 国家代码 |
|----------------------------------------------|---------------------|------|
| Afghanistan                                  | 阿富汗                 | AF   |
| Albania                                      | 阿尔巴尼亚               | AL   |
| Algeria                                      | 阿尔及利亚               | DZ   |
| Andorra                                      | 安道尔                 | AD   |
| Angola                                       | 安哥拉                 | AO   |
| Anguilla                                     | 安圭拉                 | AI   |
| Antigua and Barbuda                          | 安提瓜和巴布达             | AG   |
| Argentina                                    | 阿根廷                 | AR   |
| Armenia                                      | 亚美尼亚                | AM   |
| Australia                                    | 澳大利亚                | AU   |
| Austria                                      | 奥地利                 | AT   |
| Azerbaijan                                   | 阿塞拜疆                | AZ   |
| Bahamas                                      | 巴哈马                 | BS   |
| Bahrain                                      | 巴林                  | BH   |
| Bangladesh                                   | 孟加拉国                | BD   |
| Barbados                                     | 巴巴多斯                | BB   |
| Belarus                                      | 白俄罗斯                | BY   |
| Belgium                                      | 比利时                 | BE   |
| Belize                                       | 伯利兹                 | BZ   |
| Benin                                        | 贝宁                  | BJ   |
| Bermuda                                      | 百慕大                 | BM   |
| Bhutan                                       | 不丹                  | BT   |
| Bolivia                                      | 玻利维亚                | BO   |
| Bosnia and Herzegovina                       | 波斯尼亚和黑塞哥维那 (波黑)     | BA   |
| Botswana                                     | 博茨瓦纳                | BW   |
| Brazil                                       | 巴西                  | BR   |
| British Virgin Islands                       | 英属维尔京群岛             | VG   |
| Brunei                                       | 文莱                  | BN   |
| Bulgaria                                     | 保加利亚                | BG   |
| Burkina Faso                                 | 布基纳法索               | BF   |
| Burundi                                      | 布隆迪                 | BI   |
| Cambodia                                     | 柬埔寨                 | KH   |
| Cameroon                                     | 喀麦隆                 | CM   |
| Canada                                       | 加拿大                 | CA   |
| Cape Verde                                   | 佛得角                 | CV   |
| Cayman Islands                               | 开曼群岛                | KY   |
| Central African Republic                     | 中非共和国               | CF   |
| Chad                                         | 乍得                  | TD   |
| Chile                                        | 智利                  | CL   |
| Colombia                                     | 哥伦比亚                | CO   |
| Comoros                                      | 科摩罗                 | KM   |
| Congo (Republic)                             | 刚果共和国               | CG   |
| Congo (Democratic Republic)                  | 刚果民主共和国             | CD   |
| Costa Rica                                   | 哥斯达黎加               | CR   |
| Cote d´Ivoire (Ivory Coast)                  | 象牙海岸                | CI   |
| Croatia                                      | 克罗地亚                | HR   |
| Cuba                                         | 古巴                  | CU   |
| Cyprus                                       | 塞浦路斯                | CY   |
| Czech Republic                               | 捷克共和国               | CZ   |
| Denmark                                      | 丹麦                  | DK   |
| Djibouti                                     | 吉布提                 | DJ   |
| Dominica                                     | 多米尼克                | DM   |
| Dominican Republic                           | 多明尼加共和国             | DO   |
| Ecuador                                      | 厄瓜多尔                | EC   |
| Egypt                                        | 埃及                  | EG   |
| El Salvador                                  | 萨尔瓦多                | SV   |
| Equatorial Guinea                            | 赤道几内亚               | GQ   |
| Eritrea                                      | 厄立特里亚               | ER   |
| Estonia                                      | 爱沙尼亚                | EE   |
| Ethiopia                                     | 埃塞俄比亚               | ET   |
| Fiji                                         | 斐济                  | FJ   |
| Finland                                      | 芬兰                  | FI   |
| France                                       | 法国                  | FR   |
| Gabon                                        | 加蓬                  | GA   |
| Gambia                                       | 冈比亚                 | GM   |
| Georgia                                      | 格鲁吉亚                | GE   |
| Germany                                      | 德国                  | DE   |
| Ghana                                        | 加纳                  | GH   |
| Greece                                       | 希腊                  | GR   |
| Grenada                                      | 格林纳达                | GD   |
| Guatemala                                    | 危地马拉                | GT   |
| Guinea                                       | 几内亚                 | GN   |
| Guinea-Bissau                                | 几内亚比绍               | GW   |
| Guyana                                       | 圭亚那                 | GY   |
| Haiti                                        | 海地                  | HT   |
| Honduras                                     | 洪都拉斯                | HN   |
| Hong Kong                                    | 香港                  | HK   |
| Hungary                                      | 匈牙利                 | HU   |
| Iceland                                      | 冰岛                  | IS   |
| India                                        | 印度                  | IN   |
| Indonesia                                    | 印度尼西亚               | ID   |
| Iran                                         | 伊朗                  | IR   |
| Iraq                                         | 伊拉克                 | IQ   |
| Ireland                                      | 爱尔兰                 | IE   |
| Israel                                       | 以色列                 | IL   |
| Italy                                        | 意大利                 | IT   |
| Jamaica                                      | 牙买加                 | JM   |
| Japan                                        | 日本                  | JP   |
| Jordan                                       | 约旦                  | JO   |
| Kazakhstan                                   | 哈萨克斯坦               | KZ   |
| Kenya                                        | 肯尼亚                 | KE   |
| Kiribati                                     | 基里巴斯                | KI   |
| North Korea                                  | 朝鲜                  | KP   |
| South Korea                                  | 韩国                  | KR   |
| Kuwait                                       | 科威特                 | KW   |
| Kyrgyzstan                                   | 吉尔吉斯斯坦              | KG   |
| Laos                                         | 老挝                  | LA   |
| Latvia                                       | 拉脱维亚                | LV   |
| Lebanon                                      | 黎巴嫩                 | LB   |
| Lesotho                                      | 莱索托                 | LS   |
| Liberia                                      | 利比里亚                | LR   |
| Libya                                        | 利比亚                 | LY   |
| Liechtenstein                                | 列支敦士登               | LI   |
| Lithuania                                    | 立陶宛                 | LT   |
| Luxembourg                                   | 卢森堡                 | LU   |
| Macau                                        | 澳门                  | MO   |
| Macedonia                                    | 马其顿                 | MK   |
| Madagascar                                   | 马达加斯加               | MG   |
| Malawi                                       | 马拉维                 | MW   |
| Malaysia                                     | 马来西亚                | MY   |
| Maldives                                     | 马尔代夫                | MV   |
| Mali                                         | 马里                  | ML   |
| Malta                                        | 马耳他                 | MT   |
| Marshall Islands                             | 马绍尔群岛               | MH   |
| Mauritania                                   | 毛里塔尼亚               | MR   |
| Mauritius                                    | 毛里求斯                | MU   |
| Mexico                                       | 墨西哥                 | MX   |
| Micronesia                                   | 密克罗尼西亚              | FM   |
| Moldova                                      | 摩尔多瓦                | MD   |
| Monaco                                       | 摩纳哥                 | MC   |
| Mongolia                                     | 蒙古                  | MN   |
| Montenegro                                   | 黑山                  | ME   |
| Morocco                                      | 摩洛哥                 | MA   |
| Mozambique                                   | 莫桑比克                | MZ   |
| Myanmar (Burma)                              | 缅甸                  | MM   |
| Namibia                                      | 纳米比亚                | NA   |
| Nauru                                        | 瑙鲁                  | NR   |
| Nepal                                        | 尼泊尔                 | NP   |
| Netherlands                                  | 荷兰                  | NL   |
| New Zealand                                  | 新西兰                 | NZ   |
| Nicaragua                                    | 尼加拉瓜                | NI   |
| Niger                                        | 尼日尔                 | NE   |
| Nigeria                                      | 尼日利亚                | NG   |
| Norway                                       | 挪威                  | NO   |
| Oman                                         | 阿曼                  | OM   |
| Pakistan                                     | 巴基斯坦                | PK   |
| Palau                                        | 帕劳                  | PW   |
| Panama                                       | 巴拿马                 | PA   |
| Papua New Guinea                             | 巴布亚新几内亚             | PG   |
| Paraguay                                     | 巴拉圭                 | PY   |
| Peru                                         | 秘鲁                  | PE   |
| Philippines                                  | 菲律宾                 | PH   |
| Poland                                       | 波兰                  | PL   |
| Portugal                                     | 葡萄牙                 | PT   |
| Qatar                                        | 卡塔尔                 | QA   |
| Romania                                      | 罗马尼亚                | RO   |
| Russia                                       | 俄国                  | RU   |
| Rwanda                                       | 卢旺达                 | RW   |
| Saint Kitts and Nevis                        | 圣基茨和尼维斯             | KN   |
| Saint Lucia                                  | 圣卢西亚                | LC   |
| Saint Vincent and the Grenadines             | 圣文森特和格林纳丁斯          | VC   |
| Samoa                                        | 萨摩亚                 | WS   |
| San Marino                                   | 圣马力诺                | SM   |
| Sao Tome and Principe                        | 圣多美和普林西比            | ST   |
| Saudi Arabia                                 | 沙特阿拉伯               | SA   |
| Senegal                                      | 塞内加尔                | SN   |
| Serbia                                       | 塞尔维亚                | RS   |
| Seychelles                                   | 塞舌尔                 | SC   |
| Sierra Leone                                 | 塞拉利昂                | SL   |
| Singapore                                    | 新加坡                 | SG   |
| Slovakia                                     | 斯洛伐克                | SK   |
| Slovenia                                     | 斯洛文尼亚               | SI   |
| Solomon Islands                              | 所罗门群岛               | SB   |
| Somalia                                      | 索马里                 | SO   |
| South Africa                                 | 南非                  | ZA   |
| Spain                                        | 西班牙                 | ES   |
| Sri Lanka                                    | 斯里兰卡                | LK   |
| Sudan                                        | 苏丹                  | SD   |
| Suriname                                     | 苏里南                 | SR   |
| Swaziland                                    | 斯威士兰                | SZ   |
| Sweden                                       | 瑞典                  | SE   |
| Switzerland                                  | 瑞士                  | CH   |
| Syria                                        | 叙利亚                 | SY   |
| Taiwan                                       | 台湾                  | TW   |
| Tajikistan                                   | 塔吉克斯坦               | TJ   |
| Tanzania                                     | 坦桑尼亚                | TZ   |
| Thailand                                     | 泰国                  | TH   |
| Timor-Leste (East Timor)                     | 东帝汶                 | TL   |
| Togo                                         | 多哥                  | TG   |
| Tonga                                        | 汤加                  | TO   |
| Trinidad and Tobago                          | 特里尼达和多巴哥            | TT   |
| Tunisia                                      | 突尼斯                 | TN   |
| Turkey                                       | 土耳其                 | TR   |
| Turkmenistan                                 | 土库曼斯坦               | TM   |
| Tuvalu                                       | 图瓦卢                 | TV   |
| U.S. Virgin Islands                          | 美属维尔京群岛             | VI   |
| Uganda                                       | 乌干达                 | UG   |
| Ukraine                                      | 乌克兰                 | UA   |
| United Arab Emirates                         | 阿拉伯联合酋长国            | AE   |
| United Kingdom                               | 英国                  | GB   |
| United States                                | 美国                  | US   |
| Uruguay                                      | 乌拉圭                 | UY   |
| Uzbekistan                                   | 乌兹别克斯坦              | UZ   |
| Vanuatu                                      | 瓦努阿图                | VU   |
| Vatican City                                 | 梵蒂冈城                | VA   |
| Venezuela                                    | 委内瑞拉                | VE   |
| Vietnam                                      | 越南                  | VN   |
| Yemen                                        | 也门                  | YE   |
| Zambia                                       | 赞比亚                 | ZM   |
| Zimbabwe                                     | 津巴布韦                | ZW   |
| Kosovo                                       | 科索沃                 | XK   |
| South Sudan                                  | 南苏丹                 | SS   |
| China                                        | 中国                  | CN   |
| Palestine                                    | 巴勒斯坦                | PS   |
| Curacao                                      | 库拉索                 | CW   |
| Dominican Republic                           | 多明尼加共和国             | DO   |
| Dominican Republic                           | 多明尼加共和国             | DO   |
| Gibraltar                                    | 英属直布罗陀              | GI   |
| New Caledonia                                | 新喀里多尼亚              | NC   |
| Cook Islands                                 | 库克群岛                | CK   |
| Reunion                                      | 留尼旺                 | RE   |
| Guernsey                                     | 根西岛                 | GG   |
| Guadeloupe                                   | 瓜德罗普                | GP   |
| Martinique                                   | 马提尼克                | MQ   |
| French Polynesia                             | 法属波利尼西亚             | PF   |
| Faroe Islands                                | 法罗群岛                | FO   |
| Greenland                                    | 格陵兰岛                | GL   |
| Jersey                                       | 泽西岛                 | JE   |
| Aruba                                        | 阿鲁巴                 | AW   |
| Puerto Rico                                  | 波多黎各                | PR   |
| Isle of Man                                  | 曼岛                  | IM   |
| Guam                                         | 关岛                  | GU   |
| Sint Maarten                                 | 荷属圣马丁               | SX   |
| Turks and Caicos                             | 特克斯和凯科斯群岛           | TC   |
| Åland Islands                                | 奥兰群岛                | AX   |
| Caribbean Netherlands                        | 荷属加勒比               | BQ   |
| British Indian Ocean Territory               | 英属印度洋领地             | IO   |
| Christmas  as Island                         | 圣诞岛                 | CX   |
| Cocos (Keeling) Islands                      | 科科斯 (基林) 群岛         | CC   |
| Falkland Islands (Islas Malvinas)            | 福克兰群岛 (马尔维纳斯群岛)     | FK   |
| Mayotte                                      | 马约特                 | YT   |
| Niue                                         | 纽埃                  | NU   |
| Norfolk Island                               | 诺福克岛                | NF   |
| Northern Mariana Islands                     | 北马里亚纳群岛             | MP   |
| Pitcairn Islands                             | 皮特凯恩群岛              | PN   |
| Saint Helena, Ascension and Tristan da Cunha | 圣赫勒拿、阿森松岛和特里斯坦-达库尼亚 | SH   |
| Collectivity of Saint Martin                 | 法属圣马丁               | MF   |
| Saint Pierre and Miquelon                    | 圣皮埃尔和密克隆            | PM   |
| Tokelau                                      | 托克劳                 | TK   |
| Wallis and Futuna                            | 瓦利斯和富图纳             | WF   |
| American Samoa                               | 美属萨摩亚               | AS   |
