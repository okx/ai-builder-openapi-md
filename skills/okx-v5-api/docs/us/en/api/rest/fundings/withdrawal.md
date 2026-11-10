---
site: us
lang: en
name: Withdrawal
method: POST
path: /api/v5/asset/withdrawal
module: asset
rest: https://us.okx.com
ws: wss://wsus.okx.com
supported: true
---

### Withdrawal

Only supported withdrawal of assets from funding account. Common sub-account does not support withdrawal. 

<aside class="notice">
The API can only make withdrawal to verified addresses/account, and verified addresses can be set by WEB/APP.
</aside>

<aside class="notice">
About tag<br>
Some token deposits require a deposit address and a tag (e.g. Memo/Payment ID), which is a string that guarantees the uniqueness of your deposit address. Follow the deposit procedure carefully, or you may risk losing your assets.<br>
For currencies with labels, if it is a withdrawal between OKX users, please use internal transfer instead of online withdrawal
</aside>

#### Rate Limit: 6 requests per second
#### Rate limit rule: User ID

#### HTTP Request

`POST /api/v5/asset/withdrawal`

> Request Example

```python
import okx.Funding as Funding

# API initialization
apikey = "YOUR_API_KEY"
secretkey = "YOUR_SECRET_KEY"
passphrase = "YOUR_PASSPHRASE"

flag = "0"  # Production trading: 0, Demo trading: 1

fundingAPI = Funding.FundingAPI(apikey, secretkey, passphrase, False, flag)

# Withdrawal
result = fundingAPI.withdrawal(
    ccy="USDT",
    toAddr="TXtvfb7cdrn6VX9H49mgio8bUxZ3DGfvYF",
    amt="100",
    dest="4",
    chain="USDT-TRC20"
)
print(result)
```

#### Request Parameters

