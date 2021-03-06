[![banner](https://raw.githubusercontent.com/oceanprotocol/art/master/github/repo-banner%402x.png)](https://oceanprotocol.com)

<h1 align="center">webtasks</h1>

> 🐬 Ocean Protocol's webtasks doing automatic things for us via webtask.io

![giphy](https://user-images.githubusercontent.com/90316/37671913-0eb2f70a-2c6d-11e8-809e-04d3b40ef1c9.gif)

[![Build Status](https://travis-ci.com/oceanprotocol/webtasks.svg?token=3psqw6c8KMDqfdGQ2x6d&branch=master)](https://travis-ci.com/oceanprotocol/webtasks)
[![js oceanprotocol](https://img.shields.io/badge/js-oceanprotocol-7b1173.svg)](https://github.com/oceanprotocol/eslint-config-oceanprotocol) [![Greenkeeper badge](https://badges.greenkeeper.io/oceanprotocol/webtasks.svg)](https://greenkeeper.io/)

- [Tasks](#tasks)
  - [Medium](#medium)
    - [Get Latest Posts](#get-latest-posts)
    - [Get Followers Count](#get-followers-count)
    - [Get Categories](#get-categories)
  - [YouTube](#youtube)
  - [MailChimp](#mailchimp)
  - [Meetup](#meetup)
    - [`GET /`](#get-)
- [Development](#development)
- [Deployment](#deployment)
- [Authors](#authors)
- [License](#license)

---

## Tasks

### Medium

**`webtask-medium.js`**: Generic task to fetch and reconstruct items from any medium publication.

#### Get Latest Posts

Requires the Medium username appended at the end of the url:

```bash
http://localhost:8080/:medium_username

# when published on webtask.io
https://TASK_URL/TASK_NAME/:medium_username
```

#### Get Followers Count

```bash
http://localhost:8080/:medium_username/followers

# when published on webtask.io
https://TASK_URL/TASK_NAME/:medium_username/followers
```

**Response**

```json
{
  "followers": 2528
}
```

#### Get Categories

Reflecting the navigation categories used in the blog.

```bash
http://localhost:8080/:medium_username/categories

# when published on webtask.io
https://TASK_URL/TASK_NAME/:medium_username/categories
```

**Response**

```json
[
    {
        "title": "Product",
        "url": "https://blog.oceanprotocol.com/product/home"
    }, {
        "title": "Community",
        "url": "https://blog.oceanprotocol.com/community/home"
    }, {
        "title": "Deep Tech",
        "url": "https://blog.oceanprotocol.com/deep-tech/home"
    }, {
        "title": "People",
        "url": "https://blog.oceanprotocol.com/people/home"
    }, {
        "title": "Token Launch",
        "url": "https://blog.oceanprotocol.com/token/home"
    }, {
        "title": "Use Cases",
        "url": "https://blog.oceanprotocol.com/usecases/home"
    }
]
```

### YouTube

**`webtask-youtube.js`**: Generic task to fetch and reconstruct items from any YouTube account. YouTube API key is provided via [secret environment variable](https://webtask.io/docs/issue_parameters) `YOUTUBE_API_KEY` setup in web editor of webtask.io.

* `/playlist/:playlistId`: returns all videos in the given playlist.
* `/channel/:channelId`: returns latest 10 videos in the given channel, sorted by date.

Construct your `GET` request urls like so:

```bash
http://localhost:8080/playlist/:youtube_playlist_id
http://localhost:8080/channel/:youtube_channel_id

# when published on webtask.io
https://TASK_URL/TASK_NAME/playlist/:youtube_playlist_id
https://TASK_URL/TASK_NAME/channel/:youtube_channel_id
```

**Response**

For all endpoints, response is reconstructed in the same format:

```json
[{
    "id": "8EMowpNlWPE",
    "title": "Fang Gong, Researcher - Bringing Privacy to Ocean Protocol",
    "description": "At Ocean Protocol, we unlock data for AI and use TCRs to maintain a registry of high-quality data sets through voting. Privacy is essential to protecting the ...",
    "imageUrl": "https://i.ytimg.com/vi/8EMowpNlWPE/mqdefault.jpg",
    "videoUrl": "https://www.youtube.com/watch?v=8EMowpNlWPE"
},
{
    ...
}]
```

If you want to get the original response returned from YouTube API, append `/raw` at the end of your request, e.g.:

```
https://TASK_URL/TASK_NAME/playlist/:youtube_playlist_id/raw
```

### MailChimp

**`webtask-mailchimp.js`**: Task to add a new newsletter subscriber.

Construct your `POST /newsletter` request url like so, e.g. locally:

```bash
http://localhost:8080/newsletter/jelly@mcjellyfish.com

# when published on webtask.io
https://TASK_URL/TASK_NAME/newsletter/jelly@mcjellyfish.com
```

**Response**

When subscription is successful:

```json
{ "status": "created" }
```

When subscriber already exists:

```json
{ "status": "exists" }
```

All errors from MailChimp (when `"status"` is a number) are simply passed through and follow the MailChimp API error responses, e.g.:

```json
{
  "type": "http://developer.mailchimp.com/documentation/mailchimp/guides/error-glossary/",
  "title": "Forgotten Email Not Subscribed",
  "status": 400,
  "detail": "matthias@bigchaindb.com was permanently deleted and cannot be re-imported. The contact must re-subscribe to get back on the list.",
  "instance": "06171156-4bca-4b6c-a84f-aa3690a82798"
}
```

### Meetup

**`webtask-meetup.js`**: Task to get information from Meetup.

#### `GET /`

```bash
http://localhost:8080/meetup

# when published on webtask.io
https://TASK_URL/TASK_NAME/meetup
```

**Response**

Field `groups` holds passed through array of objects with all groups. See https://www.meetup.com/meetup_api/docs/pro/:urlname/groups/

```json
{
    "groups": [{
        ...
    }],
    "members": 40238
}
```

## Development

```bash
npm install wt-cli -g
npm start
```

And go to [localhost:8080](http://localhost:8080)

## Deployment

All tasks are running serverless on webtask.io where you can login with your GitHub account. Then interact with it locally with the `wt-cli`:

```bash
npm install wt-cli -g
wt init YOUR_GITHUB_EMAIL

wt create webtask-medium.js --name medium

# make sure it's there and get url
wt ls
```

## Authors

- Matthias Kretschmann ([@kremalicious](https://github.com/kremalicious)) - [BigchainDB](https://www.bigchaindb.com) & [Ocean Protocol](https://oceanprotocol.com)
- initial Medium web task by Pedro Gomes ([@pedrouid](https://github.com/pedrouid)) - [Balance](https://balance.io)

## License

```
Copyright 2018 Ocean Protocol Foundation Ltd.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
