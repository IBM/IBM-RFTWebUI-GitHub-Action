## Rational Functional Tester - UI Test GitHub Action
Rational Test provides software testing tools to support a DevOps approach: API testing, functional testing, UI testing, performance testing and service virtualization. It helps you automate and run tests earlier and more frequently to discover errors sooner - when they are less costly to fix.

This action enables the ability to run WebUI tests from Rational Functional Tester projects on self hosted runners.

## Usage

### Prerequisites

Configure a self hosted runner on a suitable machine
1. The host machine will need a licensed copy of Rational Functional Tester and the UI projects containing the tests you wish to run.
2. [Add a self-hosted runner](https://docs.github.com/en/actions/hosting-your-own-runners/adding-self-hosted-runners) at an appropriate level, ensure you have configured network access.
3. If you are using more than one runner [assign it a suitable label](https://docs.github.com/en/actions/hosting-your-own-runners/using-labels-with-self-hosted-runners) and note this for later.

### Setup
In the repository you want to apply the action to
1. Create a folder named ".github/workflows" in the root.
2. Create a .yml file with any name, inside the ".github/workflows" folder based on the following example content:

```yaml

name: Rational Functional Tester - UI Test

on: workflow_dispatch

jobs:
    WebUI-Action:
        name: Rational Functional Tester - UI Test
        runs-on: self-hosted
        steps:
         - name: Rational Functional Tester - UI Test
           uses: IBM/devopsauto-uitest-actions@main
           with:
            workspace: <C:\Data\SampleWorkspace>
            project: <MyProject>
            suite: <MySuite>
```

3. Update the parameterized items to refer to your project and tests (see parameter details below).
4. If you have more than one runner configured, then use its label as the argument for **runs-on**.
5. Push your updated yml file to the repository.
6. Go to the Actions section in the repository and select the workflow.
7. Click the Run workflow dropdown and the list of input boxes get displayed.

As an alternative to having all of the test projects pre-configured on the runner machine, users could leverage other actions within the workflow to pull down the latest copies. 

### Required Parameters
- **workspace** Complete path to Eclipse workspace.
- **project** The name of the project containing the test.	
- **suite** The relative path from the project to the test including the file name of the test. A test can be WebUI test, Compound test, Performance schedule or Accelerated Functional Test (AFT) suite. The test suite name must contain the file extension when it is an AFT suite. To run multiple tests from the same project sequentially, you must separate the tests by a colon (:). If you provide multiple tests, you cannot include an AFT suite along with it.

### Optional Parameters
- **imShared** Complete path to IBMIMShared location, if it is not at default location.
- **configFile** The complete path to a file that contains the parameters for a test or schedule run, If Config file is specified then no other fields will be required.
- **exportLog** You can use this parameter to specify the file directory path to store the exported HTTP test log. You can provide multiple parameter entries when running multiple tests. You must use a colon to separate the parameter entries. For example: c:/logexport.txt:c:/secondlogexport.txt
- **exportReport** Use this option to export the unified report of UI tests to the file formats such as PDF, HTML, and XML. For example, to export the report to only the PDF format, you can input "type=unified;format=pdf;folder=Exportedreport102;filename=testreport"
- **exportStatReportList** You can use this option to specify a comma-separated list of report IDs along with exportstats or exportstatshtml to list the reports that you want to export in place of the default reports, or the reports selected under Preferences. To retrieve the report IDs, navigate to Window > Preferences > Test > Performance Test Reports > Export Reports from Rational Functional Tester and under Select reports to export, select the required reports, and click Copy ID to clipboard.
- **exportStats** The complete path to a directory in which to store exported statistical report data.
- **exportStatsFormat** Use this field to enter one or more formats for the reports that you want to export by using a comma as a separator. The options are simple.csv, full.csv, simple.json, full.json, csv, and json. When you want to export both simple and full reports in json or csv format, you can specify json or csv as the options. The reports are saved to the location specified in the exportStats field. This field must be used in conjunction with exportStats field.
- **exportStatsHtml** Specify the complete path to a directory in which to export web analytic results. Analyze the results on a web browser without using the workbench. If you are running multiple tests, do not provide a value in this field.
- **imports** Path of the Project location to be imported. You can also use an empty workspace folder on your computer to import the UI Test project resources and run the tests. 
- **labels** Use this option to add labels to test results. To add multiple labels to a test result, you must separate each label by using a comma.
- **overwrite** Determines whether a result file with the same name is overwritten. The default value, false, indicates that the new result file is created. If the value is true, the file is overwritten and retains the same file name.
- **protocolInput** You can use this option with additional arguments as follows:
   - To run a Web UI test in parallel on different browsers, input the value: all.available.targets.in.parallel=all
   - To specify the Web UI preferences such as highlighting the page element and capturing screens, input the value: webui.highlight=value;webui.report.screenshots=value
   - To run only the failed tests from a previous playback in an Accelerated Functional Test suite, input the value: runfailedtests=true
   - To automatically resolve the browser and driver incompatibility, while you play back the Web UI tests, input the value: webui.browser.driver.autoupdate=true
   - To apply guided-healing and self-healing features while you run Web UI tests, input the value: autoheal=true
- **publish** You can use this parameter to publish test results to the Server. The format is: serverURL#project.name=projectName&amp;teamspace.name=teamspaceName. If the name of the project or team space contains a special character, then you must replace it with %<Hex_value_of_special_character>.
- **publishFor** You can use this option to publish the test results based on the completion status of the tests. The supported values are FAIL,PASS,INCONCLUSIVE,ERROR,ALL.
- **publishReports** You can use this option to publish specific test results to the Server. The supported values are FT, STATS, TESTLOG.
- **results** Specify a name for the results file. If you do not specify a name, the test or schedule name appended by the timestamp is used for the name. The results file is stored in the Results directory. If you are running multiple tests, do not provide a name for the results file.
- **swapDatasets** Use this option to replace dataset values during a test or schedule run. You must ensure that both original and new datasets are in the same workspace and have the same column names. You must also include the path to the dataset. For example, /project_name/ds_path/ds_filename.csv:/project_name/ds_path/new_ds_filename.csv
- **userComments** Add text to display it in the User Comments row of the report.
- **varFile** The complete path to the XML file that contains the variable name and value pairs.
- **vmArgs** To specify the Java™ maximum heap size for the Java™ process that controls the command line playback, use the option with the -Xmx argument. For example, when you input -Xmx4096m, it specifies a maximum heap size of 4096m. To execute tests in parallel on all mobile devices, which are in passive mode, connected to the workbench and ready for playback, input the value: "-Dall.available.targets.in.parallel=true". To execute tests in parallel on all supported desktop browsers and connected mobile devices, input the value "-Dall.available.targets.in.parallel=all". To execute tests in parallel on selected desktop browsers and connected mobile devices, input the value "-Dall.available.targets.in.parallel=browser1,browswer2,browser3". You must separate browser names with a comma. For example, firefox, ff, chrome, ie, ie64, safari, "-Dall.available.targets.in.parallel=browser1,browser2,browser3".