| Parameter                | Type   | Required        | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|:-------------------------| :----- |:----------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ccy                      | String | Yes             | Currency, e.g. `USDT`                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| amt                      | String | Yes             | Withdrawal amount<br>Withdrawal fee is not included in withdrawal amount. Please reserve sufficient transaction fees when withdrawing.<br>You can get fee amount by [Get currencies](/docs-v5/en/#funding-account-rest-api-get-currencies).<br>For `internal transfer`, transaction fee is always `0`.                                                                                                                                                                                      |
| dest                     | String | Yes             | Withdrawal method<br>`3`: internal transfer<br>`4`: on-chain withdrawal                                                                                                                                                                                                                                                                                                                                                                                                                     |
| toAddr                   | String | Yes             | `toAddr` should be a trusted address/account. <br>If your `dest` is `4`, some crypto currency addresses are formatted as `'address:tag'`, e.g. `'ARDOR-7JF3-8F2E-QUWZ-CAN7F:123456'`<br>If your `dest` is `3`,`toAddr` should be a recipient address which can be UID, email, phone or login account name (account name is only for sub-account).                                                                                                                                                |
| toAddrType               | String | No              | Address type<br>`1`: wallet address, email, phone, or login account name<br>`2`: UID (applicable only when dest=`3`)                                                                                                                                                                                                                                                                                                                                            |
| chain                    | String | Conditional     | Chain name<br>There are multiple chains under some currencies, such as `USDT` has `USDT-ERC20`, `USDT-TRC20`<br>If the parameter is not filled in, the default will be the main chain.<br>When you withdrawal the non-tradable asset, if the parameter is not filled in, the default will be the unique withdrawal chain.<br>Apply to `on-chain withdrawal`.<br>You can get supported chain name by the endpoint of [Get currencies](/docs-v5/en/#funding-account-rest-api-get-currencies). |
| areaCode                 | String | Conditional     | Area code for the phone number, e.g. `86`<br>If `toAddr` is a phone number, this parameter is required.<br>Apply to `internal transfer`                                                                                                                                                                                                                                                                                                                                                     |
| rcvrInfo                 | Object | Conditional     | Recipient information<br>For the specific entity users to do on-chain withdrawal/lightning withdrawal, this information is required.                                                                                                                                                                                                                                                                                                                                                        |
| > walletType             | String | Yes             | Wallet Type<br>`exchange`: Withdraw to exchange wallet<br>`private`: Withdraw to private wallet<br>For the wallet belongs to business recipient, `rcvrFirstName` may input the company name, `rcvrLastName` may input "N/A", location info may input the registered address of the company.                                                                                                            |
| > exchId                 | String | Conditional     | Exchange ID<br>You can query supported exchanges through the endpoint of [Get exchange list (public)](/docs-v5/en/#funding-account-rest-api-get-exchange-list-public)<br>If the exchange is not in the exchange list, fill in '0' in this field. <br>Apply to walletType = `exchange`                                                                                                                                                                                                       |
| > rcvrFirstName          | String | Conditional     | Receiver's first name, e.g. `Bruce`                                                                                                                                                                                                                                                                                                                                                                                                                 |
| > rcvrLastName           | String | Conditional     | Receiver's last name, e.g. `Wayne`                                                                                                                                                                                                                                                                                                                                                                                                              |
| > rcvrCountry            | String | Conditional     | The recipient's country, e.g. `United States`<br>You must enter an English country name or a two letter country code (ISO 3166-1). Please refer to the `Country Name` and `Country Code` in the country information table below.                                                                                                                                                                                                              |
| > rcvrCountrySubDivision | String | Conditional     | State/Province of the recipient, e.g. `California`                                                                                                                                                                                                                                                                                                                                                                                      |
| > rcvrTownName           | String | Conditional     | The town/city where the recipient is located, e.g. `San Jose`                                                                                                                                                                                                                                                                                                                                                                                         |
| > rcvrStreetName         | String | Conditional     | Recipient's street address, e.g. `Clementi Avenue 1`                                                                                                                                                                                                                                                                                                                                                                                          |
| clientId                 | String | No              | Client-supplied ID<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.                                                                                                                                                                                                                                                                                                                                                                    |

> Response Example

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

#### Response Parameters

| Parameter | Type   | Description                |
| :----- | :----- | :------------------ |
| ccy    | String | Currency |
| chain  | String | Chain name, e.g. `USDT-ERC20`, `USDT-TRC20` |
| amt    | String | Withdrawal amount |
| wdId   | String | Withdrawal ID |
| clientId  | String | Client-supplied ID<br>A combination of case-sensitive alphanumerics, all numbers, or all letters of up to 32 characters.  |

#### Country information

| Country name                                 | Country code |
|----------------------------------------------|--------------|
| Afghanistan                                  | AF           |
| Albania                                      | AL           |
| Algeria                                      | DZ           |
| Andorra                                      | AD           |
| Angola                                       | AO           |
| Anguilla                                     | AI           |
| Antigua and Barbuda                          | AG           |
| Argentina                                    | AR           |
| Armenia                                      | AM           |
| Australia                                    | AU           |
| Austria                                      | AT           |
| Azerbaijan                                   | AZ           |
| Bahamas                                      | BS           |
| Bahrain                                      | BH           |
| Bangladesh                                   | BD           |
| Barbados                                     | BB           |
| Belarus                                      | BY           |
| Belgium                                      | BE           |
| Belize                                       | BZ           |
| Benin                                        | BJ           |
| Bermuda                                      | BM           |
| Bhutan                                       | BT           |
| Bolivia                                      | BO           |
| Bosnia and Herzegovina                       | BA           |
| Botswana                                     | BW           |
| Brazil                                       | BR           |
| British Virgin Islands                       | VG           |
| Brunei                                       | BN           |
| Bulgaria                                     | BG           |
| Burkina Faso                                 | BF           |
| Burundi                                      | BI           |
| Cambodia                                     | KH           |
| Cameroon                                     | CM           |
| Canada                                       | CA           |
| Cape Verde                                   | CV           |
| Cayman Islands                               | KY           |
| Central African Republic                     | CF           |
| Chad                                         | TD           |
| Chile                                        | CL           |
| Colombia                                     | CO           |
| Comoros                                      | KM           |
| Congo (Republic)                             | CG           |
| Congo (Democratic Republic)                  | CD           |
| Costa Rica                                   | CR           |
| Cote d´Ivoire (Ivory Coast)                  | CI           |
| Croatia                                      | HR           |
| Cuba                                         | CU           |
| Cyprus                                       | CY           |
| Czech Republic                               | CZ           |
| Denmark                                      | DK           |
| Djibouti                                     | DJ           |
| Dominica                                     | DM           |
| Dominican Republic                           | DO           |
| Ecuador                                      | EC           |
| Egypt                                        | EG           |
| El Salvador                                  | SV           |
| Equatorial Guinea                            | GQ           |
| Eritrea                                      | ER           |
| Estonia                                      | EE           |
| Ethiopia                                     | ET           |
| Fiji                                         | FJ           |
| Finland                                      | FI           |
| France                                       | FR           |
| Gabon                                        | GA           |
| Gambia                                       | GM           |
| Georgia                                      | GE           |
| Germany                                      | DE           |
| Ghana                                        | GH           |
| Greece                                       | GR           |
| Grenada                                      | GD           |
| Guatemala                                    | GT           |
| Guinea                                       | GN           |
| Guinea-Bissau                                | GW           |
| Guyana                                       | GY           |
| Haiti                                        | HT           |
| Honduras                                     | HN           |
| Hong Kong                                    | HK           |
| Hungary                                      | HU           |
| Iceland                                      | IS           |
| India                                        | IN           |
| Indonesia                                    | ID           |
| Iran                                         | IR           |
| Iraq                                         | IQ           |
| Ireland                                      | IE           |
| Israel                                       | IL           |
| Italy                                        | IT           |
| Jamaica                                      | JM           |
| Japan                                        | JP           |
| Jordan                                       | JO           |
| Kazakhstan                                   | KZ           |
| Kenya                                        | KE           |
| Kiribati                                     | KI           |
| North Korea                                  | KP           |
| South Korea                                  | KR           |
| Kuwait                                       | KW           |
| Kyrgyzstan                                   | KG           |
| Laos                                         | LA           |
| Latvia                                       | LV           |
| Lebanon                                      | LB           |
| Lesotho                                      | LS           |
| Liberia                                      | LR           |
| Libya                                        | LY           |
| Liechtenstein                                | LI           |
| Lithuania                                    | LT           |
| Luxembourg                                   | LU           |
| Macau                                        | MO           |
| Macedonia                                    | MK           |
| Madagascar                                   | MG           |
| Malawi                                       | MW           |
| Malaysia                                     | MY           |
| Maldives                                     | MV           |
| Mali                                         | ML           |
| Malta                                        | MT           |
| Marshall Islands                             | MH           |
| Mauritania                                   | MR           |
| Mauritius                                    | MU           |
| Mexico                                       | MX           |
| Micronesia                                   | FM           |
| Moldova                                      | MD           |
| Monaco                                       | MC           |
| Mongolia                                     | MN           |
| Montenegro                                   | ME           |
| Morocco                                      | MA           |
| Mozambique                                   | MZ           |
| Myanmar (Burma)                              | MM           |
| Namibia                                      | NA           |
| Nauru                                        | NR           |
| Nepal                                        | NP           |
| Netherlands                                  | NL           |
| New Zealand                                  | NZ           |
| Nicaragua                                    | NI           |
| Niger                                        | NE           |
| Nigeria                                      | NG           |
| Norway                                       | NO           |
| Oman                                         | OM           |
| Pakistan                                     | PK           |
| Palau                                        | PW           |
| Panama                                       | PA           |
| Papua New Guinea                             | PG           |
| Paraguay                                     | PY           |
| Peru                                         | PE           |
| Philippines                                  | PH           |
| Poland                                       | PL           |
| Portugal                                     | PT           |
| Qatar                                        | QA           |
| Romania                                      | RO           |
| Russia                                       | RU           |
| Rwanda                                       | RW           |
| Saint Kitts and Nevis                        | KN           |
| Saint Lucia                                  | LC           |
| Saint Vincent and the Grenadines             | VC           |
| Samoa                                        | WS           |
| San Marino                                   | SM           |
| Sao Tome and Principe                        | ST           |
| Saudi Arabia                                 | SA           |
| Senegal                                      | SN           |
| Serbia                                       | RS           |
| Seychelles                                   | SC           |
| Sierra Leone                                 | SL           |
| Singapore                                    | SG           |
| Slovakia                                     | SK           |
| Slovenia                                     | SI           |
| Solomon Islands                              | SB           |
| Somalia                                      | SO           |
| South Africa                                 | ZA           |
| Spain                                        | ES           |
| Sri Lanka                                    | LK           |
| Sudan                                        | SD           |
| Suriname                                     | SR           |
| Swaziland                                    | SZ           |
| Sweden                                       | SE           |
| Switzerland                                  | CH           |
| Syria                                        | SY           |
| Taiwan                                       | TW           |
| Tajikistan                                   | TJ           |
| Tanzania                                     | TZ           |
| Thailand                                     | TH           |
| Timor-Leste (East Timor)                     | TL           |
| Togo                                         | TG           |
| Tonga                                        | TO           |
| Trinidad and Tobago                          | TT           |
| Tunisia                                      | TN           |
| Turkey                                       | TR           |
| Turkmenistan                                 | TM           |
| Tuvalu                                       | TV           |
| U.S. Virgin Islands                          | VI           |
| Uganda                                       | UG           |
| Ukraine                                      | UA           |
| United Arab Emirates                         | AE           |
| United Kingdom                               | GB           |
| United States                                | US           |
| Uruguay                                      | UY           |
| Uzbekistan                                   | UZ           |
| Vanuatu                                      | VU           |
| Vatican City                                 | VA           |
| Venezuela                                    | VE           |
| Vietnam                                      | VN           |
| Yemen                                        | YE           |
| Zambia                                       | ZM           |
| Zimbabwe                                     | ZW           |
| Kosovo                                       | XK           |
| South Sudan                                  | SS           |
| China                                        | CN           |
| Palestine                                    | PS           |
| Curacao                                      | CW           |
| Dominican Republic                           | DO           |
| Dominican Republic                           | DO           |
| Gibraltar                                    | GI           |
| New Caledonia                                | NC           |
| Cook Islands                                 | CK           |
| Reunion                                      | RE           |
| Guernsey                                     | GG           |
| Guadeloupe                                   | GP           |
| Martinique                                   | MQ           |
| French Polynesia                             | PF           |
| Faroe Islands                                | FO           |
| Greenland                                    | GL           |
| Jersey                                       | JE           |
| Aruba                                        | AW           |
| Puerto Rico                                  | PR           |
| Isle of Man                                  | IM           |
| Guam                                         | GU           |
| Sint Maarten                                 | SX           |
| Turks and Caicos                             | TC           |
| Åland Islands                                | AX           |
| Caribbean Netherlands                        | BQ           |
| British Indian Ocean Territory               | IO           |
| Christmas  as Island                         | CX           |
| Cocos (Keeling) Islands                      | CC           |
| Falkland Islands (Islas Malvinas)            | FK           |
| Mayotte                                      | YT           |
| Niue                                         | NU           |
| Norfolk Island                               | NF           |
| Northern Mariana Islands                     | MP           |
| Pitcairn Islands                             | PN           |
| Saint Helena, Ascension and Tristan da Cunha | SH           |
| Collectivity of Saint Martin                 | MF           |
| Saint Pierre and Miquelon                    | PM           |
| Tokelau                                      | TK           |
| Wallis and Futuna                            | WF           |
| American Samoa                               | AS           |
