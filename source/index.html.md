---
title: kuup.la API Reference

language_tabs:
  - shell

includes:
  - errors

search: true
---

# Introduction

The kuup.la API is organized around [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer). It has predictable, resource-oriented URLs, and uses HTTP response codes to indicate API errors. We use built-in HTTP features, like HTTP authentication and HTTP verbs, which are understood by off-the-shelf HTTP clients. [JSON](http://www.json.org/) is returned by all API responses, including errors.

**Never expose your secret API key in any public website's client-side code.**

# Authentication

> Example request:

```shell
# With shell, you can just pass the correct header with each request
curl "https://YOUR_API_DOMAIN/feed?key=your_personal_key"
```

Authenticate your account when using the API by including your personal API key in the request. To obtain a key and the api domain to use, please contact your kuup.la account manager. Do not share your secret API keys in publicly accessible areas such GitHub, client-side code, and so forth.

For all endpoint except the offer feed, authentication to the API is performed via Bearer token. To obtain your Bearer token make a POST first to the login endpoint.

# Offer feed

## Get the kuup.la offer feed

>Example request:

```shell
curl "https://YOUR_API_DOMAIN/feed?key=your_personal_key"
```

>Example response:

```json
{
  "success": true,
  "error_messages": [],
  "offers": {
    "597728475b931M3896461": {
      "ID": "597728475b931M3896461",
      "APP_ID": "451684733",
      "Name": "Pocketcolony_JP_iOS_Non incent_CPI",
      "Description": "Allowed only your direct Japanese publishers.\r\nPlease don`t use the publishers which may happen large amount of clicks.\r\n \r\nHard KPI: RR>25$\r\nless won't be paid\r\n\r\n",
      "Type": "Non incent",
      "Preview_url": "https://itunes.apple.com/jp/app/id451684733",
      "Tracking_url": "http://tracking.mobbty.com/track/c?oid=597728475b931M3896461&pid=12345&cid={your_click_macro}&sid={your_source_macro}",
      "Currency": "USD",
      "Original_name": "ポケコロ 〜ポケット暮らしのコロニアン〜",
      "Icon_url": "http://is5.mzstatic.com/image/thumb/Purple118/v4/a1/0f/1c/a10f1c51-d491-022e-9318-fcfd85502ecb/source/60x60bb.jpg",
      "Platforms": "iPhone,iPad",
      "Countries": "JP",
      "Payout": 1.01,
      "Status": "active",
      "Expiration_date": "2018-03-31T22:59:59.000Z",
      "Approved": 1,
      "Creatives": []
    }
  }
}
```

This endpoint retrieves all offers that you are eligible to run as a publisher.

### HTTP Request

`GET https://YOUR_API_DOMAIN/feed`

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
