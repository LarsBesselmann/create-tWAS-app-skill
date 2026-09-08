---
name: java-appmod-create-sampleapp
description: Use when the user wants to build, create, or scaffold a Java EE application that can be deployed to WebSphere Application Server Traditional (WAS). Guides through naming, app type selection, Maven project setup, WAS library installation, code generation using WebSphere-specific APIs, and building the EAR.
---

# Java Application Modernization - build a customized sample application.

Use when the user wants to build, create, or scaffold a Java EE application that can be deployed to WebSphere Application Server Traditional (WAS). Guides through naming, app type selection, Maven project setup, WAS library installation, code generation using WebSphere-specific APIs, and building the EAR.
---


## Step 1 — Select the Application Type

Ask the user which kind of application to build using `ask_followup_question` with these options:
- A banking application
- A shopping application
- An insurance application

Use this choice to shape the domain model, service names, and example data in the generated code.

- For a banking application, implement the option to transfer money and see the account. Implement two users with one account each.
- For a shopping application, implement a small catalog of goods like TV and PC and implement the option to buy a good. Implement two users and four goods.
- For an insurance application, implement the option to list curent insurance policies. Implement two users with two different policies each.

---
## Step 2 — Confirm the Application Name

Ask the user for the target application name using `ask_followup_question`:
- Generate an application name based on the use case
- Offer the generated name but also provide the option to enter a different name
- Use the confirmed name as both the application name and the EAR file name.

---

## Step 3 — Create the Maven Project Structure

Using `write_file` and `execute_command`, create a Maven multi-module project in a sub-folder named after the application:

```
<app-name>/
  pom.xml                  ← parent POM (EAR packaging)
  <app-name>-web/
    pom.xml                ← WAR module
    src/main/java/com/wasapp/sample/
    src/main/webapp/WEB-INF/web.xml
  <app-name>-ejb/
    pom.xml                ← EJB module
    src/main/java/com/wasapp/sample/
    src/main/resources/META-INF/ejb-jar.xml
```

- Target environment: **WAS 9.0, Java EE 7, Java 8**
- Base package: `com.wasapp.sample`
- EAR artifact id = application name; EAR file name = `<app-name>.ear`


---

## Step 4 — Implement some challenging WebSphere APIs and other frameworks

### Step 4.1 — Implement some challenging WebSphere APIs

Generate the application source code.
The application must implement some technology issues from the list in the file "was_migration_issues.md". Read ALL issues from the list.

- One or more technology issues with an automated fix must be implemented. 
  List ALL technology issues with an automated fix and ask the user to select the first, then the second and so on until he is fine with the selection.
  Display up to 4 issues at a time, but offer to list more if available.

- One or more technology issues without an automated fix must be implemented. 
  List ALL technology issues without an automated fix and ask the user to select the first, then the second and so on until he is fine with the selection. 
  Display up to 4 issues at a time, but offer to list more if available.

**All selected APIs must be implemented.** 
Use the exact method signatures from `was_public_signatures.txt` and do not simplify or omit parameters. Do not merely import the classes; each API must be actively called in working code.
Place each concern in a logically named class under `com.wasapp.sample` (e.g. `SecurityHelper.java`, `CacheService.java`, `JmsService.java`, `TransactionService.java`, etc.).
Implement only those issues that have been selected.

### Step 4.2 — Implement some challenging frameworks
Ask the user if any third-party Java 8 frameworks should be used to implement the application. Offer the following options:
- Spring 5.3.x
- Apache Struts 1.1
- none of those frameworks

If a framework has been selected, use that framework to implement the one or other business logic.


---
## Step 5 — Install the WAS Library

If the WAS library was_public.jar is not located in the skills resources directory or in the current workspace, ask the user to enter the full directory path to the WAS library was_public.jar.
Don't give any recommendation but give the hint that it can be found in the WAS_HOME/dev directory.
Verify that the file was_public.jar can be found in the specified directory.

Run the following command using `execute_command` from the project root to install the local WAS public JAR into the local Maven repository.
Adjust in the command the path to was_public.jar from "./" to the one specified by the user:
The path to the jar needs to be absolute. Run the command from the workspace root directory:

```bash
mvn -N install:install-file "-Dfile=./was_public.jar" "-DgroupId=com.ibm.websphere.appserver" "-DartifactId=was_public" "-Dversion=9.0.0" "-Dpackaging=jar"
```

Then add the dependency to every module `pom.xml` that uses WebSphere APIs:

```xml
<dependency>
  <groupId>com.ibm.websphere.appserver</groupId>
  <artifactId>was_public</artifactId>
  <version>9.0.0</version>
  <scope>provided</scope>
</dependency>
```
---


## Step 6 — Build the Application

Run the Maven build using `execute_command`:

```bash
cd <app-name> && mvn clean package
```

Fix any compilation errors before proceeding. The build must produce `<app-name>.ear` successfully.

---

## Step 7 — Explain Runtime Configuration Requirements

After a successful build, provide a clear explanation of which implemented WebSphere APIs require additional runtime configuration in WAS, including:
- Required resources (JMS topics/queues, JNDI bindings, data sources)
- Security configuration (LTPA, SSL repertoires)
- Dynamic cache configuration
- Transaction manager settings

Format this as a concise checklist grouped by API area.
Add also the information about the context root.
Create a directory called "Demo-Assets" to store the generated assets.
Copy the generated EAR file into the Demo-Assets directory.

