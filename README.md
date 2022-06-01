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
An additional bit file must be provided to the OE acquisition node that is available here: https://github.com/open-ephys/GUI/tree/master/Resources/Bitfiles. Then, location of the bit file must be specified within the RHD2000EvalBoard node's properties (see Bonsai Property Settings/RHD2000EvalBoardProperty.png for details).

## Channel selection
When channels are selected in Bonsai, the channel numbers needs to be converted from the Open Ephys channel map. With a 16-channels adapter board the following channel numbers are available.
Port A: 8-23 (Open Ephys channel 1 = Bonsai channel 8, Open Ephys channel 16 = Bonsai channel 23)
Port B: 40-55 (same as on Port A, but shifted with 32)
Port C: 72-87 (same, shifted with 32)
Port D: 104-119 (same, shifted with 32)

When 32 channels or higher are used, no conversion is needed (tested with 64-channel amplifier).

## Output file naming

IMPORTANT ONLY when the Seizure Analysis GUI is used for data analysis. The GUI distinguishes different data types based on it's name. Therefore, timestamp file should be named as 'ts', voltage data as 'amplifier' and video as 'vid'. In the MatrixWriter/VideoWriter set the suffix to 'Timestamp'. The timestamp will be used in the GUI to determine the recording date of the files.
Tip: when the FileName is set for MatrixWriter/VideoWriter, make sure you add the extension after the name to get a usable file. In this case: 'ts.bin', 'amplifier.bin', 'vid.avi'. The timestamp is added automatically by Bonsai.

Example for final file names with Timestamp suffix:
'ts2022-02-16T13_02_17.bin'
'amplifier2022-02-16T13_02_17.bin'
'vid2022-02-16T13_02_17.avi'

## Bonsai property settings
Bonsai nodes are parametrized by setting their properties in the Property panel. Many of the settings are fixed and do not require to be changed, but some of them requires modifications by the user. First, settings that requires the user's attention are listed. 

**_VideoCaptureDevice_**
*Set this when more than one camera is connected to the computer.* Cameras will be listed here and user have to select the one to use for video acquisition.

![image](https://user-images.githubusercontent.com/94412124/171449218-e4c107af-69fc-49d3-8633-6a989563b79f.png)

**_Rhd2000EvalBoard_**
1. Go to the BitFileName field and click on the (...). Next, a file browser pops up. Select the bit file you've downloaded. (In this example we used the rhd2000_usb3.bit renamed to main.bit).
2. Set sampling frequency in the SampleRate field.

![image](https://user-images.githubusercontent.com/94412124/171454894-93697513-9513-4da6-b3c3-2d95dce306df.png)


*Extracting voltage and timestamp data from RHD2000EvalBoard node*

Right click on the RHD2000EvalBoard node. Select the first item in the list - Output. A list of data fields appears. Click on the AmplifierData/Timestamp to create nodes for the voltage/timestamp data.

![image](https://user-images.githubusercontent.com/94412124/171456707-badb6d05-7cf7-405e-8538-f2971eeee5d8.png)


*Disabling auto-exposure* ()

Go to the Property Panel of VideoCaptureDevice Click on the (...) in the CaptureProperties | (Collection) field. The following window pops-up.Exposure variable can be added by clicking on the Add button. Setting the ControlFlags to Manual and Value to -5 disables auto-exposure, maintaining a constan frame acqusition rate.

![image](https://user-images.githubusercontent.com/94412124/171451871-9f3b1e03-8a66-40bb-a838-ec3008bb2b4b.png)






