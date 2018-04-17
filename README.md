# Introduction
LUT-LDPC-VHDL is a software tool that uses the optimized look-up tables designed by the the LUT-LDPC software in order to generate VHDL code and simulation files for a fully-unrolled LDPC decoder (cf. [[3-4]](#literature)).

# Quickstart
All necessary steps to download and install LUT-LDPC as well as LUT-LDPC-VHDL and perform a reference decoder design
on Ubuntu 17.10 are documented in the [Quickstart guide](https://github.com/mmeidlinger/lut_ldpc/blob/master/QUICKSTART.md).
More detailed explanations of those steps are provide in the following sections.

# Installation

## Requirements
For the generation of the VHDL code, the LUT-LDPC-VHDL software requires an installation of MATLAB. It has been tested and verified to be working with MATLAB R2016b. The simulation of the generated VHDL code with the provided testbenches requires a functioning installation of ModelSim.

## Installation
The LUT-LDPC-VHDL software is installed as part of the LUT-LDPC software tools. The installation procedure is described in detail [here](https://github.com/mmeidlinger/lut_ldpc/blob/master/README.md#installation). After the installation, the VHDL generation scripts can be found in the `lut_ldpc_vhdl` directory.

# Structure
The `lut_ldpc_vhdl` folder has the following structure
``` bash
.
├── ModelSim                        # Scripts and files for compilation and simulation with ModelSim
├── TopLevelDecoder                 # Generated VHDL files for LUT-based decoder
├── TopLevelDecoderGenerator        # VHDL generation scripts for LUT-based decoder
├── TopLevelDecoderAdders           # Generated VHDL files for baseline adder-based decoder
├── TopLevelDecoderGeneratorAdders  # VHDL generation scripts for baseline adder-based decoder
└── README.md                       # This README file
```

# Usage

## VHDL Code Generation
The VHDL generation scripts can generate fully-unrolled decoders using a standard adder-based min-sum decoding algorithm as well using optimized LUTs. The adder-based decoders are mainly meant as a baseline for a comparison with the LUT-based decoders. The VHDL generation scripts currently have a number of constraints with respect to the LUT-LDPC-VHDL software tools. Specifically:

1. Irregular LDPC codes are not supported.
2. Check node LUTs are not supported.
3. Re-using LUTs for consecutive iterations is not supported.
4. Downsizing of LUTs is not supported.

### Adder-based Decoder
The adder-based decoders are generated by the MATLAB script `TopLevelDecoderGeneratorAdders/topLevelDecoderGenerator.m`. The following parameters can be set freely (we also provide the default values)
```matlab
H_filename = '../../codes/rate0.84_reg_v6c32_N2048.alist';  % LDPC code parity-check matrix in alist format
param.QLLR = 5;                                             % Quantization bit-width for internal LLRs
param.QCh = 5;                                              % Quantization bit-width for channel LLRs
param.maxIter = 5;                                          % Maximum number of decoding iterations
```
All remaining parameters are derived from the parity-check matrix of the code. The generated VHDL files can be found in the `TopLevelDecoderAdders` folder.

### LUT-based Decoder
The LUT-based decoders are generated by the MATLAB script `TopLevelDecoderGenerator/topLevelDecoderGenerator.m`. Unlike the adder-based generation script, the LUT-based generation script is not stand-alone in the sense that a decoder has to be designed first using the LUT LDPC C++ libraries. An example of this procedure can be found  [here](https://github.com/mmeidlinger/lut_ldpc/blob/master/README.md#designing-lut-decoders-and-testing-bit-error-rate-performance). The following parameter can be set freely (we also provide the default value)
```matlab
param.lut_codec_filename = '../../results/RES_N2048_R0.841309_maxIter8_zcw1_frames100_minLUT/lut_codec.it';  % Codec filename generated by the C++ program
```
All remaining parameters are derived from the parity-check matrix of the code and the remaining loaded files. The generated VHDL files can be found in the `TopLevelDecoder` folder.

## VHDL Code Simulation
The compilation scripts for the LUT-based decoder and adder-based decoder (`compile.sh`) are provided in `ModelSim/BIN_Decoder` and `ModelSim/BIN_DecoderAdder`. Once the HDL files have been compiled into the work library, the simulation is run by running the scrips (`run.sh`), provided in simillar directories, as 'source run.sh'. Note that you need to update the variables: VCOM, VLIB, VSIM, in the scripts according to your ModelSim installation.
The in/out stimulis are provided in `ModelSim/OUT` directory.

# Referencing
If you use this software for your academic research, please consider referencing our original contributions [[1,2,3,4]](#literature).

# Literature
[[1] M. Meidlinger, A. Balatsoukas-Stimming, A. Burg, and G. Matz, “Quantized message passing for LDPC codes,” in Proc. 49th Asilomar Conf. Signals, Systems and Computers, Pacific Grove, CA, USA, Nov. 2015.](http://ieeexplore.ieee.org/document/7421419/)

[[2] M. Meidlinger and G. Matz, “On irregular LDPC codes with quantized message passing decoding,” in Proc. IEEE SPAWC 2017, Sapporo, Japan, Jul. 2017.
](http://ieeexplore.ieee.org/document/8227780/)

[[3]  A. Balatsoukas-Stimming, M. Meidlinger, R. Ghanaatian, G. Matz, and A. Burg, “A fully-unrolled LDPC decoder based on quantized message passing,” in Proc. SiPS 2015, Hang Zhou, China, 10 2015.
](http://ieeexplore.ieee.org/abstract/document/7345024/)

[[4] R. Ghanaatian, A. Balatsoukas-Stimming, C. Mu ̈ller, M. Meidlinger, G. Matz, A. Teman, and A. Burg, “A 588 Gbps LDPC decoder based on finite-alphabet message passing,” IEEE Trans. VLSI Systems, vol. 26, no. 2, pp. 329–340, 2 2018.
](http://ieeexplore.ieee.org/document/8113527/)
