# Auralization
## Welcome
This repository contains a Max patch for generating an auralization with a GUI.

This patch is a work in progress that is designed to be an easy setup for anyone that would like to create auralizations. It is designed to only require 4 inputs:
1. Audio to be auralized and played back (this should be anechoic audio for best results).
2. Ambisonic impulse responses (IRs) of the to-be-auralized space either artificially generated or measured in the field.
3. "Presentation" images that are designed to show the viewpoint of the receiver within the space.
4. "User Interface" (UI) images designed to show a scale model of the space being auralized.

From these inputs, auralizations on a range of playback media are possible. If you are interested, let's dive in!

(See subpatch "p Detailed_Instructions" within the main patcher for a full rundown of everything listed below and more.)


## Auralization Principles
Conceptually understanding what an auralization actually is/does is critical before creating one as there is a great deal of nuance to creating a legitimate auralization. The definition of auralization is to model and simulate the experience of acoustic phenomena rendered as a soundfield in a virtualized space. This basically means that an auralization allows a person to use headphones or speakers to listen to a space without having to physically be in it.

The basic mathematic principals at work boil down to combining (convolving) two acoustic signals together to create an augmented third signal that represents the space that is being modeled. One signal is an "impulse response", or IR, which can be thought of as the acoustic DNA of the space that it was captured in. It is a full frequency transient signal, that sounds similar to a gunshot or balloon pop. The second signal is an "anechoic audio" signal, which is an audio signal recorded in the absence of all echoes or reflections. This audio can be anything, like a person talking or playing violin. Think of a room completely covered in thick blankets and how dead or dry it may sound. Anechoic audio is similar to this, but to the nth degree. The goal is to make that very dry sounding violin player sound like he/she is playing in the space that the gunshot (IR) was recorded.

That doesn't sound too complicated, right?

The other important concept to getting this all to work is the spatialization of sound. Everyone is used to mono or stereo recordings; just think of any song that you listen to on Spotify through your headphones. However, based on the shape of your ears, you are very adept at determining whether a sound is coming from in front or behind you, or above or below. This spatial component is missing from stereo recordings and therefore a great deal of realism is lost in an auralization that would be boiled down to 2 channels. To solve this issue, a multichannel spatial sound format called "ambisonics" is utilized to create the perception of a sound coming from in front or behind or above or below. The IR will be encoded in an ambisonic format, which contains not only the frequencies that are being heard, but also the directions in which they are coming from. When the anechoic audio is combined with this ambisonic (spatial) IR, the spatial properties of the IR will be embedded into the new signal. This allows you to listen to that violin player performing in front of you whilst hearing the reflections off of the side and backwalls enveloping you.

With these basic concepts, a relatively realistic auralization can be produced. To recap, here is an example.
- Notre Dame Cathedral is modeled in a 3D acoustic modeling software (CATT, Odeon, EASE).
- An ambisonic IR was artificially created from this model that captures all acoustic reflections in time and space.
- A choir was recorded singing in an anechoic chamber, filled with foam wedges to eliminate any reverberation or echo.
- The ambisonic IR is convolved (combined) with the anechoic recording of the choir.
- The newly combined audio signal is now decoded from ambisonics to a speaker array around a listening position.
- The listener in the middle of the speaker array hears what the choir would sound like if they were singing in Notre Dame Cathedral, including perceiving the reverberance of the room and the reflections from above, below, and all around.

While this is an incredible method of modeling soundscapes, it needs to be understood that the reproduced sound will never exactly sound like the space being modeled. There are far too many nuances in geometry and material properties within a space to accurately capture and reproduce all of it. An auralization can be visually likened to a watercolor painting where you still understand what the painting is depicting, but it does not contain the fine details of a photograph.


## Steps (Getting this patch to work!)
Before we begin, you will need to gather the following files created from CATT/Odeon & Rhino/Revit/Sketchup:
   1. Anechoic audio to be convolved.
   2. Ambisonic impulse responses (IRs) generated from CATT or measured in the field.
   3. Presentation images taken from the viewpoint of the receiver.
   4. User interface (UI) images that show a scale model of the space being auralized.
For more information on the files required, the file structure, and the naming convention, please reference the "File Structure" and "File Naming Convention" sections of this subpatch.

Ok, so let's get to it!

