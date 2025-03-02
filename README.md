# @distube/ytdl-core

Cloudflare-compatible fork of the DisTube fork of `ytdl-core`. Beyond the improvements in the DisTube version, this fork provides compatibility with the Cloudflare Workers API (which is a subset of Node's APIs). To accomplish that, support was dropped for specifying cookies or proxy settings. Since the fetch API used by Cloudflare differs slightly from the Node version, there are some syntax differences in interfacing with ytdl too. The examples below may require tweaking to run on this fork.

<a href='https://ko-fi.com/skick' target='_blank'><img height='48' src='https://storage.ko-fi.com/cdn/kofi3.png' alt='Buy Me a Coffee at ko-fi.com' /></a>

## Usage

```js
const ytdl = require("@distube/ytdl-core");
// TypeScript: import ytdl from '@distube/ytdl-core'; with --esModuleInterop
// TypeScript: import * as ytdl from '@distube/ytdl-core'; with --allowSyntheticDefaultImports
// TypeScript: import ytdl = require('@distube/ytdl-core'); with neither of the above

// Download a video
ytdl("http://www.youtube.com/watch?v=aqz-KE-bpKQ").pipe(require("fs").createWriteStream("video.mp4"));

// Get video info
ytdl.getBasicInfo("http://www.youtube.com/watch?v=aqz-KE-bpKQ").then(info => {
  console.log(info.videoDetails.title);
});

// Get video info with download formats
ytdl.getInfo("http://www.youtube.com/watch?v=aqz-KE-bpKQ").then(info => {
  console.log(info.formats);
});
```

## API

You can find the API documentation in the [original repo](https://github.com/fent/node-ytdl-core#api). Except a few changes:

### `ytdl.getInfoOptions`

- `requestOptions` is now `undici`'s [`RequestOptions`](https://github.com/nodejs/undici#undicirequesturl-options-promise).
- `agent`: [`ytdl.Agent`](https://github.com/distubejs/ytdl-core/blob/master/typings/index.d.ts#L10-L14)
- `playerClients`: An array of player clients to use. Accepts `WEB`, `WEB_EMBEDDED`, `TV`, `IOS`, and `ANDROID`. Defaults to `["WEB_EMBEDDED", "IOS", "ANDROID","TV"]`.
- `fetch`: Custom fetch implementation. Defaults to `undici`'s request.

## Limitations

ytdl cannot download videos that fall into the following

- Regionally restricted
- Private
- Rentals
- YouTube Premium content
- Only [HLS Livestreams](https://en.wikipedia.org/wiki/HTTP_Live_Streaming) are currently supported. Other formats will get filtered out in ytdl.chooseFormats

Generated download links are valid for 6 hours, and may only be downloadable from the same IP address.

## Rate Limiting

When doing too many requests YouTube might block. This will result in your requests getting denied with HTTP-StatusCode 429. The following steps might help you:

- Update `@distube/ytdl-core` to the latest version
- Wait it out (it usually goes away within a few days)

## Related Projects

- [DisTube](https://github.com/skick1234/DisTube) - A Discord.js module to simplify your music commands and play songs with audio filters on Discord without any API key.
- [@distube/ytsr](https://github.com/distubejs/ytsr) - DisTube fork of [ytsr](https://github.com/TimeForANinja/node-ytsr).
- [@distube/ytpl](https://github.com/distubejs/ytpl) - DisTube fork of [ytpl](https://github.com/TimeForANinja/node-ytpl).
