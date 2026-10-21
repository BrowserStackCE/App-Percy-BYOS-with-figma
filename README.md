# App Percy: Upload Your Own Screenshots

Upload existing screenshots (for example, exported from Figma) to [App Percy](https://www.browserstack.com/docs/app-percy/advanced/byos) without running an app or a device.

## How it works

`manual_upload.js` reads every device folder inside `resources/`, loads its `device.json`, and sends each PNG in that folder to Percy as a snapshot. The snapshot name is the file name without `.png`.

## Folder structure

```
.
├── manual_upload.js
├── .env                  # holds PERCY_TOKEN (git-ignored)
└── resources
    └── pixel7            # one folder per device
        ├── device.json   # device details
        ├── home.png
        ├── list.png
        └── ...
```

To add another device, create another folder under `resources/` with its own `device.json` and screenshots.

## device.json

Set these values to match the device your screenshots were designed for:

```json
{
  "deviceName": "Pixel 7",
  "os": "Android",
  "osVersion": "13",
  "deviceScreenSize": "1080x2400",
  "statusBarHeight": 63,
  "navigationBarHeight": 126,
  "orientation": "portrait"
}
```

| Field | Meaning |
|---|---|
| `deviceName` | Device name shown in Percy |
| `os` | `Android` or `iOS` |
| `osVersion` | OS version, for example `13` |
| `deviceScreenSize` | Screenshot size as `widthxheight` in pixels |
| `statusBarHeight` | Status bar height in pixels |
| `navigationBarHeight` | Navigation bar height in pixels |
| `orientation` | `portrait` or `landscape` |

## Setup

1. Install dependencies:

   ```
   npm install
   ```

   If starting from scratch:

   ```
   npm install --save-dev @percy/cli @percy/appium-app
   ```

2. Get your App Percy project token from your Percy project settings.

3. Create a `.env` file in the project root (it is git-ignored) with:

   ```
   PERCY_TOKEN=your_project_token
   ```

   Never commit your token.

## Run

```
set -a; source .env; set +a
npx percy app:exec -- node manual_upload.js
```

You should see a `Processing ...` line for each screenshot, then `Snapshot taken: <name>`. At the end Percy prints the build URL.

## Troubleshooting

| Problem | Fix |
|---|---|
| `Cannot find module '@percy/appium-app'` | Run `npm install` |
| `Snapshot command was not called` | The script crashed before taking snapshots; check the error above it |
| Nothing is processed | Make sure `resources/<device>/device.json` and PNG files exist |
| Build shows up in the wrong project | Check that `PERCY_TOKEN` in `.env` is the token for the intended project |



## Part 2 Connecting Repo to Figma:
1. After running 1 build , make sure all screenshots are uploaded.
2. Please follow the steps to connect your App Percy Project with Figma : 
https://www.browserstack.com/docs/app-percy/visual-testing-workflows/figma/configuration

3. We selecting the figma frames, ensure you are selecting the right name (as per previous percy builds)

4. After completing Figma Integration, one figma build will automatically be created and auto-approved

5. Run another Percy build using ' set -a; source .env; set +a
npx percy app:exec -- node manual_upload.js' 

6. The future builds should compare snapshots with figma.

Video guide for reference: https://zoom.us/clips/share/CGvbG4cwSva4sY6SkLbD0w 