1. Check to make sure that all of your file locations are included in Max's path. Please read these "File Structure" and "File Naming Convention" sections of this subpatch before continuing to step 2.
2. Pull up the main patcher window (The one that literally says "Auralization Main Patcher Window" in the top left corner). Make sure that you are not in "presentation mode". To determine if you are in presentation mode, locate the easel icon at the bottom left of the main patcher window. If it is highlighted yellow, then you are in presentation mode, if it is grayed out, then you are not in presentation mode. You want it grayed out for the following steps.
2. On your desktop file explorer, locate the folder where you are storing your anechoic audio. Drag and drop it from your file explorer to red "Anechoic Audio" box in the "Folder Selection" section of the main patcher window. This will load the filenames into the anechoic audio dropdown menu in the main patcher (excluding file extension). This will be used when selecting which audio source you want to auralize later. Make sure that your anechoic audio files are named exactly how you want them to be presented in the dropdown menu.
3. Go back to your desktop file explorer and locate the folder where you are storing your ambisonic impulse responses (IRs). Drag and drop this folder from your file explorer to the blue "Impulse Responses" box in the "Folder Selection" section of the main patcher window. Be patient here! This usually takes 5-10 seconds. This will load all IR files into a "polybuffer~", which essentially is a buffer that can hold multiple files. These are the IRs that you will convolve your anechoic audio with to make is sound like the anechoic audio is being played in the space the IRs were capture.
4. The Room Conditions need to be manually typed into the "Room Condition Select" menu. To do this, fir unlock the patch. Then select the "umenu" object located under "Room Condition Select", open the "Max Inspector" (the little circle icon with an "i" in it on the right side of the Max window), scroll down to "Menu Items", and select edit. Enter all room conditions in the text field that pops up, each separated by a comma. Make sure that they are in the same order as your files are indexed (i.e. if your IR files are 11.wav, 12.wav, 13.wav, 14.wav, then you need to order your conditions are "Condition 1", "Condition 2", "Condition 3", "Condition 4".). Lock the patch when finished.
5. Select "Presentation Mode" for the main patcher window by clicking on the easel icon in the bottom left of the screen. The UI image should already automatically be loaded. If nothing appears, check your file names and Max's path to make sure everything is correct.
6. Unlock your patch. Resize the UI image as necessary.
7. There should be 5 red buttons with numbers on them. These correspond to the receiver locations for your IRs. Situate them in the appropriate locations on the UI image of where each receiver was in the space.
8. Lock your patch. Exit "Presentation Mode"
9. In the "Ambisonic Sorting Method" section in the main patcher, select the method in which the channels of the ambisonic audio file are ordered. For more information on this, please reference the "Ambisonics Channel Ordering Conventions" section in this subpatch.
10. In the "Source Audio Channel Select" section in the main patcher, select the format of the anechoic audio file (mono = 1 channel, stereo = 2 channels).
11. In the "Output Select" section in the main patcher, select the method in which you would like the final convolved audio to be outputted. This code is currently set up for a 5.1 setup (5 speakers, 1 sub), a stereo setup (2 speakers, 1 sub) or a binaural setup (for headphones).
*NOTE* the binaural setup has not been programmed, but should be easy to implement. The code package (SPAT5) used to create much of this has the ability to include HRTFs and I did not have sufficient time to explore this.
12. In the "Ambisonic Order" section in the main patcher, select the ambisonic order of the IR.
*Note* only 1st order is currently supported in this patch, but the infrastructure for 2nd and 3rd order is there. Implementation should be relatively easy, almost just a copy/paste of the "p Ambisonic_Decode_1" subpatch just with more channels.
13. In the "HOA Decoder Settings" section in the main patcher, select the desired decoding, optimization, and normalization methods. This is relatively technical, so please reference the "p HOA_Settings_Information" subpatch located just below the "HOA Decoder Settings" section in the main patcher.
14. Open the "p Speaker_Setup" subpatcher found under the "Subpatchers" section in the main patch. Ensure that speaker locations are correct and channels are routed correctly. Close this subpatcher and return to the main patcher.
15. Select "Presentation Mode".
16. To load files to begin listening, select a Receiver Location, the desired Audio Source, and the desired Room Condition. Make sure that the volume bar is up to a sufficient level.
17. There should be a separate window that loaded when the patcher opened that should now have a presentation image shown in it. Drag this to the 2nd screen designated for the presentation images and make it full screen. If you would like the border to disappear, there is a button labeled "Border" in the "Presentation Display Options" section in the main patcher (while not in presentation mode) that will remove the border so you are just left with the image.
18. Press Play and listen to the magic.


## File Structure
For this patch to reference the correct files, each of the following folders need to be added to Max's path. To do this, locate the toolbar at the top of any Max window and go to "Options" > "File Preferences...". A new window will open. Click the "+" in bottom left corner to add the file path of each folder that contains files that will be references in the patch.

Note that the folder names are not important as long as they are included in Max's path. For organizational and workflow reasons, do maintain the 4 separate folders, regardless of their names.

Folder:
- "1 Anechoic Audio" - Anechoic audio .wav files that are to be convolved with the IRs. The patch accepts either mono or stereo files. In the main patcher, select which format the audio file is on the "Source Audio Channel Select" panel.
- "2 IRs" - Impulse Response .wav files. These should be in ambisonic format of 1st (4-channels), 2nd (9-channels), or 3rd (16-channels) order. Knowing the channel ordering convention in which these IRs were encoded is critical. Accepted methods are: ACN, SID, FMH. See the "Ambisonics Channel Ordering Conventions" section in this information patch for more details.
- "3 Presentation Images" - Images (.jpg or .png - select file type in main patch "Image File Type" menu) from the viewpoint of the receiver. These are the images that are to be shown on the large presentation screen to give the perception that "this is where you are sitting" in the model.
- "4 UI Images" - Images (.png accepted only) that will be used in the user interface to overlay the interactive receiver location buttons on top of. For example, this could be a isometric view from the inside of the hall/space to orient the user to where the receiver location is in the model. In the user interface window, the receiver location buttons will be placed in locations that correspond visually to where their corresponding IRs were recorded/created in the model/space.
*.png files are only accepted because they can have transparent backgrounds (.jpg cannot), which can more seamlessly be integrated into the UI.*


## File Naming Convention
### Anechoic Audio
"___.wav"
- The anechoic audio files can be named anything. Their name will be what appears in the dropdown menu.

### Impulse Responses
"##.wav"
- 1st number = Receiver location. In the patch, this correspond to the red circle buttons with numbers on them. In CATT, this corresponds to the location of the receiver within the model.
- 2nd number = Condition. In the patch this corresponds to the drop-down menu underneath "Room Condition Select". This represents the condition of the room that the IR was recorded in (ex. curtains in vs. curtains out).

### Presentation Images
"##.jpg" or "##.png"
This numbering convention corresponds directly to the impulse response convention. For example, if the IR was recorded at location 2 in room condition 3, the name of the IR file would be "23.wav" and the name of the image file would be "23.jpg" or "23.png".

### User Interface (UI) Images
"#.png"
This numbering convention corresponds directly to the room condition. Because all IR buttons will be placed on top of the UI Image, the image will remain the same regardless of which IR receiver location is selected. The image should change with the condition, however (ex. curtains exposed vs. curtains removed). For example, if room condition 3 was selected, the name of the UI Image file would be "3.png".


## Things Left to Develop!
### 1. Higher Order Ambisonics Functionality
I am hoping that this doesn't create too much of a headache, as most of the infrastructure has already been implemented. It should be more of a copy/paste. There will be several subpatches to edit.

In the "P Convolution" subpatch all ambisonic orders are functional up until the decoding stage. To add this functionality:
- Copy the "p_Ambisonic_Decode_1" subpatch twice and title them "p_Ambisonic_Decode_2" and "p_Ambisonic_Decode_3".
- Change the "mc.receive~ Convolved_1 4" object to "mc.receive~ Convolved_2 9" (for 2nd order) or "mc.receive~ Convolved_3 16" (for 3rd order).
- Change the "mc.receive~ Mono_Stereo_HOA_Anechoic_Audio_1 4" object to "mc.receive~ Mono_Stereo_HOA_Anechoic_Audio_2 9" (for 2nd order) or "mc.receive~ Mono_Stereo_HOA_Anechoic_Audio_3 9" (for 3rd order).
- Change all objects with 4 channels to either 9 (for 2nd order), or 16 (for 3rd order) channels.
- Change any SPAT5 object's attributes that say @order 1 to either @order 2 (for 2nd order), or @order 3 (for 3rd order).
- There may be other things, but this should basically be everything for the "p_Ambisonic_Decode" subpatch.


Exit the "Convolution" subpatch and return to the main patcher. Open the "IR_Buffers" subpatch. You may be able to do this within the same patch, or it may be easier just copying the patch and creating a switch. I haven't conceptualized the implementation with this one as well.
- Follow the instruction in the red comment bubbles.
- Copy the "peek~" object to the correct amount required per each order of ambisonics (9 for 2nd order, 16 for 3rd order). Adjust naming accordingly.
- Copy the "buffer~ IR# #" objects to the correct amount required per each order of ambisonics (9 for 2nd order, 16 for 3rd order). Adjust naming accordingly.
- Connect all patch cords appropriately.


### 2. Binaural Decoding (Partially implemented)
This isn't critical, but could offer a better decoding experience for headphones. Open the "p Convolution" then "p Ambisonics_decode_1" subpatchers.

In the "Binaural" section, this should basically be a copy/paste of the Stereo speakers code, but there is a way to implement HRTFs using SPAT. You can find this in the spat5.overview patch. This is located under the "extras" toolbar menu. More research is needed here.

You should also make sure to check this with the "p Speaker_Setup" subpatch to see if any edits will need to be made there.

### 3. Other Thoughts
- Reimplement crossfade when switching between IRs to avoid the "pop"
- Play/pause & stop buttons rather than the words "play, resume, pause, stop"
- Add option for 2nd condition. Ex:
     - Occupied + Curtains
     - Occupied + No Curtains
     - Not Occupied + Curtains
     - Not Occupied+ No Curtains
- Standardize audio & IRs to 44.1kHz?
- Spectrogram (of some sort) for playback to visualize sound
- Progress bar for UI
- Integrate Alexa control with the smart lights (IFTTT)
- Make UI sleeker & cleaner
