## Using WINGS for designing workflows

Below are steps and resources for getting started with the WINGS image for designing and executing workflows.

### Step 1
Download and install the WINGS template image by using this document
```
docker pull wingstemplate/wings_template
```
This contains a WINGS template image built on top of kcapd/wings-genomics as detailed in the [detailed WINGS tutorial here](https://dgarijo.github.io/Materials/Tutorials/wings-docker/). Will need [docker](https://www.docker.com/) installed.

### Step 2
Download WINGS API and relevant scripts from this repository - ***wings-api_and_scripts***
Unzip and place in an empty folder.

### Step 3
As mentioned in tutorial highlighted in Step 1, you will need to run the docker image by downloading the start-wings.sh file and changing a few specifics like the image name etc. Once successfully started you can navigate to http://localhost:8080/wings-portal in order to start working with WINGS and the designing the workflow.
The credentials for the WINGS image are
Username:admin
Password:4dm1n!23

### Step 4
The test workflow contains a component that takes an input file from Sub Challenge 1 and produced and output file. You can download the component (under ***Manage Component***) zipped file and change ONLY the ***run*** file (as shown below) and the analysis script itself and reupload the zip to change component functionality. Data will similarly be uploaded under ***Manage Data***. Currently a data type `data_m` is created in the image, under which any new files can be uploaded.
```
. $BASEDIR/io.sh <num of inputs> <num of parameters> <num of outputs> "$@"
Rscript --no-save --no-restore $BASEDIR/impute.R $INPUTS1 $OUTPUTS1 # Command to run analysis script
........
mv $OUTPUTS1.tsv $OUTPUTS1 # Moving the output to the correct variable
```
### Step 5
Once the workflow is complete and runs successfully within the image, you can now automate data upload, and workflow runs. 
Open the automating `executing_workflow_on_wings.sh` script and change the required parameters as instructed. This template script will 
- Upload data under `data_m` data type
- Run the required workflow using the input provided
- Move the output from the image container to your local machine
- Rename the output file

***Note*** 
1. Might need to install module `requests` using `sudo pip install requests` on machine
2. Varying types of input JSONs are located under `/resources`. On the left is the variable name and on the right is the binding. If it has a `"file:" ...` in the beginning, then it is an input file, if not, then it is a parameter. A collection input is set as an array.
3. Output files are named as `<variable_name+alphanumeric>`. The included image denotes the output file name as `output_file` and the `executing_workflow_on_wings.sh` script retains only files bound to this variable. 
4. Clearing the cache is a good troubleshooting method if image is working intermittently.
5. Clearing workflow runs and data before using the automating script is ideal.


