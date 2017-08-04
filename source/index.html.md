---
title: Mobbty API Reference

language_tabs:
  - shell

includes:
  - errors

search: true
---

# Introduction

The Mobbty API is organized around [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer). It has predictable, resource-oriented URLs, and uses HTTP response codes to indicate API errors. We use built-in HTTP features, like HTTP authentication and HTTP verbs, which are understood by off-the-shelf HTTP clients. [JSON](http://www.json.org/) is returned by all API responses, including errors.

**Never expose your secret API key in any public website's client-side code.**

# Authentication

> Example request:

```shell
# With shell, you can just pass the correct header with each request
curl "http://api.mobbty.com/feed?key=your_personal_key"
```

Authenticate your account when using the API by including your personal API key in the request. To obtain a key please contact your Mobbty account manager. Do not share your secret API keys in publicly accessible areas such GitHub, client-side code, and so forth.

For all endpoint except the offer feed, authentication to the API is performed via Bearer token. To obtain your Bearer token make a POST first to the login endpoint.

# Offer feed

## Get the Mobbty offer feed

>Example request:

```shell
curl "http://api.mobbty.com/feed?key=your_personal_key"
```

>Example response:

```json
{
  "offers": [
    {
      "offerid": "59089a4c60c511c1fa6812a1",
      "countries": [
        "US"
      ],
      "os": "ios",
      "payout": 2.80,
      "name": "DealDash - Bid to Shop & Save on Auction Games",
      "icon": "http://is1.mzstatic.com/image/thumb/Purple111/v4/6f/18/d0/6f18d097-e019-71c0-5142-4efe6b5a367e/source/60x60bb.jpg",
      "previewURL": "https://itunes.apple.com/us/app/dealdash-bid-to-shop-save/id965782383?mt=8&uo=4",
      "bundleID": 965782383,
      "click_url": "http://click.mobbty.com/ctrack?pubid=1234&offerid=59089a4c60c511c1fa6812a1"
    }
  ],
  "hasmore": true,
  "next_url": "http://api.mobbty.com/feed?skip=1&limit=1",
  "result": "OK",
  "count": 1569
}
```

This endpoint retrieves all offers that you are eligible to run as a publisher.

### HTTP Request

`GET http://api.mobbty.com/feed`

### Query parameters

Parameter | Default | Description
--------- | ------- | -----------
key | required | you personal api token
min_payout | 0 | Only return offers that have a greater or equal payout than the given value.
max_payout | infinite | If set, the result will include offers that have less or equal payout than the given value.
countries | all | Only return offers available in the given value. The value should be comma separated [ISO 3166-1 alpha 2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) country codes.
os | android,ios | Filter the response based on given os value.

<aside class="warning">
Although all offers that are part of the output are valid, be sure to <strong>at least</strong> check this endpoint every 10 minutes to avoid sending traffic to expired offers.
</aside>

# Conversions

## Trigger a new conversion

>Example request:

```shell
curl "http://api.mobbty.com/conversion/confirm" \
  -u "Bearer: your_personal_token" \
  -d click_id=mbbty_590c271cb2687
```

>Example response:

```json
{
  "result": "OK",
  "partner": "conversion registered!"
}
```
This endpoint allows you to register conversion events for the traffic that you received from the Mobbty eco-system. 

Parameter | Mandatory | Description
--------- | ------- | -----------
click_id | yes | Use this parameter to provide the click id that triggered the conversion event.

**In case you are not able to provide the authentication upon calling the endpoint, contact your Mobbty account manager. On request we can bypass this flag and use ip-whitelisting.**

<aside class="notice">
Clients that use the Mobbty Adfraud protection module automatically receive the audit status in the response of this endpoint.
</aside>

<aside class="warning">
Requests without <strong>click_id</strong> will be responded to with error code 400.
</aside>

## Audit conversion

>Example request:

```shell
curl "http://api.mobbty.com/conversion/audit" \
  -u "Bearer: your_personal_token" \
  -d ip=87.11.20.192 \
  -d user_agent=Mozilla%2F5.0%20(Linux%3B%20U%3B%20Android%202.2.1%3B%20en-us%3B%20ADR6400L%204G%20Build%2FFRG83D)%20AppleWebKit%2F533.1%20(KHTML%2C%20like%20Gecko)%20Version%2F4.0%20Mobile%20Safari%2F533.1%20air.com.speakingmax \
  -d device_id=f07a13984f6d116a \
  -d referrer_url=https%3A%2F%2Fapkpure.com%2Fsportsbet%2Fcom.sportsbet.guide%2Fdownload%3Ffrom%3Ddetails \
  -d source_id=1323 \
  -d click_time=2017-02-20T03:22:21 \
  -d conversion_time=2017-02-20T03:28:52 \
  -d bundle_id=com.sportsbet.guide \
  -d offer_id=101bkj1l \
  -d allowed_country[]=IT
  
```

>Example response:

```json
{
  "audit":
    {
      "country": "IT",
      "fraud_score": 0.12,
      "risk": "low",
      "motivation": {
        "duplicate_ip": false,
        "duplicate_device_id": false,
        "duplicate_fingerprint": false,
        "proxy": false,
        "bad_ip": false,
        "emulator": false,
        "anomalies": {
          "useragent": {
            "old_os_version": false,
            "non_mobile_platform": false
          },
          "conversion_rate": {
            "hourly": true,
            "daily": false
          },
          "click_to_conversion": false
        }
      }
    },
  "result": "OK"
}
```

This endpoint allows you to audit your conversion event on various forms of ad fraud. 

Parameter | Mandatory | Description
--------- | ------- | -----------
ip | yes | The ip address of the device triggering the conversion event.
user_agent | no | The [user agent](https://en.wikipedia.org/wiki/User_agent) string of the device triggering the conversion event.
device_id | no | The [device ID](https://en.wikipedia.org/wiki/Unique_Device_Identification) of the device triggering the conversion event.
referrer_url | no | The url the user was coming redirect from to the specific app
source_id | no | The unique identiefier of the source in the client system
click_time | no | The time (Y-m-d:h:i:s) the attributed click took place
conversion_time | no | The time (Y-m-d:h:i:s) the conversion event took place
bundle_id | no | The bundle id of the specific app in the app store
offer_id | no | The unique identifier of the offer in the client system
allowed_country (array) | no | The countries that are allowed to initiate conversion events for the specific offer

<aside class="notice">
This endpoint is part of the additional <strong>Adfraud protection</strong> module. Contact your Mobbty account manager for details, rates and access.
</aside>

<aside class="warning">
By default the maximum QPM is set to <strong>200</strong>. If you would like to increase this number contact your Mobbty account manager for details and rates.
</aside>

