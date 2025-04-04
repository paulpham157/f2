---
outline: [2,3]
---

## Parameter list

| Short parameter | Long parameter | Type | Description |
| ------ | ------ | ---- | ---- |
| `-c`   | `--config` | `FILE` | Profile path, lowest priority |
| `-k`   | `--key` | `TEXT` | Bark's `API Key` |
| `-d`   | `--token` | `TEXT` | Bark's `Device Token` |
| `-M`   | `--mode` | `ENUM` | Push Mode |
| `-t`   | `--title` | `TEXT` | Push Title |
| `-b`   | `--body` | `TEXT` | Push Body |
| `-cl`  | `--call` | `BOOLEAN` | Whether to continue ringing, default is off |
| `-l`   | `--level` | `ENUM` | Push Level |
| `-v`   | `--volume` | `INTEGER` | Push Volume, Range `0-10` |
| `-bd`  | `--badge` | `INTEGER` | Push notification badge count |
| `-ac`  | `--autoCopy` | `BOOLEAN` | Whether to automatically copy and push content |
| `-cp`  | `--copy` | `TEXT` | Specify the content to be copied, or if not specified, the entire push content |
| `-s`   | `--sound` | `ENUM` | Push ringtones |
| `-i`   | `--icon` | `TEXT` | Push icon URL, download the same icon URL only once |
| `-g`   | `--group` | `TEXT` | Push grouping, Notification Center will display pushes by group |
| `-a`   | `--isArchive` | `BOOLEAN` | Whether to save the push, saved by default |
| `-u`   | `--url` | `TEXT` | Clicking on the push notification jumps to the URL |
| `-P`   | `--proxies` | `TEXT...` | Proxy server |
|        | `--update-config` | `FLAG` | Update configuration file |
|        | `--init-config` | `TEXT` | Initialize configuration file |
| `-h`   |               | `FLAG` | Display rich text help |
|        | `--help`      | `FLAG` | Display the help message and exit |


## Detailed description

### `--config`

Specify the path to the configuration file, with the lowest priority. The default path for the **main configuration file** is `f2/conf/app.yaml`, which supports **absolute paths** and **relative paths**.

### `--key`

The `key` of `Bark` is a 22-character string. It can be viewed on the homepage of the `Bark` app.

![Bark Key](/bark/bark-key.jpg)

::: details :link: Example: Bark's official `/push` interface
::: code-group
```shell [bash]
curl -X "POST" "https://api.day.app/push" \
     -H 'Content-Type: application/json; charset=utf-8' \
     -d $'{
  "body": "Test Bark Server",
  "title": "Test Title",
  "device_key": "your_key"
}'
```
:::

### `--token`

The `Token` of `Bark` is a 64-bit string. It can be viewed on the homepage of the `Bark` app.

![Bark Token](/bark/bark-token.jpg)

::: warning :lock: Key source
- The `Key` of `Bark` is generated by the `Bark` server, **do not disclose** it, including developers and the community.
- The `Device Token` of `Bark` is generated by the `Apple` server, **do not disclose**.
:::

