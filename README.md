# Chronic-Recordings-Bonsai-Workflow

Bonsai workflows for video-LFP recordings with webcamera and OpenEphys acquisition box. 
* BasicVideoLFPRecordings: video and LFP data are acquired, synchronized and saved when the workflow is stopped. Suitable for short experiments.
* ChronicVideoLFPRecordings: same as the previous, but data is saved in every hour (set in Timer node). Suitable for chronic monitoring.
* ChronicVideoLFPRecordings2Camera: same as the previous, but video is recorded on the camera simultaneously. Output is a singe video where each frame is divided into two halves - one half belongs to one camera, the other half is to the other camera.
