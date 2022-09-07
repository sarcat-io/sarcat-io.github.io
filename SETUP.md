# SARCAT SETUP
quick reference:
   1. git clone https://github.com/sarcat-io/SARCAT.git
   2. cd ./SARCAT
   3. sudo chmod +x *.sh && ./sarcatSetup.sh
   4. nano ../!SARCATconfig.sarcat.json and fill in the fields // will create a prompter function to complete
   5. 

## Prerequisites and Permissions
*Note: this has not been tested on windows as of September 5th 2022
SARCAT Build refers to the system where SARCAT is built and staged. The expectation is that when processing production assessment reports, SARCAT is run in the authorization boundary. This directly implies that all SARCAT build and configuration is done **PRIOR TO MOVING INTO PRODUCTION**.

SARCAT Build System must have the following installed:
  - git
  - docker (or ability to run docker builds and containers)
  - 
SARCAT Build System must have access to the following domains:
  - github.com
  - github.io
  - sarcat.io
  - registry.npmjs.org
  - skimdb.npmjs.com
  - pypi.org
  - files.pythonhosted.org
  
SARCAT permissions requirements are as follows: 
- Host OS
  - Build docker images (build system)
  - start/stop containers (build and run system)
  - read/write files and directories within immediate directory folder  (build and run system)
  - Docker container requires to 
    - read write files to and from its the immediate parent directory and sub directories 
    - mount sub sirectories of the immediate parent directory as a volume  (build and run system)
    - e.g., if SARCAT is cloned while in /home/users/parent/ the resulting directory will be /home/users/parent/SARCAT
    - SARCAT needs to read write files and directories in /home/users/parent/ and its subdirectories
- Guest OS
  - Network Connectivity to domains mentioned above (build system)
  - read/write files to host OS file system 


## Setup
Run the following commands on the SARCAT Build System
   1. git clone https://github.com/sarcat-io/SARCAT.git
   2. cd ./SARCAT
   3. sudo chmod +x sarcatSetup.sh && ./sarcatSetup.sh
      1. Creates a folder named ../!SARCAT_ARCHIVE!


The container has now been built and SARCAT is now ready to run


## Before first run
### !SARCAT_ARCHIVE!
in the SARCAT Parent folder, you will see a folder named "!SARCAT_ARCHIVE!".  **this will be your is your persistent SARCAT folder**. This folder will contain all of your bundles and all of your raw assessment files. This should serve as the clear text version of the archive that is made available to authorizing officials. When providing as deliverable to authorizing officials, SARCAT will run a  command that will compress and encrypt the archive. The compressed and encrypted archive is what should then be moved to the location that is accessible by the authorizing officials.

### Sarcat Configuration Files
#### Minimum Requirements


## Bootstrapping
### Original Discovery Date for Vulnerabilities
