# Beton Brutal AutoSplit

This is an Auto Splitter for [Beton Brutal](https://store.steampowered.com/app/2330500/BETON_BRUTAL/). It uses AutoSplit by looking at a live recording of your game and match images of the height counter to figure out when to split in LiveSplit.
Since it records your game, this might affect your performance. Playing at lower resolution or limiting AutoSplit FPS can help improve this.

The images are simply screenshots of the game where everything but the height counter is transparent. AutoSplit will only compare the non-transparent pixels.

## Instalation
1. Download [LiveSplit](https://livesplit.org/downloads/) and Extract it somewhere (Ex:`C:\Program Files (x86)`)
2. Download [AutoSplit v1.6.1](https://github.com/Toufool/AutoSplit/releases/tag/v1.6.1) and extract it somewhere. I have mine inside the LiveSplit folder (`C:\Program Files (x86)`).
3. Download [AutoSplitIntegration.dll](https://github.com/Toufool/LiveSplit.AutoSplitIntegration/blob/main/update/Components/LiveSplit.AutoSplitIntegration.dll?raw=true) and paste it under the `Components` folder of your LiveSplit path from Step 1 (Ex: `C:\Program Files (x86)\LiveSplit\Components`)
4. Download [the files in this repository](https://github.com/CubeSkyy/BBAutoSplit/releases) and extract them anywhere. (Ex:`Desktop\BBAutoSplit`)
5. Open LiveSplit (Ex: `C:\Program Files (x86)\LiveSplit\LiveSplit.exe`). If requested to update, press yes:

![image](https://user-images.githubusercontent.com/16226383/233750323-6d52137f-8d18-4c35-ba2b-5ca313bec56e.png)

6. Right click the newly opened timer and press `Open Splits` -> `From file...` and open one of the downloaded splits (Ex:`Desktop\BBAutoSplit\Splits\Beton Brutal - any%.lss`). If you want different splits, you will need to create your own image masks. Refer to the Image Matching Section below.
6. Right click the newly opened timer and press `Open Layout` -> `From file...` and open the downloaded layout (Ex:`Desktop\BBAutoSplitSplits\Beton Brutal.lsl`) this should also open the AutoSplit window. (If it doesn't, right click LiveSplit and click `Control` -> `Start AutoSplit`)

## Setup
The folowing setup serves just as a base, you might need to adjust the settings and images to your computer/display.

### AutoSplit Settings

You can read more about all of these [in this page](https://github.com/Toufool/AutoSplit#split-image-folder).
- **Split Image Folder:** This is where the image masks are stored. Start with the default from `Desktop\Image_Matching`. Make sure to select the folder with your in-game resolution or the splitter will not work. If your Category/Resolution doesn't exist yet, you will need to create your own. This is explained the Section below.
- **Capture Region:** Open Beton Brutal and press `Select Window` and click on Beton. **`Make sure that X/Y are at 0`** and the Width/Height match your resolution.
- **FPS Limit: 30.** Increasing this will make the splitter more acurate but it might slow down the game. I have had good results with 30, but you can experiment with 60.
- **Comparison Method: Histograms.** Default images were tested with this method but L2 Norm should also work. Do not use pHash, it doesn't work with masked images (which we are using).
- **Default Similarity Threshold: 0.99** This determines how similiar the image mask and recording need to be to classify as a match (So it triggers a split). I had good results with this value, but you can experiment. You have a live preview of the current comparison under `Show live similarity`
- **Default Pause Time (sec): 1** This value determines how long the program will pause after spliting. It is mostly used for games with loading screens, which Beton doesn't have.
- **Loop Split Images: Checked** When last split is triggered (a run ends) AutoSplit will reset and look for 0 height again if this setting is checked. Otherwise, you need to manually start it again.

Your settings should look something like this:

![image](https://user-images.githubusercontent.com/16226383/233752679-863008e6-20bb-468a-851a-a4a66eb3e918.png)

After pressing `Reload Start Image` AutoSplit will start looking for a start of a run to start the timer. 

If the splitter is not starting because the `Live Similarity` is lower than the `Current Similarity Threshold`, then you can try to lower the `Default Similarity Threshold`. If this also doesn't work you will need to make your own images, this is explained in a later section.

### Image Matching

You can read more about how image matching works and what each part of the filenames do [in this link](https://github.com/Toufool/AutoSplit#custom-split-image-settings). I will give brief explanations and examples in this section.

The images are simply screenshots of the game where everything but the height counter is transparent. The resolution and position of the counter is crucial for matching to work correctly.

You will need an image editing software that can create PNG's with transparent backgrounds. I recommend [Paint.NET](https://www.getpaint.net/) since its free and lightweight.


To create an image:

1. Start by creating a folder to store all your images.
2. Make sure Beton is being captured by AutoSplit and X/Y are at 0.Enter practice and go to the place you want the split and press `Take Screenshot` in AutoSplit. I have better results when the height counter is under a dark background.
Example:
![image](https://user-images.githubusercontent.com/16226383/233753780-9c8dbc5a-54b0-473a-b840-ad1d70ab26c6.png)

2. Open the image with an editor, delete everything but the height text. I have better results by being very strict in the cropping.
Example:
![image](https://user-images.githubusercontent.com/16226383/233753909-00618c1c-8ce0-4fff-89dd-e176de9cad17.png)
The output should be something like this:
![image](https://user-images.githubusercontent.com/16226383/233753917-bf994f54-10c6-4963-885b-9d9003bbc449.png)

3. Save the image as PNG. The name should be in the format [Split Number]-[Split Name].png. In case you need a special threshold for a specific image you can set it with parenthesis (Ex: `009_353M(0.96).png`.

#### Start Image

This is what determines when the timer is started if its paused. We want this to be the height counter at 0, exactly after we press `Restart Run`.

1. Press `Restart Run` in Beton and without moving the cursor, press `Take Screenshot` in AutoSplit. 
2. Follow steps 1-3 in previous section.
3. Save the image with the name "001_start_auto_splitter{d}.png". Explanation: "start_auto_splitter" is a special tag for the first image. {d} tells AutoSplitter to not split when this image is found. Since it already starts the timer, if {d} is not present, the timer will start and then instantly split when the start image is found.

#### Reset image

AutoSplit will always be looking for the Reset Image. If found, it will reset the timer. We want this to be something specific to the Restart Run Button or Main Menu.

I have had better results using the main menu with the fixed background that appears right after you confirm the `Restart Run`.
1. Confirm `Restart Run` until you get to the main menu.
2. Press `Take Screenshot` in AutoSplit and crop the numbers on the screen that can change (Current Run, Height, etc)
Example:
![image](https://user-images.githubusercontent.com/16226383/233755457-e4d27dd0-a636-4f50-81e5-da85d77942c5.png)
3. Save the image with the "Reset" in the name. You might need to lower the threshold, I found 0.96 to work well, but you can experiment. (Ex: `999_Reset(0.94).png`)


## FAQ

1. Livesplit keeps asking me if I want to save times every time I reset.

- You can disable this under `LiveSplit Settings` -> `Warn On Reset If Better Time`


