# Spotify-on-Readme

This is a simple tutorial of how to use **Github Action** and **Spotify Web API** to display & update spotify stats on your github readme.  
Check out my [github profile](https://github.com/Tanimal19) to see what it looks like.

<br>

## Prerequisite

1. having a Spotify account
2. go to <https://developer.spotify.com/documentation/web-api> and create an app
   - `website`: `https://github.com/<user_name>`
   - `redirect_uri`: `http://127.0.0.1:8888/callback`
   - `APIs uesd`: select `Web API`
3. nodejs

<br>

## 1. Set up and Get token

1. clone this repo
2. run
```
npm install express axios cors cookie-parser
```
3. replace these variable in `app.js`
```js
var client_id = ''; // Your clientId
var client_secret = ''; // Your client secret
var redirect_uri = 'http://127.0.0.1:8888/callback'; // Your redirect uri
var scope = 'user-top-read playlist-read-private'; // Your scope
```

4. run
```
node app.js
```
5. go to <http://127.0.0.1:8888> and simply follow the instruct,  
   then copy&save your `refresh_token` 

<br>

## 2. Configure github action

1. Set up a new github action
2. Copy `workflows/*` to your github action's folder (`.github/workflows/`)
3. Add github secrets (Settings -> Secrets and variables -> Actions -> New repository secret)  
   - `SPOTIFY_CLIENT_ID`: your clientId
   - `SPOTIFY_CLIENT_SECRET`: your client secret
   - `SPOTIFY_REFRESH_TOKEN`: your refresh_token

> [!IMPORTANT]
> This config will get your  
> **top 5 artists** and **top 10 tracks** on spotify **for last 4 weeks**
> and update once a week

> [!TIP]
> Get any data you want by using `fetchSpotifyStats(url)`  
> Go to [Spotify API document](https://developer.spotify.com/documentation/web-api/reference/get-users-top-artists-and-tracks) to know how to configure the url  
> Don't forget to do step 1 (Set up and Get token) again and modify the [scope](https://developer.spotify.com/documentation/web-api/concepts/scopes)

<br>

## 3. Add tags into readme

add below snippet into your `README.md`
```
<table>
  <tr>
    <td align="center"><strong>Top Artists</strong></td>
    <td align="center"><strong>Top Tracks</strong></td>
  </tr>
  <tr>
    <td align="center" id="top-artist"></td>
    <td id="top-track"></td>
  </tr>
</table>
```
`get_spotify_data.js` will replace the content of `<td align="center" id="top-artist"></td>` with an unordered list of **top 5 artists**, and replace the content of `<td id="top-track"></td>` with an ordered list of **top 10 tracks**.  

> [!TIP]
> You can check out and modify `get_spotify_data.js` to change the format

> [!WARNING]
> According to [Spotify Design Guidelines](https://developer.spotify.com/documentation/design), you should put Spotify's logo when utilize it's content.
