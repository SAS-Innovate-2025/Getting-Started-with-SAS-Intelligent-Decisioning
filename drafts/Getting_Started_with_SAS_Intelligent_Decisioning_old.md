# Getting Started with SAS Intelligent Decisioning

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
1. Select the **loan_requests_full.csv** table and click ![Lightning Bolt icon](img/LightningBolt.png) to load the table into memory.

   ![SAS Data Explorer](img/DataExplorer.png)

1. Select the **In-memory data (available)** source to confirm the **LOAN_REQUESTS_FULL** table is now listed there.

   ![In-memory Data](img/InMemoryData.png)

1. Select the **LOAN_REQUESTS_FULL** table hyperlink to view its *Details*.
1. Select the **Sample data** tab to view a sample of the data set.
1. Click the **x** in the right-corner of the window to close the table.

   ![Sample data](img/SampleData.png)

## Create and Activate Global Variables

1. Select ![Viya Menu Selector](img/HamburgerMenu.png) **&#10132; Build Decisions** to open *SAS Intelligent Decisioning*.
1. Select ![Global Variables icon](img/GlobalVariablesIcon.png) to open the **Global Variables** page.
1. Click **New global variable** to create a new global variable.

   ![New global variable](img/NewGlobalVariable.png)

1. Enter the following information:
   * Name: **Rate_Low**
   * Description: **Low-level interest rate**
   * Data type: **Decimal**
   * Value: **5.50**

   ![Create Rate_Low global variable](img/CreateRate_LowVar.png)

1. Click **Save** to create the global variable.

   ![Rate_Low saved](img/Rate_LowSaved.png)

   > &#9998; You can select ![More options](img/MoreOptions.png) &#10132; **Hide** to hide a global variable.  You can then select ![More options](img/MoreOptions.png) &#10132; **Manage hidden global variables** to either *delete* or *restore* the hidden global variable.

1. Click **New global variable** to create a second new global variable.

   ![New global variable with existing global variables](img/NewGlobalVariable2.png)

1. Enter the following information:
   * Name: **Rate_High**
   * Description: **High-level interest rate**
   * Data type: **Decimal**
   * Value: **10.75**

   ![Create Rate_High global variable](img/CreateRate_HighVar.png)

1. Click **Save** to create the global variable.

   ![Rate_High saved](img/Rate_HighSaved.png)

   > &#9998; Global variables must be activated before they can be used in a rule or decision.

   > &#9888; Once a global variable is activated, the current version is locked and cannot be edited.

   > &#9998; By default, the current value of each global variable is included with the rule or decision code when published to CAS or MAS.  However, an administrator can change these settings.  For more information, refer to this <a href="https://go.documentation.sas.com/doc/en/edmcdc/default/edmug/p039r2uirgk3j2n15cu9rsmfsxk7.htm#n1n590t8htupion16jngyxi2syk6" target="_blank">help topic</a>.

1. Click the **Rate_Low** hyperlink to open the global variable's properties in a window.

   ![Rate_Low link](img/Rate_LowLink.png)

1. Select the **Versions** tab.
1. In *Notes*, type **Activated on *<today's date>***.
1. Click the record **Version 1.0** to highlight it.
1. Click **Activate** to activate the selected global variable for use within rules and decisions.

   ![Activate Rate_Low](img/ActivateRate_Low.png)

1. Select **Yes** to confirm that the global variable version will be locked and become non-editable and a new editable version is created.

   ![Rate_Low activated](img/Rate_LowActivated.png)

1. Click **x** to close the *Edit Global Variable* window.

1. Repeat the previous steps to activate the **Rate_High** global variable. When activating add in the *Notes* **Activated on *<today's date>***.

   ![Rate_High activated](img/Rate_HighActivated.png)

## Create and Test an Assignment Rule Set

1. In SAS Intelligent Decisioning, select ![Rule Sets Icon](img/RuleSetsIcon.png) to open the **Rule sets** page.
1. Click **New rule set** to create a new rule set.

     ![Create new rule set](img/NewRuleSet.png)


1. Enter the following information for the new rule set:
    * Name: **Initial_Loan_Review**
    * Type: **Assignment**
    * Description: **Initial Loan Review Assignment Rule.**
    * Location: **/Public/SID Workshop**

        > &#9755; Select the folder icon to navigate to this folder in *SAS Content*.

        ![Initial_Loan_Review settings](img/Initial_Loan_ReviewSettings.png)

1. Click **Save** to create the rule set.
1. On the **Variables** tab, ensure that the **Rule Set Variables** sub-tab is selected.
1. Select **Add variable &#10132; Data table** to add variables from an *in-memory* table.

   ![Add rule set variables](img/AddRuleSetVariables.png)

