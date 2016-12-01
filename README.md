# synthetics-manager

This project has two goals:
* Be able to run your New Relic Synthethics locally. This lets you do things like debugging.
* Manage your New Relic Synthetics from the command line. Create new synthetics or update existing ones. This allows you to store the synthetics code under source code control.

## Description

This tool is designed to allow creating, writing and managing New Relic scripted browser synthetics. This allows synthetics to be run and debug on a local machine and then push them to New Relic when ready. This also allows synthetics to be stored in a SCM system to track changes and have a CI system push them to New Relic in an automation fashion.

synthetics-manager contains both a command line tool and a library. 

The synthmanager command line tool allows the create, update and import synthetics to/from New Relic. 

The synthetics-manager library provides the setup needed to simulate the environment that synthetics are run in New Relic. This is added to your synthetics with "require ('synthetics_manager')" line of code at the top of your local synthetics script. This line should NOT be removed or changed. It is automatically striped out of the synthetic code when it is uploaded to New Relic.

## Getting Started

Install the synthmanager command via npm to allow you to use the command line tool:
```
$ npm install synthetics_manager -g
```

Next, create an npm project to store our synthetics:
```
$ mkdir syntheticsProject
$ cd syntheticsProject
$ npm init
```

After creating a project, add synthetics_manager as a dependency:
```
$ npm install synthetics_manager --save
```

Now, new synthetics can be created with the 'synthmanager create' command:
```
$ synthmanager create --name "New Synthetic Name" --filename newSyntheticName.js
```

This will create the synthetic in New Relic and create a js file with the specified name under the synthetics directory. The js file has some basic setup needed to run the synthetic locally.

After, when the synthetics code has been completed, it can be uploaded to New Relic with the 'synthmanager update' command: 
```
$ synthmanager update --name "New Synthetic Name"
```

## Running Synthetics locally

### Prerequisites

New Relic runs synthetics using the chrome web browser. So that is the recommended way to run them locally. In order to run them, the following need to be setup:
* Chrome Web browser
* Selenium Server (http://www.seleniumhq.org/download/), should be running
* Chrome Driver (https://github.com/SeleniumHQ/selenium/wiki/ChromeDriver), should be in the path

### Running Synthetics

Once the prerequisites are installed and a synthetic is created, it can be run locally. This can be done with an IDE or using node:
```
$ node synthetics/newSyntheticName.js
```

## Command Line Usage

### Create a new synthetic

```
synthmanager create --name <synthetic_name> --file <filename>
```

Create a synthetic in New Relic and a file to contain the synthetic code.
* --name <synthetic_name> - Name of synthetic. This is the name used in New Relic as well as how it should be refered to by other commands
* --file <filename> - Filename where the synthethics code should go. This file will be created under the 'synthetics' directory. The file should not already exist.
* --frequency <frequency> - Frequency to run the synthetic in minutes. This should be an integer. Possible values are:  1, 5, 10, 15, 30, 60, 360, 720, or 1440.
* --locations <location> - Locations to run the synthetic. This can be specified multiple times to specify multiple locations.

### Update New Relic with synthetics code

```
synthmanager update --name <synthetic_name>
```

Update New Relic with the latest synthetic code for the specified synthetic.
* --name <synthetic_name> - name of synthetic to update. This should be the name used when the synthetic was created.

### Import a synthetic from New Relic

```
synthmanager import --name <synthetic_name> --id <synthetic_id> --file <filename>
```

Import an existing synthetic from New Relic.

## Configuration

Configuration options can be changed by adding a 'synthetics.config.json' file in the base of the project. 

Available configuration options are:
* apikey - Synthetics API key (Note: There may be security issues storing this value in a file).
* syntheticsDirectory - Directory to store synthetics file in (default: './synthetics/').
* syntheticsListFile - File to store information about created synthetics in (default: './synthetics.json').



## TODO

* import synthetics
* include git information
