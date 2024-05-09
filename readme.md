# Next California SpaceX Launch Widget

This project provides a Scriptable widget script for iOS that fetches and displays the next SpaceX launch from California. The widget utilizes the Rocket Launch Live API.

## Features
- Fetches and displays the next California SpaceX launch using Rocket Launch Live API.
- Displays the launch name and date in a widget.

## Requirements
- An iOS device with the Scriptable app installed.

## Installation
1. Install the **Scriptable** app from the [App Store](https://apps.apple.com/us/app/scriptable/id1405459188).
2. Create a new script in Scriptable and paste the following code.

### Scriptable Code
```javascript
// Rocket Launch Live API URL for next launches
const API_URL = "https://fdo.rocketlaunch.live/json/launches/next/5";

// Function to fetch the next California SpaceX launch
async function fetchNextCaliforniaLaunch() {
  let response = await new Request(API_URL).loadJSON();
  let results = response.result;
  for (let launch of results) {
    if (launch.provider.name === "SpaceX" && launch.pad.location.state === "CA") {
      let launchDate = new Date(launch.t0).toLocaleString("en-US", {
        dateStyle: "medium",
        timeStyle: "short",
      });
      return {
        name: launch.name,
        date: launchDate,
      };
    }
  }
  return null;
}

// Create the widget
async function createWidget() {
  let widget = new ListWidget();
  widget.backgroundColor = new Color("#1C1C1E");
  widget.setPadding(10, 10, 10, 10);

  let title = widget.addText("Next CA SpaceX Launch");
  title.textColor = Color.white();
  title.font = Font.boldSystemFont(14);
  title.centerAlignText();

  let launchInfo = await fetchNextCaliforniaLaunch();
  if (launchInfo) {
    let launchName = widget.addText(launchInfo.name);
    launchName.textColor = Color.green();
    launchName.font = Font.boldSystemFont(12);
    launchName.centerAlignText();

    let launchDate = widget.addText(launchInfo.date);
    launchDate.textColor = Color.white();
    launchDate.font = Font.systemFont(10);
    launchDate.centerAlignText();
  } else {
    let noLaunchText = widget.addText("No CA SpaceX Launch Found");
    noLaunchText.textColor = Color.red();
    noLaunchText.font = Font.systemFont(10);
    noLaunchText.centerAlignText();
  }

  return widget;
}

// Display the widget
let widget = await createWidget();
if (config.runsInWidget) {
  Script.setWidget(widget);
} else {
  widget.presentSmall();
}
Script.complete();
```

## Usage
1. Paste the code above into a new script in the Scriptable app.
2. Save the script and run it to test the widget.
3. Add the Scriptable widget to your home screen.
4. Configure the widget to display this script.

## Customization
• Change the Background Color: Modify the widget.backgroundColor property.
• Adjust Text Colors and Font Sizes: Update the textColor and font properties.
