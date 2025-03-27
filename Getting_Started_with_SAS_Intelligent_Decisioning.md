# Getting Started with SAS Intelligent Decisioning

* [Exercise Description](#exercise-description)
* [Log in to SAS Viya](#log-in-to-sas-viya)
* [Load CAS Data In-Memory](#load-cas-data-in-memory)
* [Create and Test an Assignment Rule Set](#create-and-test-an-assignment-rule-set)
* [Create and Test a DS2 Code File](#create-and-test-a-ds2-code-file)
* [Create and Test a Decision](#create-and-test-a-decision)
* [Exercise Completed](#exercise-completed)

## Exercise Description

Are you interested in using SAS Viya to automate your business decisions and drive real-time customer interactions, but don't know where to begin? In this hands-on workshop, you'll learn how to create a decision step-by-step. Use the SAS Intelligent Decisioning interface to create rule sets and other decision elements, arrange decision objects in a workflow, and test completed decisions for accuracy.

## Log in to SAS Viya

Open a new window in the *Google Chrome* browser and select the **SAS Viya** bookmark.

* ID: **student**
* Password: **Metadata0**

Select **No** when prompted about accepting *Admin* privileges.

## Load CAS Data In-Memory

1. Select ![Viya Menu Selector](img/HamburgerMenu.png) **&#10132; Manage Data** to open *SAS Data Explorer*.
1. Expand the **cas-shared-default** server to view the listing of CAS libraries.
1. Select the **DM** CAS library to view its contents.
1. Select the **LOAN_APPLICANTS.csv** table and click ![Lightning Bolt icon](img/LightningBolt.png) to load the table into memory.

   ![SAS Data Explorer](img/DataExplorer.png)

1. Select the **In-memory data (available)** source to confirm the **LOAN_APPLICANTS** table is now listed there.

   ![In-memory Data](img/InMemoryData.png)

1. Select the **LOAN_APPLICANTS** table hyperlink to view its *Details*.
1. Select the **Sample data** tab to view a sample of the data set.
1. Click the **x** in the right-corner of the window to close the table.

   ![Sample data](img/SampleData.png)

## Create and Test an Assignment Rule Set

1. Select ![Viya Menu Selector](img/HamburgerMenu.png) **&#10132; Build Decisions** to open *SAS Intelligent Decisioning*.
1. Select ![Rule Sets Icon](img/RuleSetsIcon.png) to open the **Rule sets** page.
1. Click **New rule set** to create a new rule set.

     ![Create new rule set](img/NewRuleSet.png)

1. Enter the following information for the new rule set:
    * Name: **Initial_Loan_Review**
    * Type: **Assignment**
    * Description: **Initial Loan Review Assignment Rule**
    * Location: **/Public/SID Workshop**

        > &#9755; Select ![Folder icon](img/FolderIcon.png) to navigate to *SAS Content/Public*, then select ![New](img/New.png) **&#10132; Folder** to create a new folder named *SID Workshop* if the workshop folder does not already exist.

        ![Initial_Loan_Review settings](img/Initial_Loan_ReviewSettings.png)

1. Click **Save** to create the rule set.
1. On the **Variables** tab, ensure that the **Rule Set Variables** sub-tab is selected.
1. Select **Add variable &#10132; Data table** to add variables from an *in-memory* table.

   ![Add rule set variables](img/AddRuleSetVariables.png)

1. Select ![Folder icon](img/FolderIcon.png) in the **Data table** selection.
1. In the *Choose Data* dialog, select *Recent* from the drop-down menu next to *Show*, then select **LOAN_APPLICANTS** from the available data list and click **OK**.

   ![Select LOAN_APPLICANTS](img/SelectLoanApplicants.png)

1. Click ![Select All](img/SelectAll.png) to select all the columns from the table and add them to the selected items side.

   ![Select all variables](img/SelectAllVariables.png)

1. Click **Add** to add the selected items as variables for the rule set.

   ![Added variables](img/AddedVariables.png)

    > &#9998; The variables are automatically added as both *Input* and *Output* variables. You can uncheck those selections as appropriate for your rule set.

1. To add some output custom variables, select **Add variable &#10132; Custom variable**.
1. Expand the **Optional** section.
1. Enter the following:
   * Name: **STATUS**
   * Data type: **Character**
   * Description: **Loan Status**
   * Length: **8**

   ![Custom variable settings](img/CustomVariableSetttings.png)

1. Click **Add**.
1. Uncheck **Input**.

     ![Add custom variable](img/AddCustomVariable.png)

1. Repeat steps 13-15 to add the following additional output custom variables:

| Name | Data type | Input | Output | Length | Initial Value | Description |
|  :---:  |  :---:  |  :---:  |  :---:  |  :---:  |  :---:  |  :---:  |
| JUSTIFICATION  | Character |       | checked | 50 | UNDER REVIEW | Justification for Loan Approval Status |
| OFFER_RATE | Decimal |       | checked |       |       | Interest Rate to Offer with Loan Approval |

   ![Additional custom variables](img/AdditionalCustomVariables.png)

1. Click **OK** to add the custom variables to the rule set.
1. Click ![Save button](img/SaveButton.png) to save the rule set.

    ![Updated variables](img/UpdatedVariables.png)

1. Select the **Rule set** tab.
1. Click **Add other**.
1. Select **Assignment** for the *rule type* and click **OK**.

   ![Add assignment](img/AddAssignment.png)

1. For the action assignment, select **ASSIGN STATUS 'REVIEW'**.
1. Click **+ Add Rule** to add a rule block.

   ![Initial loan review - assign status](img/InitialLoanReview1.png)

1. Name the rule **Automatic Loan Denial or Approval**.

   > &#9755; Select ![More options](img/MoreOptions.png) **&#10132; Edit rule information**.

1. Select the following for the rule:<br>
   **IF BAD = 1**<br>
   **THEN ASSIGN STATUS 'DENIED'** <br>
    **&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;ASSIGN JUSTIFICATION 'AUTOMATIC DENIAL CRITERIA MET'**

    > &#9755; Hover over the first *ASSIGN* statement to surface statement options, then click ![Add/Plus](img/AddPlus.png) to add the second *ASSIGN* statement.

     ![Initial loan review - automatic denial](img/InitialLoanReview2.png)

1. Click ![Save button](img/SaveButton.png) to save the rule set.
1. Click **+ Add Rule** to add a new rule block.
1. Use the drop-down menu to change the **IF** to an **OR** to add an *OR* condition to the existing rule block.
1. Select the following for the *OR* condition:<br>
   **OR DEBTINC > 50.0** <br>
   **AND DEROG >= 2**

     > &#9755; Hover over the *OR* statement to surface statement options, then click ![Add/Plus](img/AddPlus.png) to add the *AND* statement.


   ![Initial loan review - add offer rate](img/InitialLoanReview3.png)

1. Click **+ Add Rule** to add a new rule block.
1. Change the **IF** to an **ELSE** to add an *ELSE IF* condition to the existing rule block.
1. Select the following for the *ELSE* condition: <br>
   **ELSE BAD = 0** <br>
   **AND LOAN <= 15000** <br>
   **AND DEBTINC < 10.0** <br>
   **THEN ASSIGN STATUS 'APPROVED'** <br>
   **&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;ASSIGN JUSTIFICATION 'AUTOMATIC APPROVAL CRITERIA MET'** <br>
   **&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;ASSIGN OFFER_RATE 2.75**

   > &#9755; Click ![Add/Plus](img/AddPlus.png) to add the *AND* statements and additional *ASSIGN* statements.

   ![Initial loan review - automatic approval](img/InitialLoanReview4.png)

1. Click ![Save button](img/SaveButton.png) to save the rule set.
1. To test the rule set, select the **Scoring** tab.
1. On the **Tests** sub-tab, click **New Test**.
1. Enter the following information:
   * Name: **Initial_Loan_Review_Test**
   * Description: **Test 1**
   * Location: **/Public/SID Workshop/Tests**
      > &#9755; Select ![Folder icon](img/FolderIcon.png) to navigate to *SAS Content/SID Workshop*, then select ![New](img/New.png) **&#10132; Folder** to create a new folder named *Tests* if the test folder does not exist already.
   * Input table: **LOAN_APPLICANTS**.

   ![Test rule set](img/TestRuleSet.png)

1. Click **Variables** to view the variables mapping between the input table and the rule set variables.

     > &#9998; All the variables are automatically mapped since the variable names on the rule set match the names on the input table.

1. Click **OK** to close the *Variable Mappings* dialog.
1. Click **Save** to save this test to the rule set.

     > &#9998; If you do not select **Save** and simply select **Run**, then the test is run; however, the test is not saved to the rule set and if your session is closed, you will need to set up the test again to run it in the future.

1. Check the newly created test and click **Run** to run the specified test.

     ![Select test](img/SelectTest.png)

     > &#9998; It may take a few seconds for the test to complete.  You can select ![Refresh](img/Refresh.png) to refresh the test status.

1. Once the test run is complete, select ![Result table](img/ResultTable.png) to view the test results.
1. Click **Rule-Fired Analysis** and **Run Rule-Fired Analysis** to view which records triggered a rule.

   ![Rule-fired analysis](img/RuleFiredAnalysis.png)

1. Once the rule-fired analysis has run, review the results on the *Analysis* tab.

   ![Test output](img/TestOutput.png)

1. Click **x** to close the test results and the rule set.

## Create and Test a DS2 Code File

1. Select ![Code File Icon](img/CodeFileIcon.png) to open the **Code files** page.
1. Click **New code file** to create a new code file.
1. Enter the following information:
    * Name: **Additional_Review**
    * Type: **DS2 Code File** (default selection)
    * Description: **Additional review for requests that have not been approved or denied**
    * Location: **/Public/SID Workshop**

     ![DS2 code 1 - settings](img/DS2Code1.png)

1. Click **Save** to create the DS2 Code file.
1. Replace the default code with the code block below:

    ```sas
    package "${PACKAGE_NAME}" /inline;
       method execute(in_out double CREDIT_SCORE,
                      in_out double INCOME,
                      in_out double LOAN,
                      in_out double OFFER_RATE,
                      in_out varchar JUSTIFICATION,
                      in_out varchar STATUS);

            if CREDIT_SCORE > 650 AND LOAN < INCOME then
                do;
                   STATUS = 'APPROVED';
                   JUSTIFICATION = 'APPROVED BASED ON CREDIT SCORE';
                   OFFER_RATE = 5.5;
                end;
            else if (CREDIT_SCORE >= 525 AND CREDIT_SCORE <= 650) AND LOAN  < 0.7 * INCOME then
                do;
                   STATUS = 'APPROVED';
                   JUSTIFICATION = 'APPROVED BASED ON CREDIT SCORE';
                   OFFER_RATE = 10.75;
               end;
            else
               do;
                   STATUS = 'DENIED';
                   JUSTIFICATION = 'DENIED BASED ON CREDIT SCORE AND/OR LOAN AMOUNT';
               end;
       end;
    endpackage;
    ```

     ![DS2 Code 2 - copy/paste](img/DS2Code2.png)

1. Click ![Save button](img/SaveButton.png) to save the code file.
1. Click **Sync variables** to add the variables declared in the DS2 code to the *Variables* tab for for the code file.
1. Select the **Variables** tab to confirm the variables were added.

     ![DS2 Code Variables](img/DS2CodeVars.png)

1. To test the code file, select the **Scoring** tab.
1. On the **Tests** sub-tab, click **New Test**.
1. Enter the following information:
   * Name: **Additional_Review_Test**
   * Description: **Test 1**
   * Location: **/Public/SID Workshop/Tests**
      > &#9755; Select ![Folder icon](img/FolderIcon.png) to navigate to *SAS Content/SID Workshop/Tests*.
   * Input table: **LOAN_APPLICANTS**.

   ![Test DS2 code](img/TestDS2Code.png)

1. Click **Variables** to resolve the input variable mapping.
1. Enter the following information for unmapped variables:

   | Variable | Type | Table Column | Value |
   |  :---:  |  :---:  |  :---:  |  :---:  |
   | OFFER_RATE | Decimal | **Use value** | **0** |
   | JUSTIFICATION  | Character | **Use value** | **UNDER REVIEW** |
   | STATUS | Character | **Use value** | **REVIEW** |

   ![Fix variable mappings](img/FixVarMaps.png)

1. Click **OK** to close the *Variable Mappings* dialog.
1. Click **Save** to save this test to the code file.
1. Check the newly created test and click **Run** to run the specified test.

     ![Select test](img/SelectTest.png)

     > &#9998; It may take a few seconds for the test to complete.  You can select ![Refresh](img/Refresh.png) to refresh the test status.

1. Once the test run is complete, select ![Result table](img/ResultTable.png) to view the test results.
1. Review the results on the *Output* tab.

   ![DS2 test output](img/DS2TestOutput.png)

1. Click **x** to close the test results and the code file.

## Create and Test a Decision

1. Select ![Decision icon](img/DecisionIcon.png) to open the **Decisions** page.
1. Click **New decision** to create a new decision.
1. Enter the following information for the new decision:
   * Name: **Loan_Request_Review**
   * Description: **Loan request review process for ABC Bank.**
   * Location: **/Public/SID Workshop**

     > &#9755; Select ![Folder icon](img/FolderIcon.png) to navigate to this folder in *SAS Content*.

      ![New Decision](img/NewDecision.png)

1. Click **Save** to create the decision.
1. On the *Start* node, select ![More options](img/MoreOptions.png) **&#10132; Add below &#10132; Rule set**.

     ![Add rule set](img/AddRuleSet.png)

1. Navigate to the **SAS Content &#10132; Public &#10132; SID Workshop** folder.
1. Select the **Initial_Loan_Review** rule set.

     ![Select Initial_Loan_Review](img/SelectInitialLoanReview.png)

1. Click **OK** to add the selected rule set to the decision.

     > &#9998; There is an error since the variables from the rule set are not part of the decision.

1. Select ![More options](img/MoreOptions.png) **&#10132; Add missing variables**.

     ![Add missing variables](img/AddMissingVars.png)

1. Click **Add** to add all the missing variables to the decision.
1. Click ![Save button](img/SaveButton.png) to save the decision.
1. Select the **Variables** tab to review the added variables.

     ![Decision variables](img/DecisionVars.png)

1. Select the **Decision Flow** tab.
1. On the *Initial_Loan_Review* node, select ![More options](img/MoreOptions.png) **&#10132; Add &#10132; Branch**.

     ![Add branch](img/AddBranch.png)

1. Enter the name **Loan Status** and select **Equals** for the branch type.

     ![New branch](img/NewBranch.png)

1. Click **OK** to create the branch.
1. In the *Properties* section, select the **STATUS** variable for the *Branch expression*.

     > &#9755; Select the drop-down menu under *Branch expression* to view the full list of variables.

1. Click ![Add/Plus](img/AddPlus.png) to add the following *paths*:
   * **"REVIEW"**
   * **"APPROVED"**
   * **"DENIED"**.

    > &#9998; The *Other* path is automatically added for this branch type.

1. Click ![Save button](img/SaveButton.png) to save the decision.

     ![Branches](img/Branches.png)

1. Right-click the path with the *"REVIEW"* branch and select **&#10132; Add &#10132; DS2 code file**.

   ![Add DS2](img/AddDS2.png)

1. Navigate to the **SAS Content &#10132; Public &#10132; SID Workshop** folder.
1. Select the **Additional_Review** DS2 code file and click **OK** to add the selected file to the decision.

     ![Select DS2](img/SelectDS2.png)

1. Click **Save** to save the decision.
1. Click **Validate** to validate the decision.

   ![Validate](img/Validate.png)

1. Select ![Error icon](img/ErrorIcon.png) on the right pane to confirm if the decision was validated successfully.

   ![Validated Successfully](img/ValidatedSuccess.png)

1. To test the decision, select the **Scoring** tab.
1. On the **Tests** sub-tab, click **New Test**.
1. Enter the following information:
   * Name: **Loan_Request_Review_Test**
   * Description: **Test 1**
   * Location: **/Public/SID Workshop/Tests**
      > &#9755; Select ![Folder icon](img/FolderIcon.png) to navigate to *SAS Content/SID Workshop/Tests*.
   * Input table: **LOAN_APPLICANTS**.

   ![Test Decision](img/TestDecision.png)

1. Click **Save** to save this test to the rule set.
1. Check the newly created test and click **Run** to run the specified test.
1. Once the test run is complete, select ![Result table](img/ResultTable.png) to view the test results.
1. Review the results on the *Output* tab.

   ![Decision test output](img/DecisionTestOutput.png)

1. Click **x** to close the test results and the rule set.

## Exercise Completed

**You have completed the exercise on getting started with SAS Intelligent Decisioning!**

**THANK YOU FOR ATTENDING THIS WORKSHOP!**
