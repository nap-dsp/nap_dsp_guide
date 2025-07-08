# NAP DSP integration guide.



This guide for integrating with NAP DSP using OpenRTB 2.5(Native Ads 1.2).  
(Should there be missing fields or requirements, please default requirements to the spec itself.)

https://www.iab.com/wp-content/uploads/2016/03/OpenRTB-API-Specification-Version-2-5-FINAL.pdf<br>
https://www.iab.com/wp-content/uploads/2018/03/OpenRTB-Native-Ads-Specification-Final-1.2.pdf

NAP DSP uses JSON and the HTTP POST method.

If you have any question about this document, please reach out to adpacker@nasmedia.co.kr.  
<br>

**Note**
- Required fields must be included.
- Recommended, Optional fields have positive impact on operations.
- NAP DSP extension fields are ![red](https://user-images.githubusercontent.com/56284426/144810848-2dfd3193-782d-4cfe-be98-42bc36ad878f.png) `red placeholder`.
<br>

# 1. Bid Request Specification
## Object model

Object | Support | OpenRTB 2.5 Section for Reference | Native Ads 1.2 Section for Reference | Extension
:--- | :---: | :---: | :---: | :---
BidRequest | O | 3.2.1 |  | O
Source | O | 3.2.2 |  | X
Regs | O | 3.2.3 |  | X
Imp | O | 3.2.4 |  | X
Metric | X | 3.2.5 |  | X
Banner | O | 3.2.6 |  | X
Video | O | 3.2.7 |  | X
Native | O | 3.2.9 |  | X
Format | X | 3.2.10 |  | X
PMP | O | 3.2.11 |  | X
Deal | O | 3.2.12 |  | X
Site | O | 3.2.13 |  | X
App | O | 3.2.14 |  | X
Publisher | O | 3.2.15 |  | X
Content | O | 3.2.16 |  | X
Producer | X | 3.2.17 |  | X
Device | O | 3.2.18 |  | X
Geo | O | 3.2.19 |  | X
User | O | 3.2.20 |  | X
Data | X | 3.2.21 |  | X
Segment | X | 3.2.22 |  | X
Native Markup | O | - | 4.1 | -
Asset | O | - | 4.2 | -
Title | O | - | 4.3 | -
Image | O | - | 4.4 | -
Video | O | - | 4.5 | -
Data | O | - | 4.6 | -
EventTrackers | O | - | 4.7 | -
<br>

## Object Specifications

- Object : BidRequest

Attribute | Support | Type
:--- | :---: | :---
id | O | string; **required**
imp | O | object array; **required**
site | O | object; **required**(site or app)
app | O | object; **required**(site or app)
device | O | object; **required**
user | O | object; recommended
test | O | interger; optional
at | O | integer; **required**
tmax | O | integer; **required**
wseat | X | string array
bseat | X | string array
allimps | X | integer
cur | O | string array(USD only)
wlang | X | string array
bcat | O | string array; recommended
badv | O | string array; recommended
bapp | O | string array; recommended
source | O | object; optional
regs | O | object; optional
ext | O | object; optional
<br>

- Object : Source

Attribute | Support | Type
:--- | :---: | :---
fd | X | integer
tid | X | string
pchain | X | string
ext | O | object; optional
<br>

- Object : Reg

Attribute | Support | Type
:--- | :---: | :---
coppa | O | string; optional
ext | O | object; optional
<br>

- Object : Imp

Attribute | Support | Type
:--- | :---: | :---
id | O | string; **required**
metric | O | object array; optional
banner | O | object; **required**(for banner imp)
video | O | object; **required**(for video imp)
native | O | object; **required**(for native imp)
pmp | O | object; optional
displaymanager | O | string; recommend(for video/native imp)
displaymanagerver | O | string; recommended(for video/native imp)
instl | O | integer; optional
tagid | O | string; recommended
bidfloor | O | float; recommended
bidfloorcur | O (USD only) | string; **required**
secure | O | integer; optional
iframebuster | X | string array
exp | X | integer
ext | O | object; optional
<br>

- Object : Banner

Attribute | Support | Type
:--- | :---: | :---
w | O | integer; **required**
h | O | integer; **required**
format | O | object array; optional
wmax | X | integer
hmax | X | integer
wmin | X | integer
hmin | X | integer
id | O | string; optional
btype | O | integer array; optional
battr | O | integer array; optional
pos | O | integer; optional
mimes | X | string array
topframe | O | integer; optional
expdir | X | integer array
api | O | integer array; optional
vcm | X | interger
ext | X | object
<br>

- Object : Video

Attribute | Support | Type
:--- | :---: | :---
mimes | O | string array; **required**
minduration | O | integer; **required**
maxduration | O | integer; **required**
protocols | O | integer array; recommended
protocol | X | integer
w | O | integer; recommended
h | O | integer; recommended
startdelay | O | integer; recommended
placement | O | integer; optional
linearity | O | integer; optional
skip | O | integer; optional
skipmin | O | integer; optional
skipafter | O | integer; optional
sequence | O | integer; optional
battr | O | integer array; optional
maxextended | O | integer; optional
minbitrate | O | integer; optional
maxbitrate | O | integer; optional
boxingallowed | O | integer; optional
playbackmethod | O | integer array; optional
playbackend | O | integer; optional
delivery | O | integer array; optional
pos | O | integer; optional
companionad | O | objects array; optional
api | O | integer array; optional
companiontype | O | integer array; optional
ext | O | object; optional
<br>

- Object : Native

Attribute | Support | Type
:--- | :---: | :---
request | O | string; **required**
ver | O | string; recommended
api | O | integer array; optional
battr | O | integer array; optional
ext | O | object; optional
<br>

- Object : Pmp

Attribute | Support | Type
:--- | :---: | :---
private_auction | O | integer; optional
deals | O | objects array; optional
ext | O | object; optional
<br>

- Object : Deal

Attribute | Support | Type
:--- | :---: | :---
id | O | string; optional
bidfloor | O | float; optional
bidfloorcur | O | string; optional
at | O | integer; optional
wseat | O | string array; optional
wadomain | O | string array; optional
ext | O | object; optional
<br>

- Object : Site

Attribute | Support | Type
:--- | :---: | :---
id | O | string; **required**
name | O | string; recommended
domain | O | string; **required**
cat | O | string array; recommended
sectioncat | O | string array; optional
pagecat | O | string array; optional
page | O | string; optional
ref | O | string; optional
search | X | string
mobile | O | integer; optional
privacypolicy | X | integer
publisher | O | object; optional
content | O | object; optional
keywords | O | string; optional
ext | O | object; optional
<br>

- Object : App

Attribute | Support | Type
:--- | :---: | :---
id | O | string; **required**
name | O | string; recommended
bundle | O | string; **required**
domain | O | string; optional
storeurl | O | string; recommended
cat | O | string array; recommended
sectioncat | X | string array
pagecat | X | string array
ver | O | string; optional
privacypolicy | X | integer
paid | X | integer
publisher | O | object; optional
content | O | object; optional
keywords | O | string; optional
ext | O | object; optional
<br>

- Object : Publisher

Attribute | Support | Type
:--- | :---: | :---
id | O | string; **required**
name | O | string; optional
cat | X | string array
domain | X | string
ext | X | object
<br>

- Object : Device

Attribute | Support | Type
:--- | :---: | :---
ua | O | string; **required**
geo | O | object; recommended
lmt | O | integer; recommended
dnt | O | integer; recommended
ip | O | string; **required**
devicetype | O | integer; recommended
make | O | string; optional
model | O | string; optional
os | O | string; **required**
osv | O | string; recommended
hwv | O | string; optional
h | O | integer; optional
w | O | integer; optional
pxratio | O | float; optional
js | O | integer; optional
language | O | string; recommended
carrier | O | string; recommended
connectiontype | O | integer; optional
ifa | O | string; recommended
didsha1 | X | string
didmd5 | X | string
dpidsha1 | X | string
dpidmd5 | X | string
ext | O | object; optional
<br>

- Object : Geo

Attribute | Support | Type
:--- | :---: | :---
lat | O | float; recommended
lon | O | float; recommended
type | O | integer; optional
accuracy | X | integer
lastfix | X | integer
ipservice | X | integer
country | O | string; recommended
region | O | string; recommended
regionfips104 | O | string; Optional
metro | O | string; Optional
city | O | string; Optional
zip | O | string; Optional
utcoffset | X | integer
ext | X | object
<br>

- Object : User

Attribute | Support | Type
:--- | :---: | :---
id | O | string; recommended
buyerid | O | string; recommended
yob | O | integer; optional
gender | O | string; optional
keywords | O | string; optional
customdata | X | string
geo | O | object; optional
data | O | object array; optional
ext | O | object; optional
<br>


- Object : Native Markup

Field name | **required**
:--- | :---
ver | -
context | -
contextsubtype | -
plcmttype | -
plcmtcnt | -
seq | -
assets | O
aurlsupport | -
durlsupport | -
eventtrackers | -
privacy | -
<br>

- Object : Asset

Field name | **required**
:--- | :---
Id | O
required | -
title | -
img | -
video | -
data | -
<br>

- Object : Title

Field name | **required**
:--- | :---
len | O
<br>

- Object : Image

Field name | **required**
:--- | :---
type | -
w | -
wmin | -
h | -
hmin | -
mimes | -
<br>

- Object : Video

Field name | **required**
:--- | :---
mimes | O
minduration | O
maxduration | O
protocols | O
<br>

- Object : Data

Field name | **required**
:--- | :---
type | O
len | -
<br>

- Object : EventTrackers

Field name | **required**
:--- | :---
event | O
methods | O
<br>

## Extentions

- Object : ext of BidRequest

Attribute | Type | Description
:--- | :---: | :---
![red](https://user-images.githubusercontent.com/56284426/144810848-2dfd3193-782d-4cfe-be98-42bc36ad878f.png)contracttype | integer; optional | 0:CPM(RTB), 1:CPC. Default is CPM.
![red](https://user-images.githubusercontent.com/56284426/144810848-2dfd3193-782d-4cfe-be98-42bc36ad878f.png)clicktrack | integer; optional | Where 0 = Click tracking macro `${CLICK_TRACK_URL}` for SSP is not included in BidResponse(Bid.adm), 1 = Click tracking macro for SSP is included in BidResponse(Bid.adm)
![red](https://user-images.githubusercontent.com/56284426/144810848-2dfd3193-782d-4cfe-be98-42bc36ad878f.png)clicktrackmacro | string; optional | If you fill this field, will be included in BidResponse(Bid.adm) instead of `${CLICK_TRACK_URL}`. clicktrack field must be set 1.
<br>

## Banner
	{
		"id": "123-abc-test",
		"tmax": 500,
		"cur": [
			"USD"
		],
		"device": {
			"dnt": 0,
			"lmt": 0,
			"geo": {
				"country": "KOR",
				"type": 2,
				"lat": -6.1741,
				"lon": 106.8296,
				"region": "AP",
				"city": "seoul"
			},
			"ifa": "00000000-5227-4b8e-bdc9-6aa7658ae073",
			"connectiontype": 3,
			"language": "ko",
			"model": "M2003J15SC",
			"carrier": "Telkomsel",
			"os": "android",
			"osv": "10",
			"js": 1,
			"ua": "Mozilla/5.0 (Linux; Android 10; M2003J15SC Build/QP1A.190711.020; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/94.0.4606.85 Mobile Safari/537.36",
			"ip": "123.234.43.96",
			"devicetype": 4
		},
		"ext": {
		},
		"app": {
			"bundle": "com.test.nasmedia",
			"storeurl": "https://play.google.com/store/apps/details?id=com.test.nasmedia",
			"cat": [
				"IAB9",
				"IAB9-30",
				"IAB1"
			],
			"pagecat": [
				"IAB24"
			],
			"publisher": {
				"id": "1234"
			},
			"id": "6e66774b9000",
			"name": "Test_imp"
		},
		"imp": [
			{
				"id": "1",
				"instl": 0,
				"secure": 1,
				"bidfloorcur": "USD",
				"tagid": "12345",
				"banner": {
					"api": [
						5
					],
					"battr": [
						1,
						3,
						5,
						6,
						8,
						9,
						13
					],
					"w": 320,
					"h": 50,
					"format": [
						{
							"w": 320,
							"h": 50
						}
					]
				},
				"bidfloor": 0.135
			}
		],
		"at": 2,
		"user": {
			"id": "0000000033d5f850cc023bee7bb7e70ffb98bc33",
			"ext": {
				"consent": ""
			}
		},
		"regs": {
			"coppa": 0,
			"ext": {
				"gdpr": 0
			}
		}
	}
<br>

## Native
	{
		"id": "test-id-123",
		"tmax": 500,
		"cur": [
			"USD"
		],
		"device": {
			"dnt": 0,
			"geo": {
				"country": "USA",
				"lat": 40.4681,
				"lon": -78.9907,
				"city": "Nashville"
			},
			"ifa": "00000000-0a85-41b7-b09a-f84e176f8d7d",
			"connectiontype": 2,
			"model": "TB-X104F",
			"make": "Lenovo",
			"carrier": "Verizon Internet Services",
			"os": "android",
			"osv": "8.1.0",
			"js": 1,
			"ua": "Mozilla/5.0 (Linux; Android 8.0.0; Lenovo TB-X104F Build/OPM1.171019.026; wv) AppleWebKit/537.36 (KHTML, like Gecko) Version/4.0 Chrome/73.0.3683.90 Safari/537.36",
			"ip": "123.151.140.127",
			"devicetype": 1
		},
		"app": {
			"bundle": "com.test.docu",
			"storeurl": "https://play.google.com/store/apps/details?id=com.test.docu",
			"cat": [
				"IAB1"
			],
			"pagecat": [
				"IAB24"
			],
			"publisher": {
				"id": "5215"
			},
			"id": "9990e23dcbc4",
			"name": "TestDoceApp"
		},
		"at": 1,
		"imp": [
			{
				"id": "1",
				"instl": 0,
				"secure": 0,
				"native": {
					"request": "{\"native\":{\"adunit\":2,\"assets\":[{\"id\":3,\"img\":{\"h\":120,\"hmin\":0,\"type\":3,\"w\":180,\"wmin\":0},\"required\":1},{\"id\":0,\"required\" :1,\"title\":{\"len\":140}},{\"data\":{\"len\":25,\"type\":1},\"id\":4,\"required\":1},{\"data\":{\"len\":140,\"type\":2},\"id\":6,\"required\":1}],\"context\":1,\"layou t\":1,\"contextsubtype\":11,\"plcmtcnt\":1,\"plcmttype\":2,\"ver\":\"1.0\",\"ext\":{\"banner\":{\"w\":300,\"h\":250}}}}"
				},
				"bidfloor": 0.25
			}
		],
		"user": {
			"id": "99999999cc9ebebba4703a3b711d9a5c0822d991ea80d5ced4582e4a629b865f"
		},
		"regs": {
			"ext": {
				"gdpr": 0
			}
		},
		"ext": {
		}
	}
<br>

# 2. Bid Response Specification
## Object model

Object | Support | OpenRTB 2.5 Section for Reference | Extension
:--- | :---: | :---: | :---
BidResponse | O | 4.2.1 | O
SeatBid | O | 4.2.2 | X
Bid | O | 4.2.3 | X
<br>

## Object Specifications

- Object : BidResponse

Attribute | Support | Type
:--- | :---: | :---
id | O | string; **required**
seatbid | O | objects array; optional
bidid | O | string; optional
cur | O (USD only) | string; **required**
customdata | O | string; optional
nbr | O | integer; optional
ext | O | object; optional
<br>

- Object : SeatBid

Attribute | Support | Type
:--- | :---: | :---
bid | O | object array; requried
seat | O | string; recommended
group | X | integer
ext | X | object
<br>

- Object : Bid

Attribute | Support | Type
:--- | :---: | :---
id | O | string; **required**
impid | O | string; **required**
price | O | float; **required**
adid | O | string; recommended
nurl | O | string; optional
burl | O | string; optional
lurl | O | string; optional
adm | O | string; **required**, Format: HTML<br>Win notice URL must be included within HTML.
adomain | O | string array; recommended
iurl | O | string; recommended
cid | O | string; **required**
crid | O | string; **required**
api | O | integer; optional
bundle | O | string; recommended
attr | O | integer array; optional
cat | O | string array; recommended
dealid | O | string; optional
h | O | integer; optional
w | O | integer; optional
ext | X | object
<br>

## Response sample

## Banner
	{ 
		"id": "1234567890", 
		"cur": "USD", 
		"seatbid": [ { 
			"seat": "adpacker", 
			"bid": [ { 
				"id": "adpk_20180101234567890", 
				"impid": "102", 
				"price": 9.43, 
				"w": 320, 
				"h": 50, 
				"iurl": "http://devimg.nasmedia.co.kr/nas_adpacker_img/image/201801/1463542000568533001516323459.jpg", 
				"adomain": [ "adpacker.co.kr" ], 
				"bundle": "com.adpacker",
				"cid": "1006", 
				"crid": "34421", 
				"cat": [ "IAB24" ], 
				"adm": "<script type=\"text\/javascript\">oData = {\"sdk\":2,\"sdk_version\":\"1.1.99\",\"module_version\":0.500,\"goods_cd\":\"IMAGE\",\"size_cd\":\"320X50\",\"log_delay\":0,\"width\":320,\"height\":50,\"ssp_click\":\"${CLICK_TRACK_URL}\",\"wm_use_yn\":\"n\",\"data\":[{\"ads_id\":33695,\"creative\":\"http:\/\/devimg.nasmedia.co.kr\/nas_adpacker_img\/image\/201801\/1463542000568533001516323459.jpg\",\"creative2\":\"\",\"width\":320,\"height\":50,\"cl\":\"http:\/\/adp1.adpacker.co.kr\/ck?adpdata=D1HPunpj1li9CKkVgsfXP07ggT02tkQrSExlm8V6Ysa3lEQpJE4kLUyd16gtl2xyY6aRd0IpmNyraa5qX0uUxAzBfC9LiqjvXd4D2LkYBninxxY7hU2-rvrfKlFj7c7raqTr74y98n0J_Cl19FPr_oXJSrS_Pg_5qUuyhrA1Wcxqq8eblDDoRnsE-S2i6onq2MsCW-dicQVtT5AZMEEgRm0LwZ4ycEAKPrfpX1cXh0-6wzT1URulnAfnYbWYUV18VA1cHPmnr1iOPPPGyuHI7HSAqwOZ25DRfjfwmSJDai47VILsWWxSKm-j0Cc4vXPh09OVjq3Mxv7g1xi8H1Zu1m1PVlxDC_EyTzMKFgKpkfEth0A4n0Zrh9wAbHsDdzqE9Fz-7VlwgGIHaR43wO94lkQPWo2Af70MeywzQsaMLdzB6TdR5iLh5rvIen3HtzHXyXDEgfwvLcdGo0WuXVg02QBa68yE5ofjHB_Il__bkaoJwa-jUuG_88RANGvA7Zv9zCv22Z3_MrnuplDMFu3fdwWq5SkOoxzMCJgd_V4Fuu4FquUpDqMczAiYHf1eBbruBarlKQ6jHMwImB39XgW67gWq5SkOoxzMCJgd_V4Fuu4FquUpDqMczAiYHf1eBbruBarlKQ6jHMwImB39XgW67gWq5SkOoxzMCJgd_V4Fuu4FquUpDqMczAiYHf1eBbruBarlKQ6jHMwImB39XgW67gWq5SkOoxzMCJgd_V4Fuu4FquUpDqMczAiYHf1eBbruBarlKQ6jHMwImB39XgW67gWq5SkOoxzMCJgd_V4Fuu4FquUpDqMczAiYHf1eBbruBarlKQ6jHMwImB39XgW67gWq5SkOoxzMCJgd_V4Fuu4FquUpDqMczAiYHf1eBbruBarlKQ6jHMwImB39XgW67gWq5SkOoxzMCJgd_V4Fuu4FquUpDqMczAiYHf1eBbruBarlKQ6jHMwImB39XgW67gWq5SkOoxzMCJgd_V4Fuu4FquUpDqMczAiYHf1eBbruBarlKQ6jHMwImB39XgW67gWq5SkOoxzMCJgd_V4Fuu4FquUpDqMczAiYHf1eBbruBarlKQ6jHMwImB39XgW67gWq5SkOoxzMCJgd_V4Fuu4FquUpDqMczAiYHf1eBbruBarlKQ6jHMwImB39XgW67gWq5SkOoxzMCJgd_V4Fuu4FquUpDqMczAiYHf1eBbruBarlKQ6jHMwImB39XgW67gWq5SkOoxzMCJgd_V4Fuu4FquUpDqMczAiYHf1eBbruBarlKQ6jHMwImB39XgW67gWq5SkOoxzMCJgd_V4Fuu4FquUpDqMczAiYHf1eBbruBarlKQ6jHMwImB39XgW67l5K3s260bYLKcpV6EYYLPk.\",\"beacon\":\"http:\/\/adp1.adpacker.co.kr\/beacon?adpdata=D1HPunpj1li9CKkVgsfXP07ggT02tkQrSExlm8V6Ysa3lEQpJE4kLUyd16gtl2xyY6aRd0IpmNyraa5qX0uUxAzBfC9LiqjvXd4D2LkYBninxxY7hU2-rvrfKlFj7c7raqTr74y98n0J_Cl19FPr_oXJSrS_Pg_5qUuyhrA1Wcxqq8eblDDoRnsE-S2i6onq2MsCW-dicQVtT5AZMEEgRm0LwZ4ycEAKPrfpX1cXh0-6wzT1URulnAfnYbWYUV18VA1cHPmnr1iOPPPGyuHI7HSAqwOZ25DRfjfwmSJDai47VILsWWxSKm-j0Cc4vXPh09OVjq3Mxv7g1xi8H1Zu1m1PVlxDC_EyTzMKFgKpkfEth0A4n0Zrh9wAbHsDdzqE9Fz-7VlwgGIHaR43wO94loHSE51KlHvVfOYp5ZaUIxzoIRUV2PASzhh9zVdJJaufiIJHkEiArT4mLYYNNkWsCpT2yb5TrqB5JlVA0hHEPTaWcaZJo40gq8K0qwfHGSgBtryKn9EZZsIzLTPdNgGuR2ga11GrY8yAR9HoROZ0mIkFquUpDqMczAiYHf1eBbruBarlKQ6jHMwImB39XgW67gWq5SkOoxzMCJgd_V4Fuu4FquUpDqMczAiYHf1eBbruBarlKQ6jHMwImB39XgW67gWq5SkOoxzMCJgd_V4Fuu4FquUpDqMczAiYHf1eBbruBarlKQ6jHMwImB39XgW67gWq5SkOoxzMCJgd_V4Fuu4FquUpDqMczAiYHf1eBbruBarlKQ6jHMwImB39XgW67gWq5SkOoxzMCJgd_V4Fuu4FquUpDqMczAiYHf1eBbruBarlKQ6jHMwImB39XgW67gWq5SkOoxzMCJgd_V4Fuu4FquUpDqMczAiYHf1eBbruBarlKQ6jHMwImB39XgW67gWq5SkOoxzMCJgd_V4Fuu4FquUpDqMczAiYHf1eBbruBarlKQ6jHMwImB39XgW67gWq5SkOoxzMCJgd_V4Fuu4FquUpDqMczAiYHf1eBbruBarlKQ6jHMwImB39XgW67gWq5SkOoxzMCJgd_V4Fuu4FquUpDqMczAiYHf1eBbruBarlKQ6jHMwImB39XgW67gWq5SkOoxzMCJgd_V4Fuu4FquUpDqMczAiYHf1eBbruBarlKQ6jHMwImB39XgW67gWq5SkOoxzMCJgd_V4Fuu4FquUpDqMczAiYHf1eBbruBarlKQ6jHMwImB39XgW67gWq5SkOoxzMCJgd_V4Fuu4FquUpDqMczAiYHf1eBbruBarlKQ6jHMwImB39XgW67gWq5SkOoxzMCJgd_V4Fuu4FquUpDqMczAiYHf1eBbruBarlKQ6jHMwImB39XgW67l5K3s260bYLKcpV6EYYLPk.&bpri=${AUCTION_PRICE} \"}]};<\/script><script type=\"text\/javascript\" src=\"https:\/\/cdnet.nasmob.com\/adpacker\/js\/ap_image_v1.3.js?v=3\"><\/script>" 
			} ] 
		} ] 
	}
<br>

## Native
	{
		"id": "0m1hqt5x",
		"bidid": "abc1123",
		"cur": "USD",
		"seatbid": [{
			"seat": "512",
			"bid": [{
				"id": "adxb_20151203171854390422024850",
				"impid": "1",
				"price": 1.2,
				"adomain": ["aaa.com"],
				"bundle": "com.test",
				"iurl": "http://test.com/image/aaa.jpg",
				"cid": "100",
				"crid": "1234",
				"adm": "{\"native\":{\"ver\":\"1.2\",\"link\":{ ... }, \"imptrackers\":[ ... ],\"assets\":[ ... ]}}"
			}]
		}]
	}
<br>

# 3. Substitution Macros

## Notifications
Macro | Description
:---: | :---
`${AUCTION_PRICE}` | Settlement price using the same currency and units as the bid. BidResponse(Bid.adm) includes a macro that must be replaced by the partner with the price it will be charged.
![red](https://user-images.githubusercontent.com/56284426/144810848-2dfd3193-782d-4cfe-be98-42bc36ad878f.png)`${CLICK_TRACK_URL}` | where BidRequest(ext.clicktrack) set 1 = macro is included in BidResponse(Bid.adm), 0 = macro is not included. If the partner replace this macro with the URL, Adpacker can call that.
<br>


