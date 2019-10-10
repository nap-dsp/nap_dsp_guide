# Adpacker integration guide.



This guide for integrating with Adpacker using OpenRTB 2.3.  
(Should there be missing fields or requirements, please default requirements to the spec itself.)

Adpacker uses JSON and the HTTP POST method.

If you have any question about this document, please reach out to adpacker@nasmedia.co.kr.  
<br>

**Note**
- Required fields must be included.
- Optional fields have positive impact on operations.
- Adpacker extension fields are ![#f03c15](https://placehold.it/15/f03c15/000000?text=+) `red placeholder`.
<br>

# 1. Bid Request Specification
## Object model

Object | Support | Extension field | Description
:--- | :---: | :---: | :---
BidRequest | O | O | Top-level object.
Imp | O | X | Container for the description of a specific impression; at least 1 per request.
Banner | O | X | Details for a banner impression (incl. in-banner video) or video companion ad.
Video | X | X | Details for a video impression or the video asset of a native impression.
Native | X | X | Container for a native impression conforming to the Native Ad Spec.
Site | O | X | Details of the website calling for the impression.
App | O | X | Details of the application calling for the impression.
Publisher | X | X | Entity that controls the content of and distributes the site or app.
Content | X | X | Details about the published content itself, within which the ad will be shown.
Producer | X | X | Producer of the content; not necessarily the publisher (e.g., syndication).
Device | O | X | Details of the device on which the content and impressions are displayed.
Geo | X | X | Location of the device or user’s home base depending on the parent object.
User | O | X | Human user of the device; audience for advertising.
Data | X | X | Collection of additional user targeting data from a specific data source.
Segment | X | X | Specific data point about a user from a specific data source.
Regs | X | X | Regulatory conditions in effect for all impressions in this bid request.
PMP | X | X | Collection of private marketplace (PMP) deals applicable to this impression.
Deal | X | X | Deal terms pertaining to this impression between a seller and buyer.
<br>

## Object Specifications

- Object : BidRequest

Attribute | Support | Type | Description
:--- | :---: | :---: | :---
id | O | string; required | Unique ID of the bid request, provided by the exchange.
imp | O | object array; required | Array of `Imp objects` representing the impressions offered. At least 1 Imp object is required.
site | O | object; Required(site or app) | Details via a `Site object` about the publisher’s website. Only applicable and recommended for websites.
app | O | object; Required(site or app) | Details via an `App object` about the publisher’s app (i.e., non-browser applications). Only applicable and recommended for apps.
device | O | object; Required | Details via a `Device object` about the user’s device to which the impression will be delivered.
user | O | object; Optional | Details via a 'User object' about the human user of the device; the advertising audience.
test | X | integer | Indicator of test mode in which auctions are not billable, where 0 = live mode, 1 = test mode.
at | X | integer | Auction type, where 1 = First Price, 2 = Second Price Plus. Exchange-specific auction types can be defined using values greater than 500. 
tmax | X | integer| Maximum time in milliseconds to submit a bid to avoid timeout. This value is commonly communicated offline.
wseat | X | string array | Whitelist of buyer seats allowed to bid on this impression. Seat IDs must be communicated between bidders and the exchange a priori. Omission implies no seat restrictions.
allimps | X | integer | Flag to indicate if Exchange can verify that the impressions offered represent all of the impressions available in context (e.g., all on the web page, all video spots such as pre/mid/post roll) to support road-blocking. 0 = no or unknown, 1 = yes, the impressions offered represent all that are available.
cur | X | string array | Array of allowed currencies for bids on this bid request using ISO-4217 alpha codes. Recommended only if the exchange accepts multiple currencies.
bcat | O | string array; Optional | Blocked advertiser categories using the IAB content categories.
badv | X | string array | Block list of advertisers by their domains (e.g., “ford.com”).
regs | X | object | A Regs object that specifies any industry, legal, or governmental regulations in force for this request.
ext | O | object; Optional | Placeholder for exchange-specific extensions to OpenRTB.
<br>

- Object : ext of BidRequest

Attribute | Type | Description
:--- | :---: | :---
![#f03c15](https://placehold.it/15/f03c15/000000?text=+)contracttype | integer; optional | 0:CPM(RTB), 1:CPC. Default is CPM.
![#f03c15](https://placehold.it/15/f03c15/000000?text=+)clicktrack | integer; optional | Where 0 = Click tracking macro `${CLICK_TRACK_URL}` for SSP is not included in BidResponse(Bid.adm), 1 = Click tracking macro for SSP is included in BidResponse(Bid.adm)
![#f03c15](https://placehold.it/15/f03c15/000000?text=+)clicktrackmacro | string; optional | If you fill this field, will be included in BidResponse(Bid.adm) instead of `${CLICK_TRACK_URL}`. clicktrack field must be set 1.
<br>

- Object : Imp

Attribute | Support | Type | Description
:--- | :---: | :---: | :---
id | O | string; required | A unique identifier for this impression within the context of the bid request (typically, starts with 1 and increments.
banner | O | object; required | A `Banner object` required if this impression is offered as a banner ad opportunity.
video | X | object | A Video object required if this impression is offered as a video ad opportunity.
native | X | object | A Native object required if this impression is offered as a native ad opportunity.
displaymanager | X | string | Name of ad mediation partner, SDK technology, or player responsible for rendering ad (typically video or mobile). Used by some ad servers to customize ad code by partner. Recommended for video and/or apps.
displaymanagerver | X | string | Version of ad mediation partner, SDK technology, or player responsible for rendering ad (typically video or mobile). Used by some ad servers to customize ad code by partner. Recommended for video and/or apps.
instl | X | integer | 1 = the ad is interstitial or full screen, 0 = not interstitial.
tagid | X | string | Identifier for specific ad placement or ad tag that was used to initiate the auction. This can be useful for debugging of any issues, or for optimization by the buyer.
bidfloor | O | float; required | Minimum bid for this impression expressed in CPM.
bidfloorcur | O (USD/KRW only) | string; required | Currency specified using ISO-4217 alpha codes. This may be different from bid currency returned by bidder if this is allowed by the exchange.
secure | O | integer; optional | Flag to indicate if the impression requires secure HTTPS URL creative assets and markup, where 0 = non-secure, 1 = secure. If omitted, the secure state is unknown, but non-secure HTTP support can be assumed.
iframebuster | X | string array | Array of exchange-specific names of supported iframe busters.
pmp | X | object | A Pmp object containing any private marketplace deals in effect for this impression.
ext | X | object | Placeholder for exchange-specific extensions to OpenRTB.
<br>

- Object : Banner

Attribute | Support | Type | Description
:--- | :---: | :---: | :---
w | O | integer; required | Width of the impression in pixels. If neither wmin nor wmax are specified, this value is an exact width requirement. Otherwise it is a preferred width.
h | O | integer; required | Height of the impression in pixels. If neither hmin nor hmax are specified, this value is an exact height requirement. Otherwise it is a preferred height.
wmax | X | integer | Maximum width of the impression in pixels. If included along with a w value then w should be interpreted as a recommended or preferred width.
hmax | X | integer | Maximum height of the impression in pixels. If included along with an h value then h should be interpreted as a recommended or preferred height.
wmin | X | integer | Minimum width of the impression in pixels. If included along with a w value then w should be interpreted as a recommended or preferred width.
hmin | X | integer | Minimum height of the impression in pixels. If included along with an h value then h should be interpreted as a recommended or preferred height.
id | X | string | Unique identifier for this banner object. Recommended when Banner objects are used with a Video object to represent an array of companion ads. Values usually start at 1 and increase with each object; should be unique within an impression.
btype | X | integer array | Blocked banner ad types.
battr | X | integer array | Blocked creative attributes.
pos | X | integer | Ad position on screen.
mimes | X | string array | Content MIME types supported. Popular MIME types may include “application/x-shockwave-flash”, “image/jpg”, and “image/gif”.
topframe | X | integer | Indicates if the banner is in the top frame as opposed to an iframe, where 0 = no, 1 = yes.
expdir | X | integer array | Directions in which the banner may expand.
api | X | integer array | List of supported API frameworks for this impression. Refer to List 5.6. If an API is not explicitly listed, it is assumed not to be supported.
ext | X | object | Placeholder for exchange-specific extensions to OpenRTB.
<br>

- Object : Site

Attribute | Support | Type | Description
:--- | :---: | :---: | :---
id | O | string; required | Exchange-specific site ID.
name | O | string; required | Site name (may be aliased at the publisher’s request).
domain | O | string; required | Domain of the site (e.g., “mysite.foo.com”).
cat | O | string array; required | Array of IAB content categories of the site.
sectioncat | X | string array | Array of IAB content categories that describe the current section of the site.
pagecat | X | string array | Array of IAB content categories that describe the current page or view of the site.
page | X | string | URL of the page where the impression will be shown.
ref | X | string | Referrer URL that caused navigation to the current page.
search | X | string | Search string that caused navigation to the current page.
mobile | X | integer | Mobile-optimized signal, where 0 = no, 1 = yes.
privacypolicy | X | integer | Indicates if the site has a privacy policy, where 0 = no, 1 = yes.
publisher | X | object | Details about the Publisher of the site.
content | X | object | Details about the Content within the site.
keywords | X | string | Comma separated list of keywords about the site.
ext | X | object | Placeholder for exchange-specific extensions to OpenRTB.
<br>

- Object : App

Attribute | Support | Type | Description
:--- | :---: | :---: | :---
id | O | string; required | Exchange-specific app ID.
name | O | string; required | App name (may be aliased at the publisher’s request).
bundle | O | string; required | Application bundle or package name (e.g., com.foo.mygame); intended to be a unique ID across exchanges.
domain | O | string; optional | Domain of the app (e.g., “mygame.foo.com”).
storeurl | O | string; optional | App store URL for an installed app; for QAG 1.5 compliance.
cat | O | string array; required | Array of IAB content categories of the app.
sectioncat | X | string array | Array of IAB content categories that describe the current section of the app.
pagecat | X | string array | Array of IAB content categories that describe the current page or view of the app.
ver | X | string | Application version.
privacypolicy | X | integer | Indicates if the app has a privacy policy, where 0 = no, 1 = yes.
paid | X | integer | 0 = app is free, 1 = the app is a paid version.
publisher | X | object | Details about the Publisher of the app.
content | X | object | Details about the Content within the app.
keywords | X | string | Comma separated list of keywords about the app.
ext | X | object | Placeholder for exchange-specific extensions to OpenRTB.
<br>

- Object : Device

Attribute | Support | Type | Description
:--- | :---: | :---: | :---
ua | O | string; optional | Browser user agent string.
geo | X | object | Location of the device assumed to be the user’s current location defined by a Geo object
dnt | O | integer; optional | Standard “Do Not Track” flag as set in the header by the browser, where 0 = tracking is unrestricted, 1 = do not track.
lmt | X | integer | “Limit Ad Tracking” signal commercially endorsed (e.g., iOS, Android), where 0 = tracking is unrestricted, 1 = tracking must be limited per commercial guidelines.
ip | O | string; required | IPv4 address closest to device.
ipv6 | X | string | IP address closest to device as IPv6.
devicetype | O | integer; optional | The general type of device.
make | X | string | Device make (e.g., “Apple”).
model | O | string; required | Device model (e.g., “iPhone”).
os | O | string; required | Device operating system (e.g., “iOS”).
osv | O | string; required | Device operating system version (e.g., “3.1.2”).
hwv | X | string | Hardware version of the device (e.g., “5S” for iPhone 5S).
h | X | integer | Physical height of the screen in pixels.
w | X | integer | Physical width of the screen in pixels.
ppi | X | integer | Screen size as pixels per linear inch.
pxratio | X | float | The ratio of physical pixels to device independent pixels.
js | X | integer | Support for JavaScript, where 0 = no, 1 = yes.
flashver | X | string | Version of Flash supported by the browser.
language | O | string; optional | Browser language using ISO-639-1-alpha-2.
carrier | O | string; required | Carrier or ISP (e.g., “VERIZON”). “WIFI” is often used in mobile to indicate high bandwidth (e.g., video friendly vs. cellular).
connectiontype | O | integer; optional | Network connection type.
ifa | O | string; required | ID sanctioned for advertiser use in the clear (i.e., not hashed).
didsha1 | X | string | Hardware device ID (e.g., IMEI); hashed via SHA1.
didmd5 | X | string | Hardware device ID (e.g., IMEI); hashed via MD5.
dpidsha1 | X | string | Platform device ID (e.g., Android ID); hashed via SHA1.
dpidmd5 | X | string | Platform device ID (e.g., Android ID); hashed via MD5.
macsha1 | X | string | MAC address of the device; hashed via SHA1.
macmd5 | X | string | MAC address of the device; hashed via MD5.
ext | X | object | Placeholder for exchange-specific extensions to OpenRTB.
<br>

- Object : User

Attribute | Support | Type | Description
:--- | :---: | :---: | :---
id | O | string; optional | Exchange-specific ID for the user. At least one of id or buyerid is recommended.
buyerid | O | string; optional | Buyer-specific ID for the user as mapped by the exchange for the buyer. At least one of buyerid or id is recommended.
yob | O | integer; optional | Year of birth as a 4-digit integer.
gender | O | string; optional | Gender, where “M” = male, “F” = female, “O” = known to be other (i.e., omitted is unknown).
keywords | X | string | Comma separated list of keywords, interests, or intent.
customdata | X | string | Optional feature to pass bidder data that was set in the exchange’s cookie. The string must be in base85 cookie safe characters and be in any format. Proper JSON encoding must be used to include “escaped” quotation marks.
geo | X | object | Location of the user’s home base defined by a Geo object. This is not necessarily their current location.
data | X | object array | Additional user data. Each Data object represents a different data source.
ext | X | object | Placeholder for exchange-specific extensions to OpenRTB.
<br>

## Request sample
	{
		"id": "80ce30c53c16e6ede735f123ef6e32361bfc7b22",
		"bcat": [ "IAB25", "IAB7-39", "IAB8-18", "IAB8-5", "IAB9-9" ],
		"imp": [ { 
			"id": "1", 
			"bidfloor": 0.03, 
			"bidfloorcur": "USD",
			"banner": { "h": 250, "w": 300 } 
		} ], 
		"site": { 
			"id": "102855", 
			"cat": [ "IAB3-1" ], 
			"domain": "www.foobar.com", 
			"name": "foobar" 
		}, 
		"device": { 
			"ua": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_6_8) AppleWebKit/537.13 (KHTML, like Gecko) Version/5.1.7 Safari/534.57.2", 
			"ip": "123.145.167.10" 
		}, 
		"user": { 
			"id": "55816b39711f9b5acf3b90e313ed29e51665623f" 
		}, 
		"ext" : { 
			"contracttype": 1,
			"clicktrack": 1
		} 
	}
<br>

# 2. Bid Response Specification
## Object model

Object | Support | Extension field | Description
:--- | :---: | :---: | :---
BidResponse | O | X | Top-level object.
SeatBid | O | X | Collection of bids made by the bidder on behalf of a specific seat.
Bid | O | X | An offer to buy a specific impression under certain business terms.
<br>

## Object Specifications

- Object : BidResponse

Attribute | Support | Type | Description
:--- | :---: | :---: | :---
id | O | string | ID of the bid request to which this is a response.
seatbid | O | object array | Array of seatbid objects; 1+ required if a bid is to be made.
bidid | X | string | Bidder generated response ID to assist with logging/tracking.
cur | O (USD/KRW only) | string | Bid currency using ISO-4217 alpha codes.
customdata | X | string | Optional feature to allow a bidder to set data in the exchange’s cookie. The string must be in base85 cookie safe characters and be in any format. Proper JSON encoding must be used to include “escaped” quotation marks.
nbr | X | integer | Reason for not bidding.
ext | X | object | Placeholder for bidder-specific extensions to OpenRTB.
<br>

- Object : SeatBid

Attribute | Support | Type | Description
:--- | :---: | :---: | :---
bid | O | object array | Array of 1+ Bid objects each related to an impression. Multiple bids can relate to the same impression.
seat | O | string | ID of the bidder seat on whose behalf this bid is made.
group | X | integer | 0 = impressions can be won individually; 1 = impressions must be won or lost as a group.
ext | X | object | Placeholder for bidder-specific extensions to OpenRTB.
<br>

- Object : Bid

Attribute | Support | Type | Description
:--- | :---: | :---: | :---
id | O | string | Bidder generated bid ID to assist with logging/tracking.
impid | O | string | ID of the Imp object in the related bid request.
price | O | float | Bid price expressed as CPM although the actual transaction is for a unit impression only. Note that while the type indicates float, integer math is highly recommended when handling currencies (e.g., BigDecimal in Java).
adid | X | string | ID of a preloaded ad to be served if the bid wins.
nurl | X | string | Win notice URL called by the exchange if the bid wins; optional means of serving ad markup.
adm | O (HTML) | string | Optional means of conveying ad markup in case the bid wins; supersedes the win notice if markup is included in both.
adomain | O | string array | Advertiser domain for block list checking (e.g., “ford.com”). This can be an array of for the case of rotating creatives. Exchanges can mandate that only one domain is allowed.
bundle | O | string | Bundle or package name (e.g., com.foo.mygame) of the app being advertised, if applicable; intended to be a unique ID across exchanges.
iurl | O | string | URL without cache-busting to an image that is representative of the content of the campaign for ad quality/safety checking.
cid | O | string | Campaign ID to assist with ad quality checking; the collection of creatives for which iurl should be representative.
crid | O | string | Creative ID to assist with ad quality checking.
cat | O | string array | IAB content categories of the creative.
attr | X | integer array | Set of attributes describing the creative.
dealid | X | string | Reference to the deal.id from the bid request if this bid pertains to a private marketplace direct deal.
h | O | integer | Height of the creative in pixels.
w | O | integer | Width of the creative in pixels.
ext | X | object | Placeholder for bidder-specific extensions to OpenRTB.
<br>

## Response sample
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

# 3. Substitution Macros

## Notifications
Macro | Description
:---: | :---
`${AUCTION_PRICE}` | Settlement price using the same currency and units as the bid. BidResponse(Bid.adm) includes a macro that must be replaced by the partner with the price it will be charged.
![#f03c15](https://placehold.it/15/f03c15/000000?text=+)`${CLICK_TRACK_URL}` | where BidRequest(ext.clicktrack) set 1 = macro is included in BidResponse(Bid.adm), 0 = macro is not included. If the partner replace this macro with the URL, Adpacker can call that.
<br>