::: danger :bangbang: Privacy Protection :bangbang:
- Never share your `Key` and `Token` in public places such as `Discussions`, `Issues`, `Discord`, `etc.`, and be careful to delete sensitive information.
- Anyone who gets your `Key` can send push notifications to your device.
- When a leak occurs, please immediately regenerate the `Key` and `Token` in the `Bark` app.
- More privacy protection information, please refer to the [Privacy Security](https://bark.day.app/#/privacy) of `Bark`.
:::

### `--mode`

Specify the push method:

- `get`: Send notification using GET request (**default**).
- `post`: Send notification using POST request.

::: info :information_source: Precautions
- If you manually splice parameters, you need to pay attention to the issue of `URL encoding`; `F2` will automatically handle the encoding.
- When using `post` mode, parameters are sent in `JSON` format, no manual encoding is required.
- [FAQ: URL encoding](https://bark.day.app/#/faq?id=%e6%8e%a8%e9%80%81%e7%89%b9%e6%ae%8a%e5%ad%97%e7%ac%a6%e5%af%bc%e8%87%b4%e6%8e%a8%e9%80%81%e5%a4%b1%e8%b4%a5%ef%bc%8c%e6%af%94%e5%a6%82-%e6%8e%a8%e9%80%81%e5%86%85%e5%ae%b9%e5%8c%85%e5%90%ab%e9%93%be%e6%8e%a5%ef%bc%8c%e6%88%96%e6%8e%a8%e9%80%81%e5%bc%82%e5%b8%b8-%e6%af%94%e5%a6%82-%e5%8f%98%e6%88%90%e7%a9%ba%e6%a0%bc)
:::

- `cipher`: push encryption mode <Badge type="warning" text="Experimental" />

Push encryption is a method to protect push content, which encrypts and decrypts the push content with a custom key to prevent the push content from being accessed or leaked by the `Bark` and `Apple` servers.

The details are all completed automatically by `F2`, just provide the plain text content normally.

![Bark Ciphertext](/bark/bark-ciphertext.jpg)

![Bark Ciphertext Setting](/bark/bark-ciphertext-setting.jpg)

::: details :link: Example: `F2` official encrypted push command
```shell [bash]
f2 bark -t "Test Title" -b "Test Body" -M cipher
```
:::

::: details :link: Example: `Bark` official sending script
<<< @/snippets/bark/ciphertext.sh
:::

> [!IMPORTANT] IMPORTANT ❗❗❗
> - After enabling push encryption, the default `get` request mode will no longer be applicable and needs to be switched to `cipher` mode.
> - In the `Push Encryption` settings on the `Bark` homepage, a custom key must be configured as required. The length of the key is determined by the selected encryption algorithm.
> - To enhance safety and reduce collision probability, `F2` automatically uses random `iv`. In `Bark`, `iv` can be filled in arbitrarily.
> - Although the push encryption feature is still in the experimental stage, it is recommended to use the `AES-256-CBC` encryption mode first. Because the `GCM` mode of the current `Bark v1.4.3(5)` version is not fully supported yet and will need to be enabled in a future version. [Allows use of GCM Mode#262](https://github.com/Finb/Bark/commit/8a2a7fc2b44073498e4abea54f62497a0e06926e).
> - For more information, see `Bark`'s [Push Encryption](https://bark.day.app/#/encryption).

### `--title`

Push title. It is recommended not to exceed **8 characters**.

### `--body`

Pushed content. It is recommended not to exceed **30 characters**.

### `--call`

Whether to continue ringing, default **off**.

### `--level`

Push notification priority:
- `active`: **Default**
- `timeSensitive`: timeliness notification
- `passive`: Passive notification
- `critical`: emergency notification

::: warning :warning: Important tips :warning:
- Notifications set to `critical` level will be forced to vibrate in `iOS 15` and above, even if the device is in silent mode.
:::

### `--volume`

Push volume, range `0-10`.

### `--badge`

The number of push badges will be displayed on the icon of the `Bark` app.

### `--autoCopy`

Whether to automatically copy push content.

Due to tightened permissions, versions after `iOS 14.5` cannot automatically copy the push content to the clipboard when receiving a push.

You can pull down to push or swipe the push point left on the lock screen to view it to automatically copy it, or click the pop-up push and copy button.

### `--copy`

Specify the content to be copied; if not specified, the entire push content will be copied. Generally used in scenarios where it is necessary to copy push content.

### `--sound`

Push notification ringtone, optional values:
- `birdsong`
- `alarm`
- `chord`
- `dog`
- `guitar`
- `piano`
- `ring`
- `robot`
- `siren`
- `trumpet`
- `vibrate`
- `none`
- ……

For more ringtones, check out the settings of the `Bark` app. It also supports custom ringtones.

### `--icon`

Push icon `URL`, download the same icon `URL` only once. The default icon for `F2`.

![F2 Icon](/f2-logo.ico)

### `--group`

Push groups, the notification center will display push notifications according to groups. Suitable for different push scenarios, such as different application and server conditions. It is recommended not to exceed **8 characters**.

### `--isArchive`

Whether to save the push, default **save**. If closed, the push will not be saved in the `Bark` app.

### `--url`

The URL that jumps when you click push supports `URL Scheme` and `Universal Link`. Suitable for some scenarios that require callback or jump.

<video src="https://github.com/user-attachments/assets/ab289f3c-dcb4-4c08-af75-91a3109493f8" width="70%" height="auto" autoplay loop style="border-radius: 8px; overflow: hidden;"></video>

::: details :link: Example: Use the `URL Scheme` to jump to the official website of the `F2` document.
```shell [bash]
f2 bark -t "Jump Test" -b "Open F2 Docs" --url "https://f2.wiki/"
```
:::

::: tip :earth_asia: Update route - Official version
- Press `F2` to push live broadcast notifications to users, click the notification to remotely download the live broadcast.
- Press `F2` to push work update notifications to users, click the push to download updates.
By the `F2 URL Scheme` command to achieve remote control.
:::

### `--proxies`

Configure the proxy server, supporting up to two parameters, corresponding to the `http://` and `https://` protocols.

Example: `--proxies http://x.x.x.x https://x.x.x.x`。

> [!IMPORTANT] IMPORTANT ❗❗❗
> **If the proxy does not support egress HTTPS, use: `--proxies http://x.x.x.x http://x.x.x.x`.**

### `--update-config`

Update configuration files via `CLI` parameters. For details, see: [Configuring Cookies](/en/site-config#configure-cookies)。

### `--init-config`

Initialize the high-frequency configuration file. See: [Initialize Config File](/en/site-config#initialize-configuration-file)。