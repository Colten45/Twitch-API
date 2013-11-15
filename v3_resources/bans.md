# Bans

***

Stores and updates information about a [channels's][channels] block list.

| Endpoint | Description |
| ---- | --------------- |
| [GET /channels/:channel/bans](/v3_resources/bans.md#get-channelschannelbans) | Get channel's ban list |
| [PUT /channels/:channel/bans/:target](/v3_resources/bans.md#put-channelschannelbanstarget) | Update channel's ban list |
| [DELETE /channels/:channel/bans/:target](/v3_resources/bans.md#delete-channelschannelbanstarget) | Update channel's ban list |

[users]: /v3_resources/users.md

## `GET /channels/:channel/bans`

Returns a list of banned objects on `:channels`'s ban list. List sorted by recency, newest first.

*__Authenticated__*, required scope: `channel_ban_read`

### Parameters

<table>
    <thead>
        <tr>
            <th>Name</th>
            <th>Required?</th>
            <th width="50">Type</th>
            <th width=100%>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>targets</code></td>
            <td>optional</td>
            <td>integer</td>
            <td>Bans from a comma separated list of targets.</td>
        </tr>
        <tr>
            <td><code>limit</code></td>
            <td>optional</td>
            <td>integer</td>
            <td>Maximum number of objects in array. Default is 25. Maximum is 100.</td>
        </tr>
        <tr>
            <td><code>offset</code></td>
            <td>optional</td>
            <td>integer</td>
            <td>Object offset for pagination. Default is 0.</td>
        </tr>
    </tbody>
</table>

### Example Request

```bash
curl -H 'Accept: application/vnd.twitchtv.v3+json' -H 'Authorization: OAuth <access_token>' \
-X GET https://api.twitch.tv/kraken/channels/test_user/bans
```

### Example Response

```json
{
  "_links": {
    "next": "https://api.twitch.tv/kraken/channels/test_user/bans?limit=25&offset=25",
    "self": "https://api.twitch.tv/kraken/channels/test_user/bans?limit=25&offset=0"
  },
  "bans": [
    {
      "_links": {
        "self": "https://api.twitch.tv/kraken/channels/test_user/bans"
      },
      "updated_at": "2013-11-15T01:02:34Z",
      "user": {
        "_links": {
          "self": "https://api.twitch.tv/kraken/users/test_user_troll"
        },
        "updated_at": "2013-02-06T22:44:19Z",
        "display_name": "test_user_troll",
        "staff": false,
        "name": "test_user_troll",
        "_id": 13460644,
        "logo": "http://static-cdn.jtvnw.net/jtv_user_pictures/test_user_troll-profile_image-9e4de45c9e6744ac-300x300.png",
        "created_at": "2010-06-30T08:26:49Z"
      },
      "_id": 970887
    },
    ...
  ]
}
```

## `PUT /channels/:channel/bans/:target`

Adds `:target` to `:channel`'s block list. `:channel` is the authenticated user/channel and `:target` is user to be banned. Returns a bans object.

*__Authenticated__*, required scope: `channel_ban_edit`

### Example Request

```bash
curl -H 'Accept: application/vnd.twitchtv.v3+json' -H 'Authorization: OAuth <access_token>' \
-X PUT https://api.twitch.tv/kraken/channel/test_user1/bans/test_user_troll
```

### Example Response

```json
{
  "_links": {
    "self": "https://api.twitch.tv/channel/test_user1/bans/test_user_troll"
  },
  "updated_at": "2013-11-15T01:02:34Z",
  "user": {
    "_links": {
      "self": "https://api.twitch.tv/kraken/users/test_user_troll"
    },
    "updated_at": "2013-01-18T22:33:55Z",
    "logo": "http://static-cdn.jtvnw.net/jtv_user_pictures/test_user_troll-profile_image-c3fa99f314dd9477-300x300.jpeg",
    "staff": false,
    "display_name": "test_user_troll",
    "name": "test_user_troll",
    "_id": 22125774,
    "created_at": "2011-05-01T14:50:12Z"
  },
  "_id": 287813
}
```

## `DELETE /channels/:channel/bans/:target`

Removes `:target` from `:channel`'s ban list. `:user` is the authenticated user/channel and `:target` is user to be unbanned.

*__Authenticated__*, required scope: `channel_ban_edit`

### Example Request

```bash
curl -H 'Accept: application/vnd.twitchtv.v3+json' -H 'Authorization: OAuth <access_token>' \
-X DELETE https://api.twitch.tv/kraken/channels/test_user1/bans/test_user_troll
```

### Example Response

`204 No Content` if successful.

### Errors

`404 Not Found` if `:target` not on `:channel`'s ban list.

`422 Unprocessable Entity` if delete failed.
