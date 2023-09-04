# paderwasn
Paderwasn is a collection of methods for acoustic signal processing in wireless acoustic sensor networks (WASNs).

## Installation
Install requirements:
```bash
$ pip install --user git+https://github.com/fgnt/lazy_dataset.git@ce8a833221580242e69d43e62361adca02478f79
$ pip install --user git+https://github.com/fgnt/paderbox.git@7fed5b44be2effcedb7a26778ada6c5668b1d6bd
```

Clone the repository:
```bash
$ git clone https://github.com/fgnt/paderwasn.git
```

Install package:
```bash
$ pip install --user -e paderwasn
```

## Content
* Algorithms:
    + [Geometry calibration](paderwasn/geometry_calibration):
        + Geometry calibration using iterative data set matching [1]
        + GARDE-algorithm [2]
    + [Signal synchronization](paderwasn/synchronization):
        + Sampling rate offset (SRO) estimation:
            + Dynamic weighted average coherence drift (WACD) [3]
            + Onlne WACD [4]
        + Sampling time offset (STO) estimation [3]
        + Resampling to compensate for an SRO
        + Simulation of a (time-varying) SRO [3]
    + [Source extraction in ad-hoc acoustic sensor networks via beamforming](paderwasn/source_extraction):
        + Integrated sampling rate synchronization and acoustic beamforming [5]
    
* Databases:
    + [Geometry calibration observations](paderwasn/databases/geometry_calibration): Collection of direction-of-arrival (DoA) and source-node distance
        estimates used for geometry calibration in [1]
    + [Asynchronous WASN database](paderwasn/databases/synchronization): Database of simulated audio signals which were recorded by an
        asynchronous WASN. This database corresponds to the database (after
        minimal adjustments) used in [3] for evaluation of signal
        synchronization algorithms. 
* [Experiments](paderwasn/experiments) using the provided algorithms and databases:
    + Comparision of geometry calibration methods
    + Comparision of SRO methods
    + STO estimation
   
## Asynchronous WASN database 
The ansynchronous WASN database consists of simulated recordings of a
asynchronous WASN with four sensor nodes. The database corresponds to a
slightly modified version of the database used in [3] (Source signals stemming
from the Timit datbase were replaced by signals stemming from the LibriSpeech
database). Four scenarios are simulated (see [3] for details):

| Scenario | Time-varying SRO | Multiple Source Positions | Speech Pauses |
| :-----------: | :-----------: |  :-----------: |  :-----------: |
| Scenario-1  | | | |
| Scenario-2  | X | | |
| Scenario-3  | X | X | X |
| Scenario-4  | X | X | |