1. Select ![Folder icon](img/FolderIcon.png) in the **Data table** selection.
1. In the *Choose Data* dialog, select *Available* from the drop-down menu next to *Show*, then select **LOAN_REQUESTS_FULL** from the available data list and click **OK**.

   ![Select Loan_Requests_Full](img/SelectLoanRequestsFull.png)

1. Click ![Select All](img/SelectAll.png) to select all the columns from the table and add them to the selected items side.

   ![Select all variables](img/SelectAllVariables.png)

1. Click **Add** to add the selected items as variables for the rule set.

   ![Added variables](img/AddedVariables.png)

    > &#9998; The variables are automatically added as both *Input* and *Output* variables. You can uncheck those selections as appropriate for your rule set.

1. To add some output custom variables, select **Add variable &#10132; Custom variable**
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

| Name |  Data type | Input | Output | Length | Initial Value | Maximum row count | Description |
|  :---:  |  :---:  |  :---:  |  :---:  |  :---:  |  :---:  |  :---:  |  :---:  |
| JUSTIFICATION  | Character |       | checked | 50 | UNDER REVIEW |       | Justification for Loan Approval Status |
| OFFER_RATE | Decimal |       | checked  |       |       |       | Interest Rate to Offer with Loan Approval |

   ![Additional custom variables](img/AdditionalCustomVariables.png)

1. Click **OK** to add the custom variables to the rule set.
1. Click ![Save button](img/SaveButton.png) to save the rule set.

    ![Updated variables](img/UpdatedVariables.png)

1. Select the **Global Variables** sub-tab.
1. Click **Select Variables** to add global variables to the rule set.
1. Check the **Rate_Low** global variable and click **OK**.
1. Click ![Save button](img/SaveButton.png) to save the rule set.

     ![Add global variables](img/AddGlobalVariables.png)

1. Select the **Rule Set** tab.
1. Click **Add other**.
1. Select **Assignment** for the *rule type* and click **OK**.

   ![Add assignment](img/AddAssignment.png)

1. For the action assignment, select **ASSIGN STATUS 'REVIEW'**.
1. Click **+ Add Rule** to add a rule block.

   ![Initial loan review - assign status](img/InitialLoanReview1.png)

1. Name the rule **Automatic Loan Denial or Approval**.

   > &#9755; Select ![More options](img/MoreOptions.png) **&#10132; edit rule information**.

1. Uncheck **Record rule-fired data**.

     > &#9998; Record rule-fired data and its analysis will be performed in a later exercise.

1. Select the following for the rule:<br>
   **IF BAD = 1**<br>
   **THEN ASSIGN STATUS 'DENIED'** <br>
    **&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ASSIGN JUSTIFICATION 'AUTOMATIC DENIAL CRITERIA MET'**

    > &#9755; Hover over the first *ASSIGN* statement to surface statement options, then click ![Add/Plus](img/AddPlus.png) to add the second *ASSIGN* statement.

     ![Initial loan review - automatic denial](img/InitialLoanReview2.png)

1. Click ![Save button](img/SaveButton.png) to save the rule set.
1. Click **+ Add Rule** to add a new rule block.
1. Use the drop-down menu to change the **IF** to an **OR** to add an *OR* condition to the existing rule block.
1. Select the following for the *OR* condition:<br>
   **OR DEBTINC > 50.0** <br>
   **AND DEROG >= 2**

     > &#9755; Hover over the *OR* statement to surface statement options, then click ![Add/Plus](img/AddPlus.png) to add the *AND* statement.

     ![Initial loan review - or statement](img/InitialLoanReview3.png)

1. Click ![Save button](img/SaveButton.png) to save the rule set.
1. Click **+ Add Rule** to add a new rule block.
1. Change the **IF** to an **ELSE** to add an *ELSE IF* condition to the existing rule block.
1. Select the following fo the *ELSE* condition: <br>
   **ELSE BAD = 0** <br>
   **AND LOAN <= 15000** <br>
   **AND DEBTINC < 10.0** <br>
   **THEN ASSIGN STATUS 'APPROVED'** <br>
    **&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ASSIGN JUSTIFICATION 'AUTOMATIC APPROVAL CRITERIA MET'**

   > &#9755; Click ![Add/Plus](img/AddPlus.png) to add the *AND* statements and additional *ASSIGN* statements.

   ![Initial loan review - automatic approval](img/InitialLoanReview4.png)

