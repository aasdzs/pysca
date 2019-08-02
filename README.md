# Windows 설치
 * 환경구축 편의를 위해 WinPython 설치 (해당 코드는 python2 구현) 
   - https://sourceforge.net/projects/winpython/files/WinPython_2.7/
 * 기본 예제를 돌리기 위한 파형(swaes_atmega_power.trs)을 다운받기 위해서는 Git-LFS 설치 필요
   - https://git-lfs.github.com/
   - Git-LFS 설치 후 git으로 clone 해야 파형까지 받아짐 (.trs 파형 가지고 있으면 다운받을 필요 없음)
 * github 소스 다운 
   - `git clone https://github.com/ikizhvatov/pysca.git`
   ![clone](https://github.com/aasdzs/pysca/blob/master/captures/01_git_clone.PNG?raw=true)
 * pysca 분석을 위해 .trs 파형 .npz 파일로 변환
   - WinPython 실행 (I:\WinPython-64bit-2.7.13.1Zero\WinPython Command Prompt.exe)
   - pysca 폴더로 이동 (cd I:\pysca-master)
   - 파형 변환 `python trs2npz.py I:\pysca-master\traces\swaes_atmega_power`
   ![trs2npz](https://github.com/aasdzs/pysca/blob/master/captures/02_trs2npz.PNG?raw=true)
 * 분석 예제 코드 실행 및 결과 확인
   - `python attackaessbox.py`
   ![attack](https://github.com/aasdzs/pysca/blob/master/captures/03_attack_sbox.PNG?raw=true)
   - 공격 결과 plot 확인  
   ![plot](https://github.com/aasdzs/pysca/blob/master/captures/04_attack_sbox_plot.PNG?raw=true)

# Linux 설치
 * 리눅스 환경에서도 동작 가능할 것으로 판단되며, 추후 확인 예정
   
---


# Pysca toolbox

This toolbox was started in 2014 to experiment with efficient differential power analysis (DPA) techniques from the paper "Behind the Scene of Side Channel Attacks" by Victor Lomné, Emmanuel Prouff, and Thomas Roche (https://eprint.iacr.org/2013/794).

To clone this repo with the included example traces you will need [Git-LFS](https://git-lfs.github.com). Without Git-LFS, only pointers to traces will be cloned.

## Why
The toolbox was designed with the following in mind:
* state-of-the-art DPA techniques
* performance
* visualization of metrics for security evaluations purpose (and not just attack)
* simplicity and flexibility through use of a language suitable for scientific computing

In terms of these points, Pysca (still) outperforms some commercial tooling. Pysca is nowadays mostly superseded by https://github.com/Riscure/Jlsca.

## What
Pysca implements:
* non-profiled linear-regression analysis (LRA) with configurable basis functions
* classical correlation power analysis (CPA)
* significant speed-up of the above by conditional averaging
* targets: AES (S-box out) and DES (round in XOR round out, round out, S-box out)
* visualization of results

## How

For usage basics refer to the [HOWTO](howto/HOWTO.md).

For a deeper dive into [leakage modelling using linear regression](https://github.com/ikizhvatov/leakage-modelling-tutorial), clone the tutorial into the subfolder:

    git clone https://github.com/ikizhvatov/leakage-modelling-tutorial.git

## Details
Pysca works on traces stored in npz (numpy zipped) format. Example tracesets are included in the repo using git-lfs. The conversion script from Riscure Inspector trs format is included. The trs reader was originally implemented by Erik van den Brink.

Under the hood, the most interesting technical tricks in pysca are perhaps:
* fast computation of correlation (see https://github.com/ikizhvatov/efficient-columnwise-correlation for a dedicated study)
* conditional averaging implementation for DES (because of all the bit permutations, it requires splitting the leakage function into two stages)

Author: Ilya Kizhvatov<br>
Version: 1.0, 2017-05-14
