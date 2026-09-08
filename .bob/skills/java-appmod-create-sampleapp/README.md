# Skill to create a traditional WebSphere application from scratch

Select issues that you want to demonstrate in your Java Modernization demo and the skill will create a traditional WebSphere Application Server application with such issues. It will then build the application binaries and will use the Application Modernization Accelerator (AMA) to create the data collection as well as the migration plan. You can then use the assets to demonstrate how IBM AMA and IBM Bob can help customers during their modernization journey.

## Motivation - why do we need such a skill?

The motivation is to demonstrate our Java Modernization story in a more powerful way to customers currently running WebSphere Application Server applications.

### What's the current challenge?

IBM has several tools to assess WebSphere Application Server (WAS) applications regarding the readiness to run on WebSphere Liberty. This includes tools like:

- IBM Migration Toolkit for Application Binaries (binary scanner)
- IBM Liberty Advisor which is part of the WAS ND Administration Console
- IBM Transformation Advisor (TA)
- IBM Modernization Accelerator (AMA)
In addition, IBM has tools like IBM to help with the modernization.

Typical scenario:
- The customer used the binary scanner or TA to assess his applications. 
- He asks IBM to demonstrate how IBM AMA and IBM Bob can help. 
- The IBMer demonstrates with our sample applications how the tools can help - but the issues that are resolved have nothing to do with the issues seen by the customer. 
- The customer raises the question if IBM AMA and IBM Bob can also resolve issue, but you cannot answer as you do not know (and never had the chance to test it).

So what is the challenge?

Our Java Modernization demos for IBM AMA and IBM Bob often do not fit very well to the customer expectation. And the reason is often that the sample applications do not fit very well for one or more reasons:
- The sample application is too simple to demonstrate the power of IBM AMA and IBM Bob.
- The sample application contains some issues but these do not reflect the issues seen at customer side.
- The sample application is a good fit for IBM AMA but the source code is not available, so could not be demonstrated with IBM Bob.
- The sample application contains too many issues so that the end-to-end demo becomes too complex or the resolution via iBM Bob takes too long.


## Target usage for the skills:

There are two steps: 
- Create a sample application including source code using the skill
- Demo the sample application using IBM AMA and IBM Bob
The skill is targeted for preparation and not in front of the customer.


## How to use the skill asset in detail:

### Software requirements
You need the following software to be installed:
- Maven
- Git
- IBM Application Modernization Accelerator (AMA) v4+
- In case of AMA v5, you also need an access key (PoC key or Full Key)
- IBM Bob v2
- Traditional WAS 9.0.5 or in minimum the file **was_public.jar** which is in the WAS_HOME/dev by default.

If you do not have such an environment, you can request a Techzone Environment called **"Application Modernization VM - for Liberty, AMA, IBM Bob"** which has traditional WAS, AMA and IBM Bob installed.
https://techzone.ibm.com/collection/liberty-getting-started-labs-demos/journey-modernization-tools.

#### What happens if you don't have access to AMA?
The skill will still generate the sample application source and binaries.
But the data collection and the migration plan cannot be generated.


### Prepare IBM Application Modernization Accelerator (AMA)

AMA needs to be started as the skill will try to access to to download the ama discovery tool. AMA is therefore expected to be started and listening on port 3000 (UI) and 2220 (API). You can adjust it when asked to allow acces to IBM Bob.

If you use the TechZone environment, AMA is typically already started. If not, you can start AMA via the following command:

         cd ~/usr/IBM/application-modernization-accelerator-local-*
        ./launch.sh

If you need more details how to use AMA or the TechZone environment, take a look at the following tutorial:
https://github.com/LarsBesselmann/LibertyGettingStarted-2026-AMA-Lab


### Download and install the skill
1. Create a project directory for the sample app (e.g. ~/work/createSampleApp).

        mkdir -p ~/work/createSampleApp

2. Clone the repository:

        git clone https://github.com/LarsBesselmann/create-tWAS-app-skill.git ~/work/createSampleApp

3. Open IBM Bob 
            
            cd ~/work/createSampleApp
            bobide .

4. Verify that the skill is available



### Create a sample application using the skill
1. Open IBM Bob via

        bobide .

2. Log into Bob

3. To create a traditional WAS application, enter in the chat window the following command:

        /java-appmod-create-sampleapp

    These are the main steps that will be performed:
    - Step 1 — Select the Application Type
        - The decision as of now does not really matter as there is no industry specific content.
        - Main impact is on the application name which can be adjusted.
    - Step 4 — Implement the Required WebSphere APIs
        - Select which issues with automated fixes should be implemented.
        - Select which issues without automated fixes should be implemented.
        - For a demo keep it is recommend to only have 1 or 2 non-automated fxies, otherwise the IBM Bob part of the demo takes too long and you cannot complete the end-2-end demo.
    - Step 6 — Build the Application
        - The application will be build and packages as ear file. 
    - Step 8 - Create an AMA workspace, download the datacollector and run it
        - If AMA is accessible, the following steps are performed:
            - an AMA workspace gets created
            - the AMA discovery tool is downloaded and installed
            - the AMA discovery tool is executed and the data collection uploaded
        - If AMA is not accessible, this step will be skipped
    - Step 9 - Generate the AMA migration plan for target Liberty with Java 8
        - If AMA is accessible, the following steps are performed:
            - the migration plan will be generated and downloaded
            - the AMA UI will be opened in the created workspace
        - If AMA is not accessible, this step will be skipped
    - Step 11 - Get ready to demo AMA and IBM Bob

You can find a walk-through without voice here:
https://ibm.box.com/s/f9d0ww2j0rdq8z0jpced2szv1pm10fmt



## Additional hints:

### How to extend the asset:
The migration issues that can currently be implemented are defined in the file **was_migration_issues.md** in the skill's resources directory. Feel free to extend it.


### Additional material:
- You can find a tutorial for AMA here: https://github.com/LarsBesselmann/LibertyGettingStarted-2026-AMA-Lab
- You can find a tutorial for AMA + Bob here: https://github.com/LarsBesselmann/LibertyGettingStarted-2026-AMA-Bob-Lab


