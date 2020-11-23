---
description: How to install and run the workflow
---

# Installation and Usage

## Step 1: Create a virtual environment.

## Step 1a: If you are using cwltool only, please install using python 3.7 as done below:

Here we can use either [virtualenv](https://virtualenv.pypa.io/) or [conda](https://docs.conda.io/en/latest/). Here we will use virtualenv.

{% code title="python3-virtualenv" %}
```bash
pip3 install virtualenv
python3 -m venv my_project
source my_project/bin/activate
```
{% endcode %}

## Step 1b: If you are using toil only, please install using python 2.7 as done below:

Here we can use either [virtualenv](https://virtualenv.pypa.io/) or [conda](https://docs.conda.io/en/latest/). Here we will use virtualenv.

{% code title="python2-virtaulenv" %}
```bash
pip install virtualenv
virtaulenv my_project
source my_project/bin/activate
```
{% endcode %}

{% hint style="info" %}
Once you execute the above command you will see your bash prompt something on this lines: 

{% code title="bash-prompt-example" %}
```bash
(my_project)[server]$ 
```
{% endcode %}
{% endhint %}

## Step 2: Clone the repository

{% code title="git-clone-with-submodule" %}
```bash
git clone --recursive https://github.com/msk-access/standard_bam_processing.git
cd standard_bam_processing
git submodule update --recursive --remote
```
{% endcode %}

## Step 3: Install requirements using pip

We have already specified the version of cwltool and other packages in the requirements.txt file. Please use this to install.

{% code title="python-package-installation-using-pip" %}
```bash
#python2
pip install -r requirements.txt
#python3
pip3 install -r requirements.txt
```
{% endcode %}

{% hint style="info" %}
To see help for the inputs for cwl workflow you can use: `cwltool standard_bam_processing.cwl --help`
{% endhint %}

{% hint style="info" %}
To see help for the inputs for cwl workflow you can use: `toil-cwl-runner standard_bam_processing.cwl --help`
{% endhint %}

Once we have successfully installed the requirements we can now run the workflow using _cwltool/toil_ if you have proper input file generated either in [json](https://www.json.org/) or [yaml](https://yaml.org/) format. Please look at [Inputs Description](inputs-description.md) for more details.

{% tabs %}
{% tab title="Using cwltool" %}
Here we show how to use [cwltool](https://github.com/common-workflow-language/cwltool) to run the workflow on single machine

## Step 4: Run the workflow with a given set of input using [cwltool](https://github.com/common-workflow-language/cwltool) on single machine

{% code title="cwltool-execution" %}
```bash
cwltool standard_bam_processing.cwl inputs.yaml
```
{% endcode %}
{% endtab %}

{% tab title="Using toil-cwl-runner locally" %}
Here we show how to run the workflow using [toil-cwl-runner](https://toil.readthedocs.io/en/latest/running/introduction.html) using single machine interface.

Once we have successfully installed the requirements we can now run the workflow using _cwltool_ if you have proper input file generated either in [json](https://www.json.org/) or [yaml](https://yaml.org/) format. Please look at [Inputs Description](inputs-description.md) for more details.

## Step 4: Run the workflow with a given set of input using [toil](https://toil.readthedocs.io/en/latest/running/introduction.html) on single machine

{% code title="toil-local-execution" %}
```bash
toil-cwl-runner standard_bam_processing.cwl inputs.yaml
```
{% endcode %}
{% endtab %}

{% tab title="Using toil-cwl-runner on JUNO" %}
Here we show how to run the workflow using [toil-cwl-runner](https://toil.readthedocs.io/en/latest/running/introduction.html) on MSKCC internal compute cluster called JUNO which has [IBM LSF](https://www.ibm.com/support/knowledgecenter/en/SSETD4/product_welcome_platform_lsf.html) as a scheduler.

## Step 4: Run the workflow with a given set of input using [toil](https://toil.readthedocs.io/en/latest/running/introduction.html) on JUNO \(MSKCC Research Cluster\)

{% code title="toil-lsf-execution" %}
```bash
TMPDIR=$PWD
TOIL_LSF_ARGS='-W 3600'
toil-cwl-runner \
       --singularity \
       --logFile /path/to/toil_log/cwltoil.log  \
       --jobStore /path/to/jobStore \
       --batchSystem lsf \
       --workDir /path/to/toil_log \
       --outdir $PWD \
       --writeLogs /path/to/toil_log \
       --logLevel DEBUG \
       --stats \
       --retryCount 2 
       --disableCaching \
       --disableChaining \
       --maxLogFileSize 20000000000 \
       --cleanWorkDir onSuccess
       --preserve-environment TOIL_LSF_ARGS TMPDIR \
       /path/to/standard_bam_processing.cwl \
       /path/to/inputs.yaml \
       > toil.stdout \
       2> toil.stderr &
```
{% endcode %}
{% endtab %}
{% endtabs %}

{% hint style="success" %}
You should now be running the workflow on the specified batch system
{% endhint %}

