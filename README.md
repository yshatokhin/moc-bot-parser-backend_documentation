## Available Scripts

In the project directory, run the app in development mode using:

`npm run start`

The running application will be available on [http://localhost:5001](http://localhost:5001) to use in Postman.

To run the app in development mode using nodemon module:

`npm run demonize`

## Get the Postman collection

1. If you have not already installed Postman, visit the [Postman website](https://www.getpostman.com/) and install the preferred version for your system.

2. Press the following **Run in Postman** button to open Postman and import the **MOC Bot Parser** Postman collection.

[![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/e9196a680f86e8ae2d82?action=collection%2Fimport)

## Call the Parse bot JSON file endpoint

1. You can define your own port value in **Collection variables**, column **Current value**:

- Select **Collections** in the sidebar.
- Select the **MOC Bot Parser** Postman collection, and then select the **Variables** tab:
  ![Edit Collection Variable](/assets/images/collection_variable_port.png)

2. Open the **MOC Bot Parser** folder, and then choose **Parse bot JSON file**.
3. Choose the **Body** tab and select **form-data** to see the parameters list:
   ![Edit Collection Body Parameters](/assets/images/collection_body_parameters.png)
4. Define necessary parameter values.
5. Press Send.

## Parameter options

<!-- prettier-ignore -->
| Name | Required or optional | Description |
| --- | --- | --- |
| botJsonFile | Required | Specify the bot JSON file that was exported from Conversation Builder |
| outDir | Required | Output directory for parsed data |
| dialogNameReplaceSpacesWithUnderscores | Optional | Directory naming: replace spaces with underscores in dialog names |
| showDialogName | Optional | Show dialog name in interactions code as the very first commented row, ex) "// 21_1_REFUND_ORDER -> RO_agent_escalation_declined" |
| enableNumbering | Optional | Enable interaction numbering in directory names. Suggested for bot analysis in VS Code or any other editor when you open the bot parsed directory in case, if you want to have the interactions ordered in the same sequence as in the bot |
| numberLength | Optional | Related to the "enableNumbering" parameter. The assumed length of the number, which is the maximum number of interactions in one dialog (10 and fewer interactions: 2; 100 and fewer interactions: 3, 1000 and fewer interactions: 4, etc.). Default value equals 2. |
| addInteractionTypeToFolderName | Optional | Add interaction type to the interaction folder name, like an extension in the filename (for example, "WELCOME_list_GBM.QUICK_REPLY") |
| addTimeStampToOutDir  | Optional | Add timestamp to the output directory name |
| outDirSuffix | Optional | Suffix for name of output directory containing the parsed data |
| skipDisabled | Optional | Skip disabled dialogs and interactions |
| copyJsonFile | Optional | Copy the original bot JSON file to the output directory with parsed data |
| interactionTypes | Optional | Filter by interactionType. Interaction types should be separated by commas. Available types can be found as a value of "interactionType" field in _interactionMetaData.json. For example: TEXT, LIST_PICKER, QUICK_REPLY, APPLE_RICHLINK, BUTTON, TEXT_QUESTION, STRUCTURED, DIALOG_STARTER, API_INTEGRATION, ESCALATION, UNIVERSAL |

## The Purpose of the application

### Structure of the parsed data folder

In the output directory with parsed data, you will get a parsed bot which can be opened in VS Code or another editor, with the following folder structure:

![Collection Structure](/assets/images/collection_structure.png)

<!-- prettier-ignore -->
| Folder name | Description | Additional information |
| --- | --- | --- |
| [botInfo](#botinfo-folder) | Bot information about dialog count, interactions count, etc. | |
| botOriginalJson | Source bot JSON file | The folder is created if [copyJsonFile](#parameter-options) parameter value is **true**. In most cases, I recommend setting this parameter value to **false** or disable in Postman |
| botOriginalJson_sortedByKey | Bot JSON file where keys were sorted by name | The folder is created if [copyJsonFile](#parameter-options) parameter value is **true**. In most cases, I recommend setting this parameter value to **false** or disable in Postman |
| [globalFunctions](#globalfunctions-folder) | js file containing global functions script | |
| [integrations](#integrations-folder) | The list of the bot integrations as a separate folder. Inside each folder: |
| | \_postBody.json - integration postBody |
| | \_transformResultsScript.js - integration transformResultsScript |
| | \<integration name\>.json - file contains the rest integration data |
| [_dialogMetaData.json](#_dialogmetadatajson-file) | Dialog metadata | |
| [interactionsList.json](#interactionslistjson-file) | Interactions list | |
| [interactions](#interactions-folder) | The list of the bot dialogs as a separate folder. Inside each folder: | |
| | Folder \<interaction name\>, inside each folder | |
| | preProcessMessage.js | js file contains preProcessMessage source code, if source code is missing, then the file is not created |
| | processUserResponse.js | js file contains processUserResponse source code, if source code is missing, then the file is not created |
| | postProcessMessage.js | js file contains postProcessMessage source code, if source code is missing, then the file is not created |
| | \_interactionMetaData.json | File contains the rest of the interaction data, E.g. text, rules, prev/next interaction, etc. |
| [_dialog_starter_metadata.json](#_dialog_starter_metadatajson-file) | File contains information about all dialog starters, used patterns, intents, and domains | |

### botInfo folder

![Bot info folder](/assets/images/bot_info.png)

### globalFunctions folder

![Global functions folder](/assets/images/global_functions.png)

### integrations folder

![Integrations folder](/assets/images/integrations.png)

### interactions folder

Parameter [showDialogName](#parameter-options) controls the appearance of the first commented line [Dialog name] -> [Interaction name].
For example:
`// 03_LINK_ACC -> LINK_ACC_api_change_order_owner`

![Interactions folder](/assets/images/interactions.png)

Parameter [showDialogName](#parameter-options) controls the appearance of the first property in the JSON file [Dialog name] -> [Interaction name].
For example:
`"_interactionPath": "03_LINK_ACC -> LINK_ACC_api_change_order_owner",`

![Interaction metadata folder](/assets/images/interaction_metadata.png)

If a dialog is disabled then the word `_DISABLED` will be concatenated with the dialog name and interaction folder, for example:

![Disabled dialog](/assets/images/dialog_disabled.png)

To skip disabled dialogs and interactions and **not create folders**, set Parameter [skipDisabled](#parameter-options) to **true**.

### \_dialogMetaData.json file

Contains some dialog information
![_dialogMetaData.json file](/assets/images/dialogMetaData.png)

### interactionsList.json file

A list of interactions in the selected dialog, ordered in the same sequence as in the bot
![interactionsList.json](/assets/images/interactionsList.png)

### \_dialog_starter_metadata.json file

A list of patterns, intents, and domains of every dialog starter interaction
![_dialog_starter_metadata.json](/assets/images/dialog_starter_metadata.png)

## Comparing and analyzing bot changes

In the examples below, you will see the differences between the In-app bot "before" and "after" some changes.

1. Overview of the differences: in the screenshot below, we can see that changes have been made in the bot integrations and interactions:
   ![Shortest view of differences](/assets/images/compare01.png)

2. Expand folders to see more detailed differences in specific integrations and dialogs, as in the screenshot below:
   ![Differences in integrations and dialogs](/assets/images/compare02.png)

3. \_integrationsList.json - in the screenshot below, we can see which integrations and endpoints have been changed:
   ![Differences in integrations](/assets/images/compare03.png)

4. GetRestaurantInfo.json - in the screenshot below, we can see what exactly has been changed:
   ![Differences in integrations](/assets/images/compare04.png)

5. The most detailed level of the differences - we can see which parts of source code or interaction text, etc. have been changed:
   ![The most detailed level of the differences](/assets/images/compare05.png)

6. Open the dialog folder to see which interactions have been changed. Red folders are changed interactions, purple folders are deleted interactions. See example in screenshot below:
   ![deleted and changed interactions](/assets/images/compare10.png)

7. preProcessMessage.js: the differences in the preProcessMessage of the interaction, the same is true for postProcessMessage and processUserResponse:
   ![changes of the interaction source code](/assets/images/compare14.png)

8. \_interactionMetaData.json: the differences in the interaction, in the screenshot below we can see that "Next Action: Go To:" has been changed from "REFUND_PAST_digital_order_q" to "RO_get_order_type_q" value.
   ![changes of the interaction](/assets/images/compare16.png)

9. \_interactionMetaData.json: the differences in the interaction text
   ![changes of the interaction](/assets/images/compare17.png)

10. There is an "interactionsList.json" file in the root of each dialog folder. It contains the list of interactions in the dialog, ordered in the same order as in the bot. In the screenshot below, we can see which interactions have been deleted from the bot:
    ![changes of the interactions list](/assets/images/compare22.png)
