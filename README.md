# Variant-filtering with Convolutional Neural Networks on Azure

This repository is an example of running GATK's CNN tool, which is a deep learning approach to filter variants based on Convolutional Neural Networks, by the Broad Institute of MIT and Harvard, on [Cromwell on Azure](https://github.com/microsoft/CromwellOnAzure). 

This repository is a fork from [the original](https://github.com/gatk-workflows/gatk4-cnn-variant-filter) and has all the required changes to run the WDL workflow on Cromwell on Azure.

Please read the following discussion to learn more about the CNN tool: [Deep Learning in GATK4](https://gatkforums.broadinstitute.org/gatk/discussion/10996/deep-learning-in-gatk4).<br/>

Here, you can find the WDL file and an example inputs JSON file with links to data hosted on a public Azure Storage account. You can use the "datasettestinputs" storage account directly as a relative path, like in the inputs JSON files.

The `cram2filtered.trigger.json` trigger file is ready to use. You can start the workflow on your instance of Cromwell on Azure, using [these instructions](https://github.com/microsoft/CromwellOnAzure/blob/master/docs/managing-your-workflow.md/#Start-your-workflow).


## gatk4-cnn-variant-filter

### Purpose :
Workflows that takes advantage of GATKs CNN tool which is a deep learning approach to filter variants based on Convolutional Neural Networks.

### cram2filtered.wdl
This workflow takes an input CRAM/BAM to call variants with HaplotypeCaller
then filters the calls with the CNNVariant neural net tool using the filtering model specified.

The site-level scores are added to the `INFO` field of the VCF. The architecture arguments,
`info_key` and `tensor_type` arguments MUST be in agreement (e.g. 2D models must have
`tensor_type` of `read_tensor` and `info_key` of `CNN_2D`, 1D models have `tensor_type` of
`reference` and `info_key` of `CNN_1D`). The `INFO` field key will be `1D_CNN` or `2D_CNN`
depending on the neural net architecture used for inference. The architecture arguments
specify pre-trained networks. New networks can be trained by the GATK tools: CNNVariantWriteTensors 
and CNNVariantTrain. The CRAM could be generated by the [single-sample pipeline](https://github.com/microsoft/gatk4-data-processing-azure/blob/master-azure/processing-for-variant-discovery-gatk4.wdl).


#### Requirements/expectations :
 - CRAM/BAM
 - BAM Index (if input is BAM) 

#### Output :
 - Filtered VCF and its index. 

### Software version notes :
- GATK 4.1.4.0 
- samtools 1.3.1
- Cromwell version support 
  - Successfully tested on v47 
  - Does not work on versions < v23 due to output syntax

### Important Note :
- The provided JSON is meant to be a ready to use example JSON template of the workflow. It is the user’s responsibility to correctly set the reference and resource input variables using the [GATK Tool and Tutorial Documentations](https://software.broadinstitute.org/gatk/documentation/).
- The following material is provided by the GATK Team. Please post any questions or concerns to one of our forum sites : [GATK](https://gatkforums.broadinstitute.org/gatk/categories/ask-the-team/), [Terra](https://broadinstitute.zendesk.com/hc/en-us/community/topics/360000500432-General-Discussion), [WDL/Cromwell](https://gatkforums.broadinstitute.org/wdl/categories/ask-the-wdl-team).
- Please visit the [User Guide](https://software.broadinstitute.org/gatk/documentation/) site for further documentation on Broad Institute's workflows and tools.


### LICENSING :
 This script is released under the WDL source code license (BSD-3) (see LICENSE in
 https://github.com/broadinstitute/wdl). Note however that the programs it calls may
 be subject to different licenses. Users are responsible for checking that they are
 authorized to run all programs before running this script.
