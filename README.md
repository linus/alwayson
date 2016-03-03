# Always On

Jag har skaffat domänen alwayson.one som jag tänkte använda för att lägga ut alla mina Beats One playlists (Beats har taglinen “Always On”).

Tänkte att man skulle kunna använda Spotifys API för att läsa och displaya listorna bara. Och sen vänta på en take-down notice:)

## More requirements

Jag vet inte riktigt hur jag tänkt med playlistsen, för det går ju att embedda dom liksom, med player och hela skiten. Enklaste lösningen kan jag tänka mig. 

Nu kan man sätta en egen cover art på varje playlist också, vet inte om man kan hämta den infon bara.

Det som dock hade varit så fett är om man kunde scrapea playlistsen på nått vänster så att tracksen är sökbara, så om man jagar en låt el artist listas playlistsen låten är med i.

Sen hade en rolig grej varit att spärra siten endast i Californien, för det är säkert där Apples legal sitter, haha

## Spotify

### Authorization

We should be able to use the [Client Credentials flow](https://developer.spotify.com/web-api/authorization-guide/#client_credentials_flow):

```bash
$ CLIENT_ID=clientid
$ CLIENT_SECRET=clientsecret
$ AUTH=$(echo -n "$CLIENT_ID:$CLIENT_SECRET" | base64 -w 0)
$ ACCESS_TOKEN=$(curl -s -H "Authorization: Basic $AUTH" -d grant_type=client_credentials https://accounts.spotify.com/api/token | jq -r .access_token)
```

### Getting the playlists

With the `access_token`, it should be as simple as

```bash
$ USER_ID=userid
$ PLAYLISTS=$(curl -s -H "Authorization: Bearer $ACCESS_TOKEN" https://api.spotify.com/v1/users/$USER_ID/playlists?limit=50 | jq -r .items[].name)
$ PLAYLIST_HREFS=$(curl -s -H "Authorization: Bearer $ACCESS_TOKEN" https://api.spotify.com/v1/users/$USER_ID/playlists?limit=50 | jq -r .items[].href)
$ for url in $PLAYLIST_HREFS; do curl -s -H "Authorization: Bearer $ACCESS_TOKEN" $url | jq '"Playlist \(.name), \(.tracks.items[].track.name)"'; done
```

## Web app

What remains is just to show this data on alwayson.one!
