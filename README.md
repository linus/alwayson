# Always On

Jag har skaffat domänen alwayson.one som jag tänkte använda för att lägga ut alla mina Beats One playlists (Beats har taglinen “Always On”).

Tänkte att man skulle kunna använda Spotifys API för att läsa och displaya listorna bara. Och sen vänta på en take-down notice:)

## Spotify

### Authorization

We should be able to use the [Client Credentials flow](https://developer.spotify.com/web-api/authorization-guide/#client_credentials_flow):

    $ CLIENT_ID=clientid
    $ CLIENT_SECRET=clientsecret
    $ AUTH=$(echo -n "$CLIENT_ID:$CLIENT_SECRET" | base64 -w 0)
    $ ACCESS_TOKEN=$(curl -s -H "Authorization: Basic $AUTH" -d grant_type=client_credentials https://accounts.spotify.com/api/token | jq -r .access_token)

### Getting the playlists

With the `access_token`, it should be as simple as

    $ USER_ID=userid
    $ PLAYLISTS=$(curl -s -H "Authorization: Bearer $ACCESS_TOKEN" https://api.spotify.com/v1/users/$USER_ID/playlists?limit=50 | jq -r .items[].name)

## Web app

What remains is just to show this data on alwayson.one!
