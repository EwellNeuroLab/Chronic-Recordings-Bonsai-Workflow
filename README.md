# Chronic-Recordings-Bonsai-Workflow

Bonsai workflows for video-LFP recordings with webcamera and OpenEphys acquisition box. 
* BasicVideoLFPRecordings: video and LFP data are acquired, synchronized and saved when the workflow is stopped. Suitable for short experiments.
* ChronicVideoLFPRecordings: same as the previous, but data is saved in every hour (set in Timer node). Suitable for chronic monitoring.
* ChronicVideoLFPRecordings2Camera: same as the previous, but video is recorded on two cameras simultaneously. Output is a single video where each frame is divided into two halves - one half belongs to one camera, the other half is to the other camera.

## Dependencies. 
Bonsai Rx (https://bonsai-rx.org/)
Packages can be downloaded in Tools -> Manage Packages
* Ephys
* Vision
* Video
* Dsp Design
An additional bit file must be provided to the OE acquisition node that is available here: https://github.com/open-ephys/GUI/tree/master/Resources/Bitfiles. Then, location of the bit file must be specified within the RHD2000EvalBoard node's properties (see Bonsai Property Settings/RHD2000EvalBoardProperty.png for details).

## Channel selection

## Output file naming.

IMPORTANT ONLY when the Seizure Analysis GUI is used for data analysis. The GUI distinguishes different data types based on it's name. Therefore, timestamp file should be named as 'ts', voltage data as 'amplifier' and video as 'vid'. In the MatrixWriter/VideoWriter set the suffix to 'Timestamp'.