To prepare the databse follow these steps:
1. Download the room impulse responses (RIRs), generated by the generator of
[Habets](https://github.com/ehabets/RIR-Generator). using this
[python port](https://github.com/boeddeker/rirgen), SRO trajectories (see [3])
and simulation descriptions:
    ```bash 
    $ python -m paderwasn.databases.synchronization.download with 'database_path="/PATH/WHERE/TO/STORE/THE/DATABASE/"'
    ```
    If you do not have downloaded the LibriSpeech database (test-clean) before
    download the test-clean part of LibriSpeech:
    ```bash 
    $ python -m paderwasn.databases.synchronization.download with 'database="librispeech"' 'database_path="/PATH/WHERE/TO/STORE/THE/DATABASE/"'
    ```
1. Create a json-file for the database:
    ```bash 
    $ python -m paderwasn.databases.synchronization.create_json with 'database_path="/PATH/TO/THE/DATABASE/"' 'librispeech_path="/PATH/TO/THE/ROOT/OF/LIBRISPEECH/"' 'json_path="/PATH/WHERE/TO/STORE/THE/DB_JSON/"'
    ```
1. Create a file-based version of the database, i.e. simulate the audio signals,
    store the audio signals on the disk and create a new json-file for the
    file-based version of the database:
    ```bash 
    $ python -m paderwasn.databases.synchronization.write_files with 'json_path="/PATH/TO/THE/DB_JSON/"' 'data_root="/PATH/WHERE/TO/STORE/THE/FILE_DB/"' 'json_file_db_path="/PATH/WHERE/TO/STORE/THE/FILE_DB_JSON/"'
    ```

## References
[1] Gburrek, T., Schmalenstroeer, J., Haeb-Umbach, R.: "Geometry Calibration in
Wireless Acoustic Sensor Networks Utilizing DoA and Distance Information", 
EURASIP Journal on Audio, Speech, and Music Processing, vol. 2021, no. 1,
pp. 1–17, 2021.

[2] Gburrek, T., Schmalenstroeer, J., Haeb-Umbach, R.: "Iterative Geometry
Calibration from Distance Estimates for Wireless Acoustic Sensor Networks". in
Proc. IEEE International Conference on Acoustics, Speech and Signal Processing
(ICASSP), 2021, pp. 741-745.

[3] Gburrek, T., Schmalenstroeer, J., Haeb-Umbach, R.: "On Synchronization of
Wireless Acoustic Sensor Networks in the Presence of Time-varying Sampling Rate
Offsets and Speaker Changes," in Proc. IEEE International Conference on
Acoustics, Speech and Signal Processing (ICASSP), 2022.

[4] Chinaev, A., Enzner, G., Gburrek, T., Schmalenstroeer, J.:
“Online Estimation of Sampling Rate Offsets in Wireless Acoustic Sensor
Networks with Packet Loss,” in Proc. 29th European Signal Processing Conference
(EUSIPCO), 2021, pp. 1–5.

[5]  Gburrek, T., Schmalenstroeer, J., Haeb-Umbach, R.:
“Online Estimation of Sampling Rate Offsets in Wireless Acoustic Sensor
Networks with Packet Loss”. in Proc. European Signal Processing Conference
(EUSIPCO), 2023.

## Citation
If you are using the code or one of the provided databases please cite the
corresponding paper (If you use the asynchronous WASN database please cite [3]):
 ```
@article{gburrek2021geometry,
	title={Geometry calibration in wireless acoustic sensor networks utilizing DoA and distance information},
	author={Gburrek, Tobias and Schmalenstroeer, Joerg and Haeb-Umbach, Reinhold},
	journal={EURASIP Journal on Audio, Speech, and Music Processing},
	volume={2021},
	number={1},
	pages={1--17},
	year={2021},
	publisher={Springer}
}
```
 ```
@inproceedings{gburrek2021garde, 
    author={Gburrek, Tobias and Schmalenstroeer, Joerg and Haeb-Umbach, Reinhold}, 
    booktitle={IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP)},
    title={Iterative Geometry Calibration from Distance Estimates for Wireless Acoustic Sensor Networks},
    year={2021},
    pages={741-745},
    doi={10.1109/ICASSP39728.2021.9413831}
}
```
 ```
@inproceedings{gburrek2022,
    author={Gburrek, Tobias and Schmalenstroeer, Joerg and Haeb-Umbach, Reinhold},
    booktitle={ICASSP 2022 - 2022 IEEE International Conference on Acoustics, Speech and Signal Processing (ICASSP)}, 
    title={On Synchronization of Wireless Acoustic Sensor Networks in the Presence of Time-Varying Sampling Rate Offsets and Speaker Changes}, 
    year={2022},
    volume={},
    number={},
    pages={916-920},
    doi={10.1109/ICASSP43922.2022.9746284}
}
```
 ```
@inproceedings{Chinaev2021,
	author = {Chinaev, Aleksej and Enzner, Gerald and Gburrek, Tobias and Schmalenstroeer, Joerg},
	booktitle = {29th European Signal Processing Conference (EUSIPCO)},
	pages = {1--5},
	title = {{Online Estimation of Sampling Rate Offsets in Wireless Acoustic Sensor Networks with Packet Loss}},
	year = {2021},
}
 ```

 ```
@inproceedings{Gburrek23,
    author={Gburrek, Tobias and Schmalenstroeer, Joerg and Haeb-Umbach, Reinhold},
	booktitle = {31st European Signal Processing Conference (EUSIPCO)},
	pages = {1--5},
	title = {{On the Integration of Sampling Rate Synchronization and Acoustic Beamforming}},
	year = {2023},
}
 ```

## Acknowledgment
Funded by the Deutsche Forschungsgemeinschaft (DFG, German Research
Foundation) - Project 282835863 ([Deutsche Forschungsgemeinschaft - 
DFG-FOR 2457](https://www.uni-paderborn.de/asn/)).

![img](https://www.uni-paderborn.de/fileadmin/_processed_/9/2/csm_ASNLogo_c443ce161b.png)
