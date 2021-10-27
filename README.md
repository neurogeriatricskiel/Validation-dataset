# Validation Dataset

*Short description*  
First few participants from a full-body dataset with inertial measurement units (IMUs) and optical motion capture data.

When using this dataset, please cite:
> Warmerdam E, Romijnders R, Geritz J, Elshehabi M, Maetzler C, Otto JC, Reimer M, Stuerner K, Baron R, Paschen S, Beyer T, Dopcke D, Eiken T, Ortmann H, Peters F, Recke Fvd, Riesen M, Rohwedder G, Schaade A, Schumacher M, Sondermann A, Maetzler W, Hansen C. Proposed Mobility Assessments with Simultaneous Full-Body Inertial Measurement Units and Optical Motion Capture in Healthy Adults and Neurological Patients for Future Validation Studies: Study Protocol. Sensors. 2021; 21(17):5833. https://doi.org/10.3390/s21175833

## Introduction
This is a mobility dataset for the development and validation of IMU-based algorithms. The dataset contains several walking trials, clinical assessments and daily living activities performed in the lab. Healthy participants as well as neurological patients (Parkinson's disease (PD), multiple sclerosis (MS), stroke, chronic low back pain) will be included in the dataset. The data collection is ongoing. In the data folder you find the data of the first 10 participants (some of the larger files are missing, since they were too large to be uploaded), more data can be made available on request.

The data of the first five healthy young adults (18-60 years; pp001-pp005) and the first five healthy older adults (>60 years; pp006-pp010) are made available. Each participant folder contains the data of the IMUs and the optical motion capture system in seperate folders.

Information about the **IMU and marker placement** as well as the performed assessments can be found in the study protocol paper (https://www.mdpi.com/1424-8220/21/17/5833).

## Data Organization
### Folder Structure
The data are organized as follows:

    ├── data                    # parent folder
    │   ├── pp001               # for each participant, a folder with the participant id
    │   ├── pp002
    │   ├── pp003               # within this folder, data are organized in folders for IMU and OMC data
    |       └── imu
    |           ├── imu_backwards.mat
    |           ├── imu_balance_sbs.mat
    |           ├── ...
    |           └── imu_walk_turn.mat
    |       └── optical
    |           ├── omc_backwards.mat
    |           ├── omc_balance_sbs.mat
    |           ├── ...
    |           └── omc_walk_turn.mat
    │   ├── ... 
    │   └── pp010
    └── ...

### Data Structure
The data are stored as MATLAB .mat files, can be read using, for instance:
```matlab
>>> load('filename.mat')
```
This reads the data into a `1x1 struct` named `data` that consists of the following fields:
- **IMU data**
  - `calibration_file`: str, or char; which calibration file is associated with the current data
  - `acc`: Nx3xM double; accelerometer data (in *g*) with:
    - N -> number of discrete time steps, 
    - 3 -> number of sensing axes (X, Y, and Z),
    - M -> number of IMUs
  - `gyro`: Nx3xM double; gyroscope data (in degrees/s) with N discrete time steps, 3 directions and M number of IMUs
  - `magn`: Nx3xM double; magnetometer data (in Gauss) with N discrete time steps, 3 directions and M number of IMUs
  - `fs`: double; sampling frequency (in Hz)
  - `imu_location`: 1xM cell; cell array with the positions of the IMUs (see: above)
- **OMC data**
  - `calibration_file`: str, or char; which calibration file is associated with the current data
  - `pos`: Nx4xM double; marker position data (in mm) with N discrete time steps, dimensions that holds the uncertainty (standard deviation of the cameras?) and M number of reflective markers
    - N -> number of discrete time steps,
    - 4 -> number of coordinate axes (X, Y, Z) + uncertainty
    - M -> number of markers
  - `fs`: double; sampling frequency (in Hz)
  - `marker_location`: 1xM cell; cell array with the positions of the reflective markers (see: above)

Then, to get the marker position data from a given marker, one could run for example:
```matlab
>>> marker_label = 'l_heel';
>>> l_heel_pos = data.pos(:,1:3,contains(data.marker_location, marker_label));
>>> r_heel_pos = data.pos(:,1:3,contains(data.marker_location, 'r_heel'));
>>> figure('DefaultAxesFontSize', 16);
>>> plot(l_heel_pos(:,3), 'b', 'LineWidth', 3); hold on; grid minor; box off;
>>> plot(r_heel_pos(:,3), 'r', 'LineWidth', 3);
>>> legend({'left heel', 'right heel'});
>>> xlabel('time (in samples)');
>>> ylabel('Z position (in mm)');
```



## Contact


If you would like to have the demographics and clinical scores of the participants, or you would like to have more data for your research, please get in touch with us (Elke Warmerdam: e.warmerdam@neurologie.uni-kiel.de & Clint Hansen: c.hansen@neurologie.uni-kiel.de).