1. Click ![Add/Plus](img/AddPlus.png) to add an additional statement to the assignment block.
1. Select ![Pencil icon](img/PencilIcon.png) to edit the added statement in the complex rule editor.
1. In the *Expression Editor*, click **Clear** to clear the current expression.

   ![Clear Syntax](img/ClearSyntax.png)

1. In the *Variables* tab, double-click **OFFER_RATE** to add it to the expression.
1. In the *Operators* section, click **=** to add it the expression.
1. In the *Variables* tab, double-click **Rate_Low** to add it to the expression.
1. In the *Operators* section, click **\*** to add it the expression.
1. In the expression section, type **0.5**.
1. Click **Verify syntax** to validate the expression logic.

   ![Expression editor](img/ExpressionEditor.png)

1. Click **Save** to save the expression code and close the *Expression Editor*.
1. Click ![Save button](img/SaveButton.png) to save the rule set.

   ![Initial loan review - add offer rate](img/InitialLoanReview5.png)

1. To test the rule set, select the **Scoring** tab.
1. On the **Tests** sub-tab, click **New Test**.
1. Enter the following information:
   * Name: **Initial_Loan_Review_Test**
   * Description: **Test 1**
   * Location: **/Public/SID Workshop/Tests**
      > &#9755; Select ![Folder icon](img/FolderIcon.png) to navigate to *SID Workshop* in *SAS Content*, then select ![New](img/New.png) **&#10132; Folder** to create a new folder names Tests.
   * Input table: **LOAN_REQUESTS_FULL**.

   ![Test rule set](img/TestRuleSet.png)

1. Click **Variables** to view the variables mapping between the input table and the rule set variables.

     > &#9998; All the variables are automatically mapped since the variable names on the rule set match the names on the input table.

1. Click **OK** to close the *Variable Mappings* dialog.
1. Click **Save** to save this test to the rule set.

     > &#9998; If do not select **Save** and simply select **Run**, then the test is run; however, the test is not saved to the rule set and if your session is closed you will need to setup the test again to run it in the future.

1. Check the newly created test and click **Run** to run the specified test.

     ![Select test](img/SelectTest.png)

     > &#9998; It may take a few seconds for the test to complete.  You can select ![Refresh](img/Refresh.png) to refresh the test status.

1. Once the test run is complete, select ![Result table](img/ResultTable.png) to view the test results.
1. Review the output table and its results.

   ![Test output](img/TestOutput.png)

## Add code file

1. code files
1. new code file
1. name Additional_Review, Type DS2 Code File, description "Additional review for requests that have not been approved or denied.", location /Public/SID Workshop
1. copy/paste code:

package "${PACKAGE_NAME}" /inline;
   method execute(in_out double CREDIT_SCORE,
                  in_out double INCOME,
                  in_out double LOAN,
                  in_out varchar REASON,
                  in_out varchar JUSTIFICATION,
                  in_out varchar STATUS);

        if REASON = 'WE' AND LOAN > 2500 then
            do;
                STATUS = 'DENIED';
                JUSTIFICATION = 'EXCEEDED WEDDING LOAN AMOUNT';
            end;
        else if REASON = 'ME' AND LOAN > 50000 then
            do;
                STATUS = 'DENIED';
                JUSTIFICATION = 'EXCEEDED MEDICAL LOAN AMOUNT';
            end;

        if CREDIT_SCORE > 650 AND LOAN < INCOME then
            do;
                STATUS = 'APPROVED';
                JUSTIFICATION = 'APPROVED BASED ON CREDIT SCORE';
                OFFER_RATE = %tslit(Rate_Low);
            end;
        else if (CREDIT_SCORE >= 525 AND CREDIT_SCORE <= 650) AND LOAN  < 0.7 * INCOME then
            do;
                STATUS = 'APPROVED';
                JUSTIFICATION = 'APPROVED BASED ON CREDIT SCORE';
                OFFER_RATE = %tslit(Rate_High);
            end;
        else
            do;
                STATUS = 'DENIED';
                JUSTIFICATION = 'DENIED BASED ON CREDIT SCORE AND/OR LOAN AMOUNT';
            end;

    end;

endpackage;

1. Save
    * if you get warning ab sync variables, select code then save
1. Sync variables
1. More options -> validate (code should compile correctly)
1. Scoring -> tests -> new test. save under workshop/tests
1. variables -> assign justification use value under review and status review
1. save
1. select -> run
1. review results. sort in various ways to review.

## Create and test Decision

1. Select ![Decision icon](img/DecisionIcon.png) to open the **Decisions** page.
1. Click **New decision** to create a new decision.
1. Enter the following information for the new rule set:
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

     > &#9998; There is an error since the variables from the Rule set are not part of the Decision.

