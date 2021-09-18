# Sparkify Churn Analysis
Predicting user churn for a music subscription service

## Project Definition

Here we use Pyspark on an AWS EMR cluster to analyse a simulated event log file from a fictional Spotify-like music streaming service called Sparkify, in an attempt to predict which users churned. We define churn as a user on paid subscription downgrading to free subscription, or cancelling the service altogether.

Specifically, we attempt to predict whether a given user churned during the 2 months covered by the data, based on their activity during this period. We explore and clean the data, engineer any needed features, and train several classification models to determine which is best suited to the task.

Precision and recall are both important, so the models are evaluated based on their F-score. For example, it would be useful for a marketing department to receive a list of users likely to churn, so that a marketing intervention could take place. However, it would not be so useful if the list included lots of users who were not likely to churn, potentially incurring unnecesary expense.

This project is a capstone project from the [Udacity Data Scientist nanodegree](https://www.udacity.com/course/data-scientist-nanodegree--nd025), and the data is provided by Udacity.

## Setup

The included jupyter notebook file, sparkify_project.ipynb,  will complete analysis of the full dataset in approximately 25 minutes using the setup specified here. The notebook was run on an Amazon Web Services EMR cluster with the following advanced options:
* Release label: emr-6.3.0
* Hadoop distribution: Amazon 3.2.1
* Applications: Spark 3.1.1, JupyterEnterpriseGateway 2.1.0, JupyterHub 1.2.0, Ganglia 3.7.2, Livy 0.7.0
* Software configuration:
```
[
    {
    "classification" : "spark", 
    "properties" : {
                    "maximizeResourceAllocation" : "true"
                    }, 
    "configurations" : []
    },
    {
    "classification" : "livy-conf", 
    "properties" : {
                   "livy.server.session.timeout" : "5h"
                   }, 
    "configurations" : []
    }
]
```
* Master: 1 m5.xlarge
* Core: 2 m5.xlarge for sample dataset (123Mb), 8 m5.xlarge for full dataset (12Gb)
* EC2 key pair: A .ppk key pair


After following the instructions at https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-connect-ssh-prereqs.html to allow SSH access, the .ppk key was used in conjuction with PuTTY to install pandas and matplotlib on the master node using the following commands:

* `sudo yum -y install python3-devel-3.7.10`
* `sudo pip3 install cython==0.29.24`
* `sudo pip3 install pandas==1.3.2`
* `sudo pip3 install matplotlib==3.4.3`

N.B. Bootstrapping these commands will not work as numpy will later be overridden by an earlier version - see https://forums.aws.amazon.com/thread.jspa?messageID=989210&tstart=0.

## Files

### sparkify_project.ipynb

The jupyter notebook file containing the full analysis, for upload to your EMR cluster. This includes the code executed, the resulting output, and commentary on the analysis.

### sparkify_project.html

A HTML version of the jupyter notebook file to review the code and commentary from any browser.

## Data

Two datasets are provided by Udacity - a 12Gb event log and a 123Mb sample of this event log. Both can be loaded from AWS S3 buckets, which are specified in the jupyter notebook file.

## Results

Our final model, a random forest classifier, did manage to make correct predictions for the majority of users, but left room for improvement, with an F-score of 0.600 to 3 d.p.

It turned out that users' enjoyment or otherwise of the music was an influential factor in whether or not they churned, as inferred from the proportion of thumbs ups to thumbs downs and the proportion of songs which were skipped.

The number of ads that were played was also influential, and users that had already visited the downgrade page in the past were more likely to go back and complete the action.

Overall usage in terms of number of songs played was not particularly influential.
