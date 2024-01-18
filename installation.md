# Installing and Running GADGET-4

## Mac OS
### Installing and Configuring GADGET-4
- Download code from Github repository: http://gitlab.mpcdf.mpg.de/vrs/gadget4
    - Save in a known directory, Eg. `~/gadget4-master`

- Open the gadget4-master folder in a code editor (ex. VSCode)
- Download homebrew: 
    - /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
- Download needed libraries:
```
brew install python
brew install gcc
brew install make
brew install g++
brew install wget
brew install openmpi
```

- Go to the Makefile.lib file: `~/gadget4-master/buildsystem/Makefile.lib`
    - Change line 5 to: `HDF5_VERSION=1.14.3`
    - Change the link in line 68 to: https://hdf-wordpress-1.s3.amazonaws.com/wp-content/uploads/manual/HDF5/HDF5_1_14_3/src/hdf5-$(HDF5_VERSION).tar.gz
    - Within the buildsystem directory, run `make -f Makefile.lib`
        - This will download, compile, and build four of the required libraries (fftw, gsl, hdf5, hwloc)

- Go to `Makefile.gen.libs` which is also within the buildsystem directory
    - Add a line at the beginning of the file: `LIB_DIR=extlibs`
    - Change `HDF5_INCL` to: `-I$(LIB_DIR)/hdf5-1.14.3/hdf5/include`
    - Change `HDF5_LIBS` to: `-L$(LIB_DIR)/hdf5-1.14.3/hdf5/lib`

- Go to `Makefile` in `gadget4-master/`
    - Change line 97 to: 
    ```
    PYTHON   = python3
    ```

- Download: https://repository.prace-ri.eu/git/UEABS/ueabs/-/raw/master/gadget/example_ics.tar.gz
    - Move the file to the gadget4-master directory from Downloads after uncompressing it
```
tar -zxf example_ics.tar.gz
mv ExampleICs ~/gadget4-master/
```

- In the param.txt within each of the 8 examples, change the directory of `InitCondFile` to the one in your computer
    - Eg. *for the CollidingGalaxiesSFR example*
`InitCondFile /u/vrs/Simulations/ICs/ExampleICs/ics_collision_g4.dat` →→→→→ `InitCondFile   /User/user/Documents/gadget4-master/ExampleICs/ics_collision_g4.dat`

- Once completed for all 8 examples, you are done with installing and configuring GADGET-4 to run simulations

### Running an Example A:
- Copy the Config.sh file from Example A and paste into the root `~/gadget4-master` directory
- Uncomment the relevant system from `Template-Makefile.systype` and rename to `Makefile.systype`
- Run `make` in terminal
- Run gadget4 and enjoy! (ctrl+c to end)
```
mpirun -np 32  ./Gadget4  param.txt
```
- The program outputs should be saved in directory output within Example A’s directory

## Windows
### Installing and Configuring GADGET-4

**Important Note: Use Windows Powershell (not Command Prompt)**

- Install Windows Subsystem for Linux and configure, and shutdown WSL:
```
wsl --install
exit
wsl --shutdown
```

- Create a new WSL config file to specify memory and CPU for WSL
```
notepad “$env:USERPROFILE\.wslconfig” 
```

- Configure memory and processors
```
[wsl2]
memory=XGB
processors=Y
```
- Update and re-start WSL
```
wsl --update
wsl
```

- Install dependencies
```
sudo apt install python
sudo apt install gcc
sudo apt install make
sudo apt install g++
sudo apt install wget
sudo apt install git
git clone https://github.com/Homebrew/brew/tarball/master homebrew
eval “$(homebrew/bin/brew shellenv)”
brew update --force --quiet
chmod -R go-w “$(brew --prefix)/share/zsh”
brew install openmpi
exit
```

- Download Gadget4 code from Github repository: http://gitlab.mpcdf.mpg.de/vrs/gadget4
- Save in a known directory
    - Eg. `~/gadget4-master`
- Open the gadget4-master file in a code editor (ex. VSCode)
    - IMPORTANT: Enter wsl on the code editor terminal to use the Linux features required for this code
- Continue from Step 5 in the macOS section

## Further References
- GADGET-4 Code Website: https://wwwmpa.mpa-garching.mpg.de/gadget4/
- GADGET-4 Manual: https://wwwmpa.mpa-garching.mpg.de/gadget4/gadget4_manual.pdf
- Installation guide in Google Doc format: https://docs.google.com/document/d/1WSbGrj2irlQHA59gG0gqhueiF8eAra_0Nsk6QdJXiJ0/edit?usp=sharing
