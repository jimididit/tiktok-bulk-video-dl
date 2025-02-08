# Bulk TikTok Video Downloading

 Easy steps for bulk downloading TikTok videos.

## Requirements

1. A computer (desktop/laptop)
2. Web Browser (I used Chrome)
3. Google Sheets (for saving list of video urls)
4. TikTok - [TikTok webpage](https://tiktok.com)

## Step 1

Go to TikTok webpage. Open Developer Tools (press F12). Go to console tab.
Paste the following code into console then press Enter.

```javascript
// Scroll from top to bottom of page to ensure all video are loaded
let goToBottom = setInterval(() => window.scrollBy(0, 400), 1000);
```

## Step 2

**NOTE: As of February 1st this code works, but TikTok might change classnames which would break this code.**
Paste the following code into the console then press Enter.

```javascript
let arrayVideos = [];
console.log('\n'.repeat(50));

// Locate all video containers in the page
const videoContainers = document.querySelectorAll('.css-1uqux2o-DivItemContainerV2'); // This miay need to be changed

videoContainers.forEach((container, index) => {
    const descriptionElement = container.querySelector('img[alt]'); // Locate <img> tag that has alt attribute with description
    const videoLinkElement = container.querySelector('a[href^="https://www.tiktok.com/"]'); // Find the <a> tag with the video URL

    const description = descriptionElement ? descriptionElement.getAttribute('alt').trim() : 'No description'; // Extract description if it exists
    const videoURL = videoLinkElement ? videoLinkElement.href : 'No URL'; // Extract URL or default

    if (description && videoURL) {
        arrayVideos.push(`"${description}","${videoURL}"`); // Format as CSV
        console.log(`Description ${index + 1}: ${description}, URL: ${videoURL}`);
    } else {
        console.log(`Could not find a description or URL for video ${index + 1}.`);
    }
});

// Generate CSV file
if (arrayVideos.length > 0) {
    const csvContent = "Description,URL\n" + arrayVideos.join("\n");
    const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8;' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = 'tt_video_data.csv';
    a.click();

    console.log("Success! CSV file created!.");
} else {
    console.log("No data found to export.");
}
```

## Step 3

Upload tt_video_data.csv to Google Sheets. You should see a column containing the all of video urls (should be column C).
Click on the first url, scroll to the end of the list, then click on the very last url while holding Shift, it will select all urls. Use Ctrl+C to copy all urls.

## Step 4

Go to [TokDownload](https://www.tokdownload.com) Paste copied urls into textbox and click the download button.
It will take several minutes to download.

Be sure to download videos from TikTok before the ban is in affect again!

**If this was helpful please follow me and star this repo, thanks.**
