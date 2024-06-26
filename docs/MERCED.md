## MERCED cluster <!-- {docsify-ignore} -->

MERCED (Multi-Environment Research Computer for Exploration and
Discovery) Cluster is a 1,872 core Linux based high performance computing
cluster. The MERCED cluster runs with the [RedHat](https://www.redhat.com/en/technologies/linux-platforms/enterprise-linux) operating system,
and employs the [Slurm](https://slurm.schedmd.com/) job scheduler and queueing system to manage job runs. a standard flavor of Linux, and employs the Slurm job scheduler and queueing system to manage job runs.


## MERCED recharge service (starting Feb. 2023) <!-- {docsify-ignore} -->
!>__OIT-CIRT recharge services, including MERCED cluster core-hours will be renewed starting Feb. 2023.__

In order to minimize disruptions to computational research on MERCED cluster, the Provost’s office has provided bridge funding for all MERCED cluster PIs for core-hour usage on MERCED through June 30, 2024


__Faculty PIs__: Please ensure that the user accounts are active and provide the COA# [here](https://merced-my.sharepoint.com/personal/yyu49_ucmerced_edu/_layouts/15/onedrive.aspx?id=%2Fpersonal%2Fyyu49%5Fucmerced%5Fedu%2FDocuments%2FMERCED%5Frecharge&ct=1639436999554&or=OWA%2DNT&cid=61ce730a%2D0df2%2Dd438%2D7bdf%2Dbe138cf58c23). Email cirt@ucmerced.edu for more assistance.

What is the unit of cost service? MERCED cluster cycles will be charged by **core-hours<sup>(1)</sup>**.

(1)  A core-hour is a single compute core<sup>(2)</sup> used for one hour (a core-hour) and 2G of RAM. The total cost in core-hours for a complete computation is:
```text
Total Cost ($) = # of core-hours x Duration (wall clock hours) x (cost per core-hour)
```
!> **For UC users, the cost per core-hour is $0.01, and the cost for non-UC external users is $0.02.**

(2)  A core is an individual processor: the part of a computer that executes programs. (The MERCED cluster has about 3100 cores.)


## System overview
MERCED hosts 82 CPU compute nodes including 28 high memory nodes. Please be aware that
the nodes among MERCED cluster are multigenerational, meaning that the CPU
processors from different nodes are having different features, the table shows below
listed detailed node information. Users may experience relative big
performance variations when running the same jobs on different nodes.

The table below listed all MERCED cluster CPU compute nodes features, and their
processors generations.

| Nodes        | feature                                                    | RAM   | Total cores per nodes | InfiniBand (IB) |
|--------------|------------------------------------------------------------|-------|-----------------------|----------------|
| 33-43        | Broadwell,avx2,E5-2650_v4,local scratch 932GB              | 128GB | 24                    | yes            |
| 44           | Broadwell,avx2,E5-2650_v4,local scratch 932GB              | 112GB | 24                    | yes            |
| 45-60        | Broadwell,avx2,E5-2650_v4,local scratch 932GB              | 257GB | 24                    | yes            |
| 61-72        | Broadwell,avx2,E5-2650_v4,local scratch 447GB              | 257GB | 24                    | yes            |
| 73-76, 79-88 | Broadwell,avx2,E5-2650_v4,local scratch 932GB              | 128GB | 24                    | yes            |
| 77           | Broadwell,avx2,E5-2650_v4,no local scratch                 | 128GB | 24                    | yes            |
| 89-104       | Skylake,sse4.2,avx,avx2,avx512,Gold_6130, no local scratch | 191GB | 32                    | yes            |
| 105-114       | cascadelake,sse4.2,avx,avx2,avx512,Gold_6230, no local scratch | 191GB | 40                    | yes            |

?> **MERCED is no longer supporting Jupyterhub.**



## Merced Queue Information:
| Public Queues(Available to all users)| Max Wall Time | Default Time | Max Nodes per Job | Max # of jobs that can be submitted | 
| -------------------------------------|---------------|--------------|-------------------|-------------------------------------|
| bigmem | 5 days | 1 hr | 2 nodes | 6 | 
| test^ | 1 hour | 5 min. | 2 nodes | 1 |
| *compute | 5 days | 1 hr | 2 nodes | 6 | 

* <span style="color: red;"> `#SBATCH -M merced ` must always be used to submit a job to MERCED cluster</span>
* <span style="color: red;"> ^ `test` queue has access to all node types use constraints to test on specific types.</span>
* <span style="color: red;"> \* `compute` queue is the default queue for all jobs submitted  </span>


## Requesting an account  

The following detail consists of how to request Merced cluster access. If you have questions or concerns, do not hesitate to:
* Schedule a MERCED consultation [here](https://arrangr.com/sarvani/facultyconsult). 

!> Who can request access for MERCED cluster?
* An active PI or student __with a PI advisor__ with associated COA information.


?> Before applying for an account, please read the following information
carefully. Note that MERCED is a recharge model, which means PI must provide __COA__ information in the ticket before they can use MERCED. 

Each PI account has at least one user group and one queue project
associate with it, which may be used by all group members. PI’s must notify the system administration staff when students, postdoctorals, and other group members depart the University and should no longer have access to MERCED. All data stored in a user accounts will be accessible at all times by the associated PI.

!> How to request for MERCED cluster access?

__Requesting access to MERCED is a 4-step process.__
1. UC Merced Principal Investigators (PIs) or other researchers request MERCED account [here](https://ucmerced.service-now.com/servicehub?id=public_kb_article&sys_id=643ea9ff1b67a0543a003112cd4bcba3&form_id=280d8bb04f72f6006137d0af0310c7b0).
2. For new account group project applications, PIs please also make sure to complete the export control [form](https://ucmerced.app.box.com/s/zvptfc8adbdzt4xs8kcj73lyretyn692), if the PI has not done one before.
3. Once the form is completed, please attach the form to the request ticket from step 1. 
4. For PIs requesting access to MERCED cluster, please provide Chart of Account(COA) number associated with this project. Other group members, please reach out to your respective PI to obtain the COA number. Attach the COA number in the request ticket. Requests without COA numbers will be denied.   

>What is included? 

OIT will verify the eligibility and create the appropriate account for
either a PI Group or a user.
* Users can install any licensed software packages in their __home__
  directories, and where appropriate for other systems. If
  you need assistance, submit a [Research Software Installation Request](https://ucmerced.service-now.com/servicehub?id=public_kb_article&sys_id=b83ee9ff1b67a0543a003112cd4bcbde&form_id=0cb3dca04f7d4300b52ba1618110c7ff)(Servers and Clusters), including on which system to install the software.
* The MERCED cluster uses a queuing system for job submission.
  Priorities of jobs are assigned on a dynamic basis. Not all jobs
  submitted will begin immediately.

?> Questions?
If you still have questions, we have put together a MERCED [FAQ page](merced_FAQ.md) that has the most common questions received and will be keeping it up to date.



## MERCED Questions: <!-- {docsify-ignore} -->

>Q:I have a lot of work on MERCED, but do not want to pay the Recharge rate, what are my options?

All work that was stored on MERCED can be accessed and be used on Pinnacles cluster for **Free**.
User's can always obtain access to other free compute resources such as our new NSF-MRI Pinnacles cluster, and [ACCESS](https://access-ci.org/) resources. CIRT can provide consultation for how to access these resources. Schedule a consultation [here](https://ucmerced.service-now.com/servicehub?id=public_kb_article&sys_id=3c3ee9ff1b67a0543a003112cd4bcb13&form_id=06da3f8edbfc08103c4d56f3ce9619f4).

>Q: How to include CIRT managed charged services into budget statement? 

The faculties plan to include the budget for CIRT-managed charged services in the budget statement. Please list it under the category of 'Other Direct Costs.' For example, purchased HPC storage services can be allocated to the 'Materials/Supplies: MERCED HPC storage and maintenance' category within the 'Other Direct Costs' section. Here is the sample <a href="./files/Budget_Justification_NIH Data_Management_And_Sharing_Justification.pdf" download="Budget_Justification_NIH Data_Management_And_Sharing_Justification.pdf">NIH data management documentations</a> and <a href="./files/Budget Justification Detailed.pdf" download="Budget Justification Detailed.pdf">budget justification</a>.


> I don’t want to pay for MERCED what are my options?

Remember, you can always obtain access to other free compute resources such as our new NSF-MRI Pinnacles cluster, and [ACCESS](https://access-ci.org/) resources – and my team can provide consultation for how to access these resources. Schedule a consultation [here](https://ucmerced.service-now.com/servicehub?id=public_kb_article&sys_id=3c3ee9ff1b67a0543a003112cd4bcb13&form_id=06da3f8edbfc08103c4d56f3ce9619f4).

> I have other questions!

We have put together a [FAQ page](rechargeFAQs.md) that has more commonly asked questions about MERCED. 

There is also a page about more general FAQs relating to [HPC](hpc_FAQ.md)

Once again, if you have any questions or concerns regarding MERCED
cluster policies. Feel free to reach out to us via submitting a [general request ticket](https://ucmerced.service-now.com/servicehub?id=public_kb_article&sys_id=3c3ee9ff1b67a0543a003112cd4bcb13&form_id=06da3f8edbfc08103c4d56f3ce9619f4).


## How to cite  
All MERCED users must agree to acknowledge the MERCED Cluster and the
supporting NSF grant (ACI-1429783) in talks, posters, manuscripts, and
other forms of dissemination relying on results obtained from time on
MERCED. An example acknowledgement section is:
```text
This research [Part of this research] was conducted using [MERCED cluster (NSF- MRI, #1429783) / Pinnacles
(NSF MRI, # 2019144) / Science DMZ (NSF- CC* #1659210)] at the Cyberinfrastructure and Research Technologies
(CIRT) at University of California, Merced.
```
From time to time the Committee on Research Computing (CoRC) may request a report of publications and presentations authored by MERCED users that have included results of calculations on MERCED. This information may be used by CoRC in advertising and report documents, future proposals, and/or other materials related to research computing at UC Merced. 


## Facility statement <!-- {docsify-ignore} -->

MERCED is a general-purpose computing cluster located in the server facility (see computing facilities [section](README.md)). The cluster consists of a login node, 65 compute nodes, and 15 high memory nodes. Total CPU counts is 1872.

