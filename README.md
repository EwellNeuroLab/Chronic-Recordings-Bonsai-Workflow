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
