# Private DarkSUSY MC in MG5

## Create a directory and upload the package to the directory

    mkdir ~/MadGraph5
    cd ~/MadGraph5
    cp /afs/cern.ch/cms/generators/www/MG5_aMC_v2.6.1.tar.gz ./
    tar -xzf MG5_aMC_v2.6.1.tar.gz

## Get UFO model 
Go to the folder `MG5_aMC_v2_6_1/models`. Copy the UFO model there and unzip it to folder `MSSMD_UFO`:

    wget https://github.com/weishi10141993/DarkSUSY_MC_MG5/blob/master/MSSMDarkSector/MSSMD_UFO.zip

Go to the folder `MSSMD_UFO` and do:

    python write_param_card.py

A `param_card.dat`will be generated. This will be used later for the decay in `madspin_card.dat`.

## Set up ggH processes
Copy the proc_card.dat to directory `MG5_aMC_v2_6_1`:
    
    wget https://raw.githubusercontent.com/weishi10141993/DarkSUSY_MC_MG5/master/MSSMDarkSector/proc_card.dat
    
Run `./bin/mg5_aMC proc_card.dat` and a folder called `DarkSUSY` will be generated. 

## Generate and decay particles

    cd /MadGraph5/MG5_aMC_v2_6_1/DarkSUSY/bin
    ./generate_events

When the prompt shows `The following switches determine which programs are run...`, switch `madspin = OFF` to `madspin = ON` following the way it gives.

When the prompt asks you `Do you want to edit a card (press enter to bypass editing)?`, type the number to edit the `madspin_card.dat` to be same as [this](https://github.com/weishi10141993/DarkSUSY_MC_MG5/blob/master/MSSMDarkSector/madspin_card.dat).

Also you have the option to edit `run_card.dat` to change run settings, such as number of events, center of mass enegry, etc.
And change model parameter settings in `param_card.dat`.

After you finish editing and saving the card, the event generation starts. A `lhe.gz` file will be generated under `MG5_aMC_v2_6_1/DarkSUSY/Events/run_01_decayed_1` directory.

Unzip the file to get the .lhe file:

    gunzip -d *.lhe.gz

## Change dark photon lifetime

    wget https://raw.githubusercontent.com/weishi10141993/DarkSUSY_MC_MG5/master/MSSMDarkSector/replace_lifetime_in_LHE.py
    python replace_lifetime_in_LHE.py > DarkSUSY_mH_125_mN1_10_mND_1_mGammaD_0p25_13TeV_cT_100_events80k.lhe

## LHE Validation [Optional]
The validation is a simple check of different distributions for the analysis. 

    wget https://raw.githubusercontent.com/weishi10141993/DarkSUSY_MC_MG5/master/MSSMDarkSector/LHE_read.py
    wget https://raw.githubusercontent.com/weishi10141993/DarkSUSY_MC_MG5/master/MSSMDarkSector/tdrStyle.py
    python LHE_read.py
