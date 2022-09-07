# SARCAT
**S**ecurity **A**nd **R**isk **C**ompliance **A**utomation **T**oolset  

SARCAT is free to use under GNU GPL v3

SARCAT is based on NIST standards and authorization requirements that are collaboratively defined between Authorization Program leadership and their technical representatives, the US Federal Cloud Service Provider industry, and multiple industry advisory organizations.

The primary authorization program targeted by SARCAT stakeholders is FedRAMP, however, CMMC, DOD, and other NIST based authorization programs (including US Federal Agency and Department internal programs and StateRAMP) were proactively considered from the beginning. 

SARCAT aims to be fully OSCAL compliant and interoperable with future objectives of dynamically using OSCAL component and assessment layers to drive reporting outputs to include updating OSCAL documentation layers with current system information and identifying findings/disparity for system owners and authorizing officials alike. 
SARCAT is actively pursuing its 501(c)(3) status and will provide updates as this progresses. 

## Philosophy
Manual assessment is ineffective and expensive. Custom automation is expensive and complicated. The IT Industry at large has the means to develop and the motive to reduce their own compliance expenditure by collaborating on a solution built using open standards and following transparent counsel and direction from US Federal IT System Authorization programs.  

SARCAT is and will remain available free of charge. The only prohibitive factor being the following:  All tools and scripts used by SARCAT participants must be added to SARCAT public repositories and licensed for use by all.

The humble request is that if you or your organization uses SARCAT, at minimum, send a note to thankyou@sarcat.io so we know who we are supporting. 

Send an email to updates@sarcat.io to be added to the SARCAT Update distribution list.

If you would to contribute or be more involved with SARCAT, send an email to contribute@sarcat.io

for more information, send an email to info@sarcat.io

## Features

### Future Features
Natively Connect to S3, Cloud Storage from Container

# Information Index
| Topic | Subtopic |
|--|--|
| Technical | |
||[Assess](./_application_/10_assess/README.md) |
||[Parse](./_application_/20_parse/README.md)|
||[Normalize](./_application_/30_normalize/README.md)|
||[Analyze](./_application_/40_analyze/README.md)|
||[Correlate](./_application_/50_correlate/README.md)|
||[Assemble](./_application_/60_assemble/README.md)|
||[Seal](./_application_/70_seal/README.md)|
||[Validate](./_application_/90_validation/README.md)|
# Organization
## Bootstrap Working Group
Meets weekly on Tuesdays @ 1330 ET with representation from the following:
 - SARCAT
 - GSA FedRAMP PMO
 - Google
 - Amazon
 - Salesforce
 - CSP AB
 - CMMC IAG
