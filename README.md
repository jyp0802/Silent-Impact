# Silent Impact

This is the official repository of the paper
\
**"Silent Impact: Tracking Tennis Shots from the Passive Arm"**

https://doi.org/10.1145/3654777.3676403



## Dataset

The dataset is categorized into two main components: **Classification** and **Detection**. Both categories contain data from IMUs on the dominant and passive arms of participants.

### Download
The dataset can be downloaded [here](https://drive.google.com/drive/folders/1CUFZEkLSs6bZLg77lnytApPgXcVlJlAf?usp=sharing).
Place the 4 `pkl` files in the `data` folder so that it looks like this:
```
Silent-Impact/
    data/
        Classification_Dominant.pkl
        Classification_Passive.pkl
        Detection_Dominant.pkl
        Detection_Passive.pkl
        Participant_Information.json
    README.md
    ...
```

### Classification Data

#### Files:
- **Dominant Arm**: `Classification_Dominant.pkl`
- **Passive Arm**: `Classification_Passive.pkl`

#### Description & Data Structure:
The classification array contains 6,000 shots across 6 different shot types, performed by 20 participants. Each entry in this array represents a sequence of sensor readings across 6 channels and spans 180 frames. At the beginning of each entry, you'll find the shot type label (ranging from 0 to 5) and the participant's ID.

- **Array Shape**: `[number_of_items, meta_data_size + window_length, sensor_channels]`

    - **number_of_items**: 6,000  
        (Calculated as: 20 participants × 6 shot types × 50 shot instances)
    
    - **meta_data_size**: 1  
        - Index 0: Shot type label  
            - Labels:  
              0 - Forehand Stroke  
              1 - Backhand Stroke  
              2 - Forehand Volley  
              3 - Backhand Volley  
              4 - Smash  
              5 - Serve
        - Index 1: Participant ID  

        > For instance, `classification_array[:, 0, 0]` returns the shot type of each entry.
    
    - **window_length**: 180
    
    - **sensor_channels**: 6  
        - 3 channels: X/Y/Z linear acceleration
        - 3 channels: X/Y/Z angular velocity

### Detection Data

#### Files:
- **Dominant Arm**: `Detection_Dominant.pkl`
- **Passive Arm**: `Detection_Passive.pkl`

#### Description & Data Structure:
The detection array provides sensor readings from tennis sessions, including rallies and casual matches, from 10 participants. Every entry in this array features a 6-channel sensor reading sequence, accompanied by a shot/no-shot label for each frame. As the sessions vary in length, the array's length matches the longest session. Each entry starts with the length of its respective sequence and the ID of the associated participant.

- **Array Shape**: `[number_of_items, meta_data_size + longest_sequence_length, sensor_channels + frame_label_size]`

    - **meta_data_size**: 1  
        - Index 0: Sequence length  
        - Index 1: Participant ID  

        > For instance, `detection_array[:, 0, 0]` returns the length of each sequence.
    
    - **sensor_channels**: 6  
        - 3 channels: X/Y/Z linear acceleration
        - 3 channels: X/Y/Z angular velocity

    - **frame_label_size**: 1  
        - 0: No shot  
        - 1: Shot

### Subject information
Demographics and tennis characteristics of each subject is provided in the `Participant_Information.json` file.

## Code
Tested with python==3.7.16, pytorch==1.13.0, cudatoolkit=11.6

### Dependencies
```
conda create -n silentimpact python=3.7
conda activate silentimpact
conda install pytorch==1.13.0 torchvision==0.14.0 torchaudio==0.13.0 pytorch-cuda=11.6 -c pytorch -c nvidia
pip install -r requirements.txt
```

### Classification training
```
python train_classification.py --arm p --name test --split 1 --gpu 0
```

### Detection training
```
python train_detection.py --arm p --name test --split 1 --gpu 0
```


## Citation
If you use the dataset or code provided in this work, please cite us:
```
@inproceedings{park2024silent,
  title={Silent Impact: Tracking Tennis Shots from the Passive Arm},
  author={Park, Junyong and Yang, Saelyne and Jo, Sungho},
  booktitle={Proceedings of the 37th Annual ACM Symposium on User Interface Software and Technology},
  pages={1--15},
  year={2024}
}
```
