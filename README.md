# PinGrabber

A Chrome Extension for extracting and downloading media from Pinterest. It handles static images, native videos, and dynamic Canvas animations.

## Features

* **Video Extraction:** Bypasses Blob URLs by fetching the clean HTML of the current route to parse the direct `.mp4` source.
* **Image Extraction:** Automatically modifies Pinterest's thumbnail URLs to request the `/originals/` high-resolution `.jpg` versions.
* **Canvas Recording:** Uses the `MediaRecorder` API and `captureStream()` to record WebGL/Canvas animations directly from the DOM, exporting them as `.webm` files.
* **SPA Handling:** Accounts for Pinterest's Single Page Application architecture to prevent downloading cached media from previous routes.
* **Background Execution:** Fully functional via keyboard shortcuts (`Alt + P`) using Chrome's `background` service workers.

## How it Works

Pinterest employs several techniques to protect its media, such as serving videos as Blob URLs and rendering animations via isolated `<canvas>` elements. 

PinGrabber solves this by:
1. Injecting a content script to analyze the DOM of the active tab.
2. If a video is detected, it performs a silent `fetch` request to the current URL. This retrieves the raw HTML, allowing the script to parse the metadata and extract the direct `.mp4` link via Regex, bypassing the Blob player.
3. If a canvas animation is detected, it exposes a UI to set FPS and Duration. It then utilizes `canvas.captureStream()` to record the frames and creates a downloadable Blob via the `chrome.downloads` API. Memory leaks are prevented by explicitly stopping all media tracks post-recording.

## Installation (Developer Mode)

1. Clone this repository or download the source code.
2. Open Google Chrome and navigate to `chrome://extensions/`.
3. Enable **Developer mode** in the top right corner.
4. Click on **Load unpacked** and select the extension directory.
5. Pin the extension to your toolbar or use `Alt + P` on any Pinterest media page.
