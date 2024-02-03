# Automatically Disconnect Bluetooth Device on Sleep (macOS)

## Why I Made This

I often use my JBL headphones with my MacBook Air for music and work. However, I faced the inconvenience of forgetting to disconnect the headphones before putting my laptop in my backpack. This meant I had to take out my laptop again to manually disconnect them.

## Solution

To solve this problem, I used a combination of Blueutil, AppleScript, and Sleepwatcher to automatically disconnect a specific Bluetooth device (in this case, my headphones) without turning off Bluetooth entirely, ensuring that features like Find My Mac continue to work.

### Step 1: Install Required Tools

- [Blueutil](https://github.com/toy/blueutil): Blueutil is a command-line utility for managing Bluetooth on macOS. You can install it using Homebrew with the following command:

```bash
brew install blueutil
```

- [Sleepwatcher](https://github.com/Erik1002/autosleep): Sleepwatcher is a tool that allows you to run scripts on sleep and wake events. Install it using Homebrew:

```bash
brew install sleepwatcher
```

### Step 2: Find Your Device ID

Run the following command in the terminal to find the ID of your connected Bluetooth device:

```bash
blueutil --connected
```

### Step 3: Create and Test an AppleScript

Create an AppleScript to turn off the specific device using its ID. Replace `"20-64-de-1d-d8-03"` with your device's ID in the following script:

```applescript
do shell script quoted form of "/opt/homebrew/bin/blueutil" & " --disconnect " & quoted form of "20-64-de-1d-d8-03"
```

### Step 4: Create a Sleepwatcher File

Create a `.sleep` file in your home directory:

```bash
vi ~/.sleep
```

Add the following script to the `.sleep` file. Make sure to replace the device ID with your own:

```bash
#!/bin/bash
osascript -e 'do shell script quoted form of "/opt/homebrew/bin/blueutil" & " --disconnect " & quoted form of "20-64-de-1d-d8-03"'
```

Give execute permissions to the `.sleep` file:

```bash
chmod 700 ~/.sleep
```

### Step 5: Test the Script

Run the following command to test the script:

```bash
/usr/local/sbin/sleepwatcher --verbose --sleep ~/.sleep
```

And Voila! No need to take my laptop out in the middle of the street every single time i get out of a café.