## Step 8 - Create an AMA workspace, download the data collector and run it
### Step 8.1 - Create an AMA workspace, download the data collector and run it

Ask the user if IBM Bob should use an existing AMA (Application Modernization Accelerator) to create a workspace and run the data collector. 

If so, use the following steps to do it:

(On Windows, use Invoke-WebRequest instead of curl)

- Run the following command to create a workspace
curl -k -X 'POST' 
  'https://localhost:2220/lands_advisor/advisor/v2/workspaces' \
  -H 'accept: */*' \
  -H 'locale: en' \
  -H 'Content-Type: application/json' \
  -d '{
  "name": "tWAS_Bob"
}'

- From the response, extract the field "id" and feed it into WORKSPACEID in the following command:
If running on Windows, set the value MYPLATFORM to Windows, otherwise to Linux

(On Windows, use Invoke-WebRequest instead of curl)

curl -k -X 'GET' \
  'https://localhost:2220/lands_advisor/advisor/v2/workspaces/WORKSPACEID/discoveryTool?platform=MYPLATFORM' \
  -H 'accept: */*' \
  -H 'locale: en'

- From the response, extract the value of "collectorURL"

- Use wget or another appropriete tool to download the discovery tool to the current directory

- Create a subdirectory called ama-discovery-tool and extract the file into the that directory
- Switch to the directory ama-discovery-tool
- Run the following command to accept the license

    echo "1" > licence_accepted

- Run the ama-discovery script with the option -o similar to 
  Windows: bin\ama-discovery -o DEMO-ASSETS/APPNAME.EAR
  Linux: bin/ama-discovery -o DEMO-ASSETS\APPNAME.EAR
  Do NOT specify to skip the upload.
- Do NOT run the ama discovery tool from the project root, run it from the directory ama-discovery-tool
  Replace DEMO-ASSETS with the fully qualified path to the Demo-Assets folder.
  Replace APPNAME.EAR with the name of the generated ear file 
  When executing the command, enter 1 when asked to the WebSphere version, following 4 for Java 8.
- Copy the generated data collection zip file into the Demo-Assets directory.

### Step 8.2 - Vadidate the AMA access key

Try to apply an AMA Full Access Key
If the file "~/software/AMA/AMA_apply_Full_Key.sh" exists, 
execute the command "sh ~/software/AMA/AMA_apply_Full_Key.sh".
Wait 5 seconds before continuing.

Use the following curl request to get information about the AMA access.
curl -k -X 'GET' 'https://localhost:2220/lands_advisor/advisor/v2/accessInfo' -H 'accept: */*'   -H 'locale: en'
Inspect the returned value, especially the field "productType".
- If the accessInfo API is not available, this indicates that likely AMA v4 is used and you can continue with the assessmentUnits curl request.
- If the returned JSON body includes "productType": "TRIAL", then ask the user the following:
"AMA has a trial key. Please apply the full key or PoC key. In case of a PoC key, assign the application to the PoC. Please return when done and enter Continue."
- If the returned JSON body includes "productType": "PoC", then ask the user the following:
"AMA has a PoC key. Please switch to AMA and assign the application to the PoC. Please return when done and enter 'Continue'."


### Step 8.3 - Ask the user to access AMA to review the uploaded data collection.
Say "Please open AMA and review the analysis results."
Say "The application <application name> is located in the workspace <workspace name>
Provide the URL to AMA which is by default https://localhost:3000
Ask the customer to keep the browser open and to enter **continue**.


## Step 9 - Generate the AMA migration plan for target Liberty with Java 8
Use the known WORKSPACEID and feed it into the following command:

(On Windows, use Invoke-WebRequest instead of curl)

curl -k -X 'GET' \
  'https://localhost:2220/lands_advisor/advisor/v2/workspaces/WORKSPACEID/assessmentUnits?includeVirtual=false' \
  -H 'accept: */*' \
  -H 'locale: en'

- From the response, extract the value of "id". If there are multiple ids, use the one which contains the application name in the value.
Feed the id as ASSETID as well as the WORKSPACEID into the following command

(On Windows, use Invoke-WebRequest instead of curl)

curl -k -X 'GET' \
  'https://localhost:2220/lands_advisor/advisor/v2/workspaces/WORKSPACEID/assessmentUnits/ASSETID/migrationPlan?eeLevel=ee6&javaLevel=java8&targetEnv=websphereLiberty' \
  -H 'accept: */*' \
  -H 'locale: en'

From the response, extract the value of "migrationBundle" which contains the migration bundle URL.
Use wget or another appropriete tool to download the migration bundle into the ama folder.

- Copy the downloaded migration bundle zip file into the Demo-Assets directory.

## Step 10 - Summarize what has been done
- Summarize what has been done similar to
  - The application has been generated, the EAR file is located in the Demo-Assets directory at ...
  - An AMA workspace with the name "tWAS_Bob" has been created.
  - The AMA Discovery Tool has been executed, the generated data collection is located in the Demo-Assets directory at ...
  - The AMA data collection has also been uploaded to AMA
  - The AMA migration bundle has been generated, the generated migration bundle is located in the Demo-Assets directory at ...

## Step 11 - Get ready to demo AMA and IBM Bob
Explain to the user the next steps similar to:
- Switch to AMA and review the assessment of the application ...
- Open a new bobide window in the project folder at ...