1. Select ![More options](img/MoreOptions.png) **&#10132; Add missing variables**.

     ![Add missing variables](img/AddMissingVars.png)

1. Click **Add** to add all the missing variables to the decision.
1. Click ![Save button](img/SaveButton.png) to save the decision.
1. Select the **Variables** tab.

     ![Decision variables](img/DecisionVars.png)

1. Select the **Global Variables** sub-tab.
1. Click **Select Variables**.

     ![Select global vars](img/SelectGlobalVars.png)

1. Select both **Rate_High** and **Rate_Low**.

     ![Check variables](img/CheckVars.png)

1. Click **OK** to add these global variables to the decision.

     ![Added global variables](img/AddedGlobalVars.png)

1. Select the **Decision Flow** tab.
1. On the *Initial_Loan_Review* node, select ![More options](img/MoreOptions.png) **&#10132; Add &#10132; Branch**.

     ![Add branch](img/AddBranch.png)

1. Enter the name **Loan Status** and select **Equals** for the branch type.

     ![New branch](img/NewBranch.png)

1. Click **OK** to create the branch.
1. In the *Properties* section, select the **STATUS** variable for the *Branch expression*.

     > &#9755; Select **More...** to view the full listing of variables in the *Branch expression* drop-down and click **OK**.

1. Click ![Add/Plus](img/AddPlus.png) to add the following *paths*:
   * **"REVIEW"**
   * **"APPROVED"**
   * **"DENIED"**.

    > &#9998; The *Other* path is automatically added for this branch type.

1. Click ![Save button](img/SaveButton.png) to save the decision.

     ![Branches](img/Branches.png)

1. highlight review path -> add -> ds2 code file (from sid_workshop)
1. save
1. validate, check for errors
1. scoring
1. new test, save
1. run
1. review results

<br>


## Import Rule Set and Variables

1. In SAS Intelligent Decisioning, select ![Rule Sets Icon](img/RuleSetsIcon.png) to open the **Rule sets** page.
1. Create a new rule set with the following information:
    * Name: **Credit_Score_Check**
    * Type: **Assignment**
    * Description: **Secondary loan review based on credit score.**
    * Location: **/Public/SID Workshop**

        > &#9755; Information must be entered **exactly as specified above** or the rule set import will fail.

        > &#9755; Select the folder icon to navigate to this folder in *SAS Content*.

        ![Credit_Score_Check settings](img/Credit_Score_CheckSettings.png)

1. Click **Save** to create the rule set.

1. On the **Variables** tab for the *Credit_Score_Check* rule set, select the **Rule Set Variables** sub-tab.
1. Select **Import &#10132; Comma-delimited (*.csv)** to open the *Import Variables from CSV File* dialog.

     ![Import CSV Variables](img/ImportCSVVar.png)

1. Select ![Folder icon](img/FolderIcon.png) to navigate the CSV file for the variables.
1. Navigate to **C:\Getting-started-with-SAS-Intelligent-Decisioning\files** and select **variables.csv**.
1. Click **Open**.

     ![Import vars](img/ImportVars.png)

1. Click **Import** to import the variables into the rule set.

     ![After import](img/AfterImport.png)

1. Select the **Global Variables** sub-tab.
1. Click **Select Variables**.
1. Select both the **Rate_High** and **Rate_Low** global variables.

     ![Check global vars](img/CheckVars.png)

1. Click **OK** to add the selected global variables to the rule set.

     ![Added global vars, 2nd decision](img/AddedGlobalVars2.png)

1. Click ![Save button](img/SaveButton.png) to save the changes to the rule set.
1. Select the **Rule Set** tab.
1. Click **Import** to import the rule set logic from a CSV.

     ![Import rule set](img/ImportRuleSet.png)

1. Navigate to **C:\Getting-started-with-SAS-Intelligent-Decisioning\files** and select **Credit_Score_Check_v1_0.csv**.
1. Click **Open**.

     ![Import rule set 2](img/ImportRuleSet2.png)

1. Click **Import** to import the rule set from CSV.

     ![Import rule set 3](img/ImportRuleSet3.png)

1. Review the rule set logic.
     > If the customer has a high credit score (greater than 650) and the loan amount is less than their income, the loan will be approved and given the low offer rate.
     > If the customer's credit score is between 525 and 650 (inclusive) and the loan amount is less than 70% of their income, the loan will be approved and given the high offer rate.
     > Otherwise, the loan will be denied.

1. Click **x** to close the rule set.

> &#9998; You will test this rule set in a later exercise.

## Update and Test Decision