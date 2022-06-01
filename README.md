# Chronic-Recordings-Bonsai-Workflow

Bonsai workflows for video-LFP recordings with webcamera and OpenEphys acquisition box. 
* BasicVideoLFPRecordings: video and LFP data are acquired, synchronized and saved when the workflow is stopped. Suitable for short experiments.
* ChronicVideoLFPRecordings: same as the previous, but data is saved in every hour (set in Timer node). Suitable for chronic monitoring.
* ChronicVideoLFPRecordings2Camera: same as the previous, but video is recorded on two cameras simultaneously. Output is a single video where each frame is divided into two halves - one half belongs to one camera, the other half is to the other camera.

## Dependencies 
Bonsai Rx (https://bonsai-rx.org/)
Packages can be downloaded in Tools -> Manage Packages
* Ephys
* Vision
* Video
* Dsp Design
An additional bit file must be provided to the OE acquisition node that is available here: https://github.com/open-ephys/GUI/tree/master/Resources/Bitfiles. Then, location of the bit file must be specified within the RHD2000EvalBoard node's properties (see Bonsai property settings section).

## Channel selection
When channels are selected in Bonsai, the channel numbers needs to be converted from the Open Ephys channel map. With a 16-channels adapter board the following channel numbers are available.
Port A: 8-23 (Open Ephys channel 1 = Bonsai channel 8, Open Ephys channel 16 = Bonsai channel 23)
Port B: 40-55 (same as on Port A, but shifted with 32)
Port C: 72-87 (same, shifted with 32)
Port D: 104-119 (same, shifted with 32)

When 32 channels or higher are used, no conversion is needed (tested with 64-channel amplifier).

## Output file naming

When the Seizure Analysis GUI is used to analyze data, extremely important to set the video/timestamp/voltage file names according to the instructions in the Bonsai property settings section (VideoWriter & MatrixWriter).


## Bonsai property settings
Bonsai nodes are parametrized by setting their properties in the Property panel. Many of the settings are fixed and do not require to be changed, but some of them requires modifications by the user. First, settings that requires these nodes are listed. 

**_VideoCaptureDevice_**
*Set this when more than one camera is connected to the computer.* Cameras will be listed here and user have to select the one to use for video acquisition. In this example, we only had one camera connected (labelled with index 0).

![image](https://user-images.githubusercontent.com/94412124/171449218-e4c107af-69fc-49d3-8633-6a989563b79f.png)

**_Rhd2000EvalBoard_**
1. Go to the BitFileName field and click on the (...). Next, a file browser pops up. Select the bit file you've downloaded. In this example we used the rhd2000_usb3.bit renamed to main.bit.
2. Set sampling frequency in the SampleRate field.

![image](https://user-images.githubusercontent.com/94412124/171454894-93697513-9513-4da6-b3c3-2d95dce306df.png)

**_SelectChannels_**

Type the channel numbers one by one that you are interested in. In this example, we selected 8 channels (2 channel from 4 mice).

![image](https://user-images.githubusercontent.com/94412124/171457983-563d9cd9-575c-4def-a33e-f1084acd4376.png)

**_Timer_**

Set the DueTime property to the time when you would like to haver your data saved. In this example, we set DueTime to 1:00:00 and therefore data is saved in every hour.

![image](https://user-images.githubusercontent.com/94412124/171460182-914ed102-94cc-42b9-898e-96b713e4d839.png)


**_VideoWriter_**

1. Make sure Buffered is set to True.
2. Click on the (...) in the FileName property. In the pop-up browser, select the folder where you want to save your data. Specify file name as 'vid.avi'. **Having this specific file name is extremely important if the data will be analyzed in the Seizure Analysis GUI.** Note: don't forget to type the .avi extension, otherwise no video file will be generated.
3. To ensure that each file has a unique name, set Suffix property. **Extremely important to set it to Timestamp if the data will be analyzed in our GUI.**
Example of resulting video file name: 'vid2022-02-16T13_02_17.avi' 

![image](https://user-images.githubusercontent.com/94412124/171464450-905f8a88-88d9-4ded-ba1c-cbb027c815d6.png)



**_MatrixWriter_**

This step needs to be done twice - once for the timestamps, once for the amplifier data. 
1. Click on the (...) in the Path property. In the pop-up browser, select the folder where you want to save your data. Specify timestamp file name as 'ts.bin' and voltage file name as 'amplifier.bin'. **Having this specific file name is extremely important if the data will be analyzed in the Seizure Analysis GUI.** Note: don't forget to type the .bin extension.
2. To ensure that each file has a unique name, set Suffix property. **Extremely important to set it to Timestamp if the data will be analyzed in our GUI.**
Example of resulting timestamp file name: 'ts2022-02-16T13_02_17.bin'.
Example of resulting voltage file name: 'amplifier2022-02-16T13_02_17.bin'.


![image](https://user-images.githubusercontent.com/94412124/171470133-fc7fe75a-2445-41d1-bbf0-e568822ac92e.png)

### Advanced settings

The following settings are listed for the sake of completeness. The user is not required to change on these settings but may give a full insight how the workflow works.

*Disabling auto-exposure of camera* 

Important to maintain constant frame acquisition rate. Go to the Property Panel of VideoCaptureDevice Click on the (...) in the CaptureProperties field. The following window pops-up.Exposure variable can be added by clicking on the Add button. Setting the ControlFlags to Manual and Value to -5 disables auto-exposure, maintaining a constan frame acqusition rate.

![image](https://user-images.githubusercontent.com/94412124/171451871-9f3b1e03-8a66-40bb-a838-ec3008bb2b4b.png)


*Extracting voltage and timestamp data from RHD2000EvalBoard node*

Right click on the RHD2000EvalBoard node. Select the first item in the list - Output. A list of data fields appears. Click on the AmplifierData/Timestamp to create nodes for the voltage/timestamp data.

![image](https://user-images.githubusercontent.com/94412124/171456707-badb6d05-7cf7-405e-8538-f2971eeee5d8.png)

*ScalarBuffer*

Format and length-matching vectors of the video timestamps are constructed with the ScalarBuffer.
1. Set the Depth to S32 (signed 32-bit integer, data format of the Open Ephys timestamp)
2. Set Size to 256,1 (data arrives in 256 packets from the acquisition board)
3. Set the 4 Value to 0 (it's an arbitrary initialization, the Value parameters will be dynamically changed once the video acquisition starts)

![image](https://user-images.githubusercontent.com/94412124/171472265-bdd613b7-4db4-4332-9a7c-71da4838273f.png)

*InputMapping*

This operator ensures that the ScalarBuffer's Value is dynamically updated with the video timestamp, whenever a frame is acquired.
1. Click on the (...) in the PropertyMappings field.
2. In the pop-up PropertyMapping Collection Editor, click on the Add button.
3. On the right-side in the Name field, select 'Value' from the list.

![image](https://user-images.githubusercontent.com/94412124/171474684-3cb25c1a-4881-4e65-9625-81072f4600f8.png)


5. Underneath, click on the (...)  in the Selector fields. Select the Index and click on the right arrow (top button) 4 times. Press Ok.

![image](https://user-images.githubusercontent.com/94412124/171474580-bafc5ee2-58f0-4812-8ec1-f759be86f892.png)

6. Press Ok again. If the setting was successful, you should see the following in the Property panel.

![image](https://user-images.githubusercontent.com/94412124/171475037-b078260e-615b-454f-b870-1894bb8971d2.png)