## Developers
Coordinate through this GitHub site via [Issues](https://github.com/sarcat-io/SARCAT/issues) and [Discussions](https://github.com/orgs/sarcat-io/discussions)
# Process
## High-level overview
### System Owners
### SARCAT Management
### Development Partners

 - Developers
 - Reviewers
 - Assessors
 - GRC Tool Vendors
 - Assessment Tool Vendors

#### Developers
## Intended Process Interaction
### Authorizing Officials (e.g., GSA FedRAMP JAB, DISA RME, CMMC AB)
### 3rd Party Audit Organizations (3PAO / C3PAO)
### Independent  Verification and Validation (IV&V) 

# Features

# How-to 
## Running SARCAT

### config.sarcat
WHile this is not required by default, it is foreseeable that if lots of companies provide customer raw data parers, that it will be difficult to manage. By default, SARCAT will automatically select a parser for the file extensions of the raw scan data. SARCAT will tell you what file extensions did not have a parser registered. 

{
    "parserPreferences":"default, organization, specific"
    "forcePreference": true false
    [{"toolName":"nessusParser", "extensions":["nessus", "xml"]}],
    "oganization":"" <- can
}

### Install all dependencies
By default,  dependencies are only installed when the raw scan files provided to parse have extensions associated with registered parsers. However, if you are running SARCAT inside an authorization boundary, you should either run sarcat outside of the boundary using empty files with correct extensions first so it is able to grab all the dependencies required or run once outside the boundary and select "install all dependencies" before moving it into the authorization boundary. 


## Adding a parser to SARCAT 
### Technical Requirements for adding a parser to SARCAT

#### SARCAT Identification File: ".sarcat"

Currently, the only supported languages for raw file parsing are bash, nodejs, and python (imported module functions via  [pythonia](https://www.npmjs.com/package/pythonia) or directly called via CLI with args). 

Powershell, GoLang and Rust are welcome and can be supported.

#### .sarcat file format and contents


{
"uuid":"UUID v4 (e.g., 81a858d5-b8fc-4995-bcff-01a93710c7bd)",
"author":"email address/addresses of those responsible for developing / maintaining the parser",
"organization": "Organization responsible for maintaining the parser",
"assessmentTypes": ["vulnerability", "configuration", "evidence"],
"rawDataVendor":"Org that makes the application",
"rawDataTool":"Name of application that produces the raw output",
"rawDataVersion": "1.0",
"rawDataFomat": "output format of the raw file to be parsed (e.g., xml, json, txt, csv proprietary etc....)",
"rawDataEncoding": "encoding of the data in the raw file to be parsed (e.g., utf-8, ascii, buffer, base64 etc....)",
"fileExtensions": ["array of extensions to be associated with the parser"], <- this is also the easiest way to specifically choose a parser. RawScanOutput files with matching extensions will be parsed by the parser that is associated with that extension
"language":{"name":"nodejs", "version":"v18.8.0"}, <- frameworks on the container are managed and installed by environment management tools such as nvm and conda
"dependencies": [{"name":"fast-xml-parser","version": "4.0.9"},{"name":"he", "version":"1.2.0"}], <- used to install dependencies e.g., "npm i he@1.2.0"
"packageManagerCommand": "npm i",
"runCommand": {"shellExecutable": "node", "fileName":"nessus.mjs", "relativeDirectoryPath": "./"}, <-nrelative to the ".sarcat" file
"outputToConsole": "true or false",
"isPkg": "true or false"
}

| Field Name | Value Example | Type | Notes |
|--|--|--|--|
| uuid | 81a858d5-b8fc-4995-bcff-01a93710c7bd | string(36) | author generated v4 UUID |
|author|["john.doe@abc.com", "jane.doe@def.com"]|string[]|author email addresses / maintainer POCs|
|toolName|"nessusParser"|string|version of the parser|
|version|"v1.0.0"|string|version of the parser|
|organization|"ABC Corp."|string|organization that develops and maintains the parser|
|assessmentTypes|["vulnerability","configuration"]|string[]|purpose/use case of parsed data |
|rawDataVendor | "Tenable" | string | Vendor that creates the tool used to produce the raw output data |
|rawDataTool|"Nessus"|string|Name of the tool used to produce the raw output data|
|rawDataVersion|"v1.0"|string|Version of the **raw data output**. *Not the version of the tool* |
|rawDataFomat|"xml"|string|Data structure of the file contents|
|rawDataEncoding|"utf-8"|string|Encoding of the file data|
|fileExtensions|["nessus","xml"]|string[]|File extensions to be associated with the parser. Note: you can use custom file extensions to force usage of a specific parser by default (e.g.,raw file names "rawScanFile.myparser" and parser file extensions ["myparser"] or simply add a config.sarcat file to your directoy of raw scan files. |
|run|{"method":"import","function":"parse"}|{method: string, function: string}|Method must be import or direct. Import will use the code natively and the function must accept two argument inputs (fullpath of filename to parse, fullpath of output filename). Will consider supporting IPC channel streaming if it is of value. Shell will spawn a new process and run it using the identified shell executable. If shell is the method, function should be null. Same input requirements required except they are provided via shell command arguments|
|language|{"name":"nodejs","version":"v18.8.0"|{name: string, version:string}|Coding language and version of the parser|
|dependencies|[{"name":"he", "version":"v1.2.0"}]|{name: string, version: string}[]|Array of objects for the dependencies required by the parser to run. Note: |

|packageManagerCommand|"npm i"|string|The command used to install dependencies|
|runCommand|{"shellExecutable": "node", "fileName": "parse.mjs", "relativeDirectoryPath": "./"}|{shellExecutable: string, fileName: string, relativeDirectoryPath: string}|Required inputs to run the parser|
|outputToConsole|boolean|boolean||
|*isPkg<sup>2</sup>*|boolean|boolean|*Future consideration for all custom parsers made installable via npm, yarn, pip etc...*|



### *Output validation testing*
Future feature
### Methods and Execution
nodejs execute the run command and arguments as follows:

    ${shellCommand} ${modulePath}${relativeDirectoryPath}${fileName} ${filePathToParse}${fileNameToParse} ${outputPath}${outputFilename}

Note: There are no slashes added therefore they must be included correctly by the author in the following SARCAT ID fields:
- runcommand.fileName
- runcommand.relativeDirectoryPath (use "./" for same directory as the .sarcat file")

Automated registration will pickup and run  
The parser can be ran one of two way:

 1. As a separate process: Path and Filename for both the file to parse and the output must be captured by the parser via CLI / Shell
 2. As an imported 

SARCAT injects the paths and filenames when the parser is called.

All data inputs and outputs must be streaming otherwise larger filesizes will cause parsing to fail.  

The parser must output properly formatted JSON files.

If outputToConsole is set to true, all other outputs such as error or messages will break formatting and result in failure. Meaning all parsing data output should either be in a specific designated output file (e.g., path supplied by command line argument) or if to console, then all other outputs such as errors, warning, or info should be written to file only outputting parsed data to console.