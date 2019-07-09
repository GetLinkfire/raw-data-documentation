# Linkfire Raw Data Documentation

1. [Data fields](#data-fields)
2. [How to match the main metrics in Linkfire v1](#how-to-match-the-main-metrics-in-linkfire-v1)
3. [How to match the main metrics in Linkfire app](#how-to-match-the-main-metrics-in-linkfire-app)

## Data fields
|Field Name|Description|Example|
|---|---|---|
|activity|Activity ID (string) added by the marketer to the link. Max 50 characters.|eveningpromotion, giveaway|
|artists|The name of the artist associated with the link.|Music Band|
|asset|The type of asset where the activity took place.|landingpage, bridgepage, widget|
|boardid|UUID of the board the visited link is in.|a1c50793-2c2a-4809-ae23-3204a4584cad|
|boardname|Name of the board where the link is placed.|music-label|
|browser|Name of visitor's browser as identified through the use of Browscap decoded from the user agent.|Chrome, IE, Facebook App, Safari|
|campaigntype|The type of link used.|Music, Content|
|channelid|UUID of the marketing channel used by the marketer.|d8b8bb4f-4c47-11e6-9fd0-066c3e7a8751|
|channelname|The name of the marketing channel used by the marketer when promoting the link.|Instagram, YouTube Description, Facebook|
|channeltype|The category of the marketing channel used by the marketer when promoting the link.|Owned, Paid, Original|
|city|Name of visitor's city as decoded from IP using Maxmind GeoIP.|Copenhagen|
|convcategory|Category of the conversion.|Music, Movies, Tickets|
|convdescription|Optional additional description about the conversion, for example Ticketmaster show venue.|Royal Arena|
|convid|The product ID as defined by the merchant where the conversion took place.|Defined by merchant|
|convidsecond|Optional additional ID used for identification, such as user ID.|Defined by merchant|
|convname|Name of the product, such as the track, album or ticket in the transaction.|Excellent Musical Music Track|
|convreference|A reference to the specific transaction provided by the merchant.|Defined by merchant|
|convsource|Name of the merchant/service where the transaction took place.|itunes, amazon, applemusic|
|convtoken|The affiliate token (if any) to which the transaction was attributed.|1001l9mr|
|convtype|The type of the product in the transaction.|Song, Video, Playlist|
|convvalue|A value assigned to the transaction, such as purchase value.|Defined by merchant|
|convvaluesecond|Optional additional value such as the commission value of a purchase.|Defined by merchant|
|convvaluetype|Identifies the format of the value.|currency|
|convvalueunit|The unit of the value.|USD, GBP, seconds|
|countrycode|ISO 3166-1 alpha-2 country code of the visitor.|DK|
|created|UTC timestamp for the action in ISO8601 format.|2019-01-07 23:59:45.084|
|device|The type of device used by the visitor.|mobile, desktop, tablet|
|eventaction|The type of action taken by the visitor.|manual, d2s, original, single|
|eventcategory|The context in which the visitor's action happened.|service, widget, player|
|eventcategorytype|The type of the item on which the visitor performed an action.|play, goto, download|
|eventlabel|The name of the item on which the visitor performed an action.|applemusic, spotify, deezer|
|eventvalue|N/A|N/A|
|ip|The visitor's IP address. Deprecated due to IP being PII.|N/A|
|linkid|UUID of the visited link.|13591d85-bd99-469a-812d-4088d6320a91|
|linkisrc|For links associated with a track, the ISRC of the track.|USBSR0930647|
|linkupc|For links associated with an album, the UPC of the album.|884483917087|
|linkurl|The URL of the link.|artist.lnk.to/album|
|newsession|Indicates whether this pageview is the first pageview of the session (matching the actual visitortoken).|true, false|
|os|The visitor's operating system.|iOS, Android, Windows 10, Windows 7|
|params|URL parameters included in the full URL of the visit, as json.|{'fbclid': 'IwAR0S5crLkOb1zCZ6ImMmj8Zs'}|
|referrer|The full URL of the HTTP referrer.|http://m.facebook.com|
|tags|The tags added to the link(s)|1960s, Live, Pop|
|toplevelreferrer|Root domain of the HTTP referrer.|youtube.com, facebook.com|
|type|The action taken by the visitor.|pageview, event, conversion|
|useragent|The visitor's full useragent. Deprecated because it may represent PII and most use cases can be solved with other fields.|N/A|
|visitortoken|Unique visitor token to identify and correlate visitor actions. Has a 10 minute lifetime.|70289507v161c9c3c759e2e534859b73|
|visitoruid|Unique visitor token to identify and correlate visitor actions. Based on a cookie with 6 months lifespan. Note: this is PII.|5b303fgcda39e8.46766248|

## How to match the main metrics in Linkfire v1
|Metric|Description|Calculation approach|
|---|---|---|
|Visits|Non-unique pageviews excluding scrapers, bots etc. Repeated hits are counted.|Count of rows where type='pageview'|
|Visitors|Activity in a 10-minute window in which which repeated pageviews are counted as unique.|Count of rows where type='pageview' and newsession='true'|
|Engagements|Total non-unique engagements, including repeated engagements in the same session|Number of distinct values of ‘Visitortoken’ where type='event'|
|Click-throughs|Visitors that clicked-through at least once|Number of distinct values of ‘Visitortoken’ where type='event' and eventcategory='service'|
|Click-through rate|Click-through rate excludes content links. Percentage of click-throughs against the number of visitors.|Click-throughs / visitors (excluding visitors to content links) * 100|

## How to match the main metrics in Linkfire app
|Metric|Description|Calculation approach|
|---|---|---|
|Visits|Non-unique pageviews excluding scrapers, bots etc. Repeated hits are counted.|Count of rows where type='pageview'|
|Sessions|Activity in a 10-minute window in which repeated pageviews are counted as unique.|Count of rows where type='pageview' and newsession='true'|
|Engagements|Total non-unique engagements, including repeated engagements.|Count of rows where type='event'|
|Click-throughs|Total non-unique click-throughs, including repeated click-throughs.|Count of rows where type='event' and eventcategory='service'|
|Click-through rate|Percentage of click-throughs against the number of visitors. Click-through rate includes content links.|Click-throughs / visits * 100|
|Conversions|Tracked sales and streams from sources that provide attribution data to Linkfire|Count of rows where type='conversion' and convtoken !='1l3vpUI'|
|Streams|Tracked streaming events from streaming services that provide attribution data to Linkfire|Count of rows where type='conversion' and convcategory='stream'|
