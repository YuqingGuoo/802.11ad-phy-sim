# 802.11ad-phy-sim
This repository contains the baseband model of the IEEE802.11ad physical layer (PHY) simulator created in MATLAB 2017b. It only supports the Single Carrier PHY (SC-PHY) specification. To run this simulator, the following MATLAB toolboxes are required: Control System Toolbox, Signal Processing Toolbox, DSP System Toolbox and Communications System Toolbox. 

All m- or mex- files are made entirely by authors, excepting the Soft-sphere decoder implementation which was originally developed at Vienna University of Technology within the LTE Link Level Simulator, see: http://publik.tuwien.ac.at/files/PubDat_199104.pdf

If the use of this simulator leading to a scientific publication, please, cite our work that can be found at the end of this page.
    
## Short description
Main blocks of the transmitter (TX) part are as follows:

***Data:*** Generation of a random bit streams (bits).

***Scrambler:*** It is used to break up long sequences of ones and zeros. It is defined by generator polynomial: ![](https://user-images.githubusercontent.com/55983849/67189872-4dd44100-f3ef-11e9-81b8-0ccb541a7f4e.png).

***LDPC encoder:*** Forward error correction (FEC) scheme for SC-PHY can use 5 code rates: 1/2, 5/8, 3/4, 13/16 and 7/8.

***Bit interleaver:*** For interleaving of the encoded message (on a bit level).

***Modulation:*** ![](https://user-images.githubusercontent.com/55983849/67190065-955acd00-f3ef-11e9-8b52-6e7455de55ab.png)-shifted BPSK, QPSK, 16QAM and 64QAM constellations.

***Symbol blocking:*** The data are transmitted block-wise at 448 symbols per block.

***Guard interval (GI):*** Another 64 symbols are inserted between the individual blocks. The GI consists of a Golay sequence, marked as Ga64, modulated with ![](https://user-images.githubusercontent.com/55983849/67190065-955acd00-f3ef-11e9-8b52-6e7455de55ab.png)-BPSK. Finally, the complete IEEE 802.11ad frame is created.

***Channel:*** Block *Channel* allows for user to select between the following channel models:
- AWGN channel model (**'awgn'**),
- fading channel model using user-defined values (**'fad'**),
- a measured indoor 60 GHz fading channel model (**'fad_meas'**).

*The *'fad_meas'* option uses a measured dataset from the indoor measurement campaign held in Brno University of Technology (BUT) at the Department of Radio Electronics (DREL). We provide both channel impulse responses and channel transfer functions of a time invariant indoor channel with a 10 GHz bandwidth spanning the frequencies from 55 GHz to 65 GHz. The data are in .mat format and is easily utilizable in MATLAB. This could serve as a database oriented channel model, from which an individual channel realizations are loaded and utilized, e.g. in an algorithm simulation. The measured data are free to use, however if leading to a scientific publication, please cite our work, which led to acquiring this dataset. The relevant publication is:* 

> P. Liu, J. Blumenstein, N. S. Perović, M. Di Renzo and A. Springer, "Performance of Generalized Spatial Modulation MIMO Over Measured 60GHz Indoor Channels," in IEEE Transactions on Communications, vol. 66, no. 1, pp. 133-148, Jan. 2018. doi: 10.1109/TCOMM.2017.2754280 URL: http://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=8046024&isnumber=8258580

*Note:* An accurate channel estimation is crucial for signal reception in wideband communication systems. For this purpose, IEEE 802.11ad employs a Channel Estimation Field (CEF) in each packet, composed from Golay sequences of length 128 samples (Ga128, Gb128). At the receiving side (RX), the channel characteristics is estimated using a correlation computation.

## Instructions for use

### IEEE 802.11ad PHY simulator
The basic simulation is started by the file *'wifi_sim_batch_AD.m'*. The parameters that can be set are as follows:
- **'decision_type'** - either *'LLR'* (Log-likelihood ratio) or *'HD'* (Hard decision),
- **'LENGTH'** - user data octet length (within the range from 1 to 262144),
- **'MCSvec'** - the MCS value(s) to simulate MCSvec, either *'all'* (corresponding to MCS defined for SC-PHY) or individual MCS value(s),
- **'channelType'** - *'awgn'*, *'fad'* or *'fad_meas'*,
- **'SNR'** - simualated SNR values (in dB) in channel model, within the outer loop,
- **'N_frames'** - number of simulated frames within the inner loop (for each SNR value).

The results are stored in the *'\results'* folder as a .mat file. Each simulated MCS has its own .mat file. For the results graphical presentation you can use the file: *'\results\present_results_wifi_AD.m'*. 

![IEEE 802.11ad SC-PHY simulation results example](https://user-images.githubusercontent.com/55983849/67470120-8ec99100-f64d-11e9-9741-ad3fdc5e8a27.png)

### Measured indoor 60 GHz fading channel model only
To use and present the measured dataset from the indoor measurement campaign held in Brno University of Technology (BUT) at the Department of Radio Electronics (DREL) employs the file show_meas_data.m stored in the folder *'\measured_chanels'*:. You will receive several characteristics of the transmission channels in time and frequency domain, see the article on this issue mentioned above.

![Examples of time- and frequency-domain channel traces](https://user-images.githubusercontent.com/55983849/67467406-f7623f00-f648-11e9-8acd-8eacbe66e315.png)


## Please cite our paper
If you use the 802.11ad simulator, please, cite:

    @INPROCEEDINGS{BlumensteinNorCAS2019,
    author  = {Blumenstein, J. and Milos, J. and Polak, L. and Mecklenbräuker, Ch.},
    title   = {{IEEE} 802.11ad {SC}-{PHY} {Layer} {Simulator}: {Performance} in {Real}-world 60 {GHz} {Indoor} {Channels}},
    booktitle = {2019 IEEE Nordic Circuits and Systems Conference},
    year    = {2019},
    publisher = {IEEE},
    pages   = {1--4},
    month   = {Oct},
    doi     = {10.1109/NORCHIP.2019.8906960}
    }
  
The paper is available via IEEE Explore at: https://ieeexplore.ieee.org/document/8906960

## Acknowledgement

*This work was supported by the Ministry of Education, Youth and Sports (MEYS) of the Czech Republic project no. [LTC18021](https://starfos.tacr.cz/en/project/LTC18021) (FEWERCON). For research, infrastructure of the [SIX Center](http://www.six.feec.vutbr.cz/) was used.*

## License
The presented simulator is available under the MIT License as expressed below, excepting files LTE_rx_soft_sd2.mexw32, 	LTE_rx_soft_sd2.mexw64, and LTE_softsphere.m that are licensed by INTHFT (see www.nt.tuwien.ac.at).

***MIT License***

***Copyright (c) [2019] [Department of Radio Electronics, Brno University of Technology]***

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
