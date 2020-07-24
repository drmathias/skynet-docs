# Getting Started

## Making Your First API Call

```shell--curl
curl -X POST "https://siasky.net/skynet/skyfile" -F file=@image.jpg
```

```shell--cli
skynet upload "./image.jpg"
```

```javascript--browser
import { upload } from "skynet-js";

// Assume we have a file from an input form.

try {
  const { skylink } = await upload("https://siasky.net", file);
} catch (error) {
  console.log(error)
}
```

```javascript--node
const skynet = require('@nebulous/skynet');

(async () => {
	const skylink = await skynet.UploadFile(
		"./image.jpg"
	);
	console.log(`Upload successful, skylink: ${skylink}`);
})();
```

```python
import siaskynet as skynet

skylink = skynet.upload_file("image.jpg")
print("Upload successful, skylink: " + skylink)
```

```go
package main

import (
	"fmt"
	skynet "github.com/NebulousLabs/go-skynet"
)

func main() {
	skylink, err := skynet.UploadFile("./image.jpg", skynet.DefaultUploadOptions)
	if err != nil {
		panic("Unable to upload: " + err.Error())
	}
	fmt.Printf("Upload successful, skylink: %v\n", skylink)
}
```

The SDKs are set up to be as simple as possible. Despite the many options for configuration, most users will be able to get started with a single API call. In the example on the right, we upload the file `image.jpg` to the default Skynet portal, `https://siasky.net`.

## Using A Different Portal

The default portal used is `https://siasky.net` (also available through the exported constant, `defaultPortalUrl`) and no configuration is required to use it. Having a reasonable choice already selected keeps friction for new developers low. However, as Skynet is a decentralized project, it is also possible to use different portals.

In most SDKs, a different portal can be passed in as part of the options for each function.

### Browser JS

```javascript--browser
import SkynetClient from "skynet-js";

// Or SkynetClient() without arguments to use the default portal.
const client = new SkynetClient("https://some-other-portal.xyz");

// Assume we have a file from an input form.

try {
  const { skylink } = await client.upload(file);
} catch (error) {
  console.log(error)
}
```

In Browser JS it is possible to either:

- Use the standalone functions and pass in a different portal as the first argument.
- Create a new client with the desired portal. Clients implement all the standalone functions as methods with bound `portalUrl` so you don't need to repeat it every time. See the code example on the right.

## Setting Additional Options

Each SDK function also accepts additional options. These vary depending on the endpoint and are documented alongside each function.

<aside class="notice">
In most of our SDKs the additional options may be left out of the function calls, in which case the default options are used. In <b>Go</b> the options must always be passed in, as the language does not support optional function parameters.
</aside>

### Common Options

Every function accepts the following common options:

Field | Description | Default
----- | ----------- | -------
`portal` | The URL of the portal. | `"https://siasky.net"`
`endpointPath` | The relative path on the portal where the endpoint may be found for the function being called. Some portals, for example, may offer alternate download paths. | `""`
`customUserAgent` | This option is available to change the User Agent, as some portals may reject user agents that are not `Sia-Agent` for security reasons. | `""`