# AWS Step Functions
AWS Step Functions is a serverless orchestration service that lets you integrate with AWS Lambda functions and other AWS services to build business-critical applications. Through Step Functions' graphical console, you see your application’s workflow as a series of event-driven steps.

Step Functions is based on state machines and tasks. In Step Functions, a workflow is called a state machine, which is a series of event-driven steps. Each step in a workflow is called a state. A Task state represents a unit of work that another AWS service, such as AWS Lambda, performs. A Task state can call any AWS service or API.

## To create the state machine prototype
1. Open the Step Functions console and choose Create state machine.
2. In the Choose a template dialog box, select Blank.
3. Choose Select. This opens Workflow Studio in Design mode.
4. In Workflow Studio, from the Actions tab, drag an AWS Lambda Invoke API action and drop it to the empty state labelled Drag first state here. Configure it as follows:
   * In the Configuration tab, for State name, enter Get credit limit.
5. From the Flow tab, drag and drop a Choice state below the Get credit limit state. Rename the Choice state to Credit applied >= 5000?.
6. Drag and drop the following states as branches of the Credit applied >= 5000? state.
   * Amazon SNS Publish – From the Actions tab, drag and drop the Amazon SNS Publish API action. Rename this state to Wait for human approval.
   * Pass state — From the Flow tab, drag and drop the Pass state. Rename this branch to Auto-approve limit.
7. Drag and drop a Parallel state after the Choice state as follows:
   * Drop the Parallel state after the Credit limit approved state.
   * Rename the Parallel state to Verify applicant's identity and address.
   * Under both the branches of the Parallel state, drag and drop two AWS Lambda Invoke API actions.
   * Rename these states as Verify identity and Verify address respectively.
   * Choose the Auto-approve limit state and for Next state, select Verify applicant's identity and address.
8. Drag a DynamoDB Scan state and drop it below the Verify applicant's identity and address state. Rename the DynamoDB Scan state to Get list of credit bureaus.
9. Drag and drop a Map state after the Get list of credit bureaus state. Configure the Map state as follows:
   * Rename it to Get scores from all credit bureaus.
   * For Processing mode, keep the default selection of Inline.
   * Drag and drop an AWS Lambda Invoke API action to the empty state labelled Drop state here.
   * Rename the AWS Lambda Invoke state to Get all scores.
10. Keep this window open and proceed to the next tutorial for further actions.

In this tutorial, you learn how to define the first service integration for your workflow. You use the Task state named Get credit limit to invoke a Lambda function. Within Task states, you can use the AWS SDK integrations that Step Functions supports.
## Create and test the Lambda function
1. Ceate a Lambda function titled 'RandomNumberforCredit' either in python or node.
2. Choose Deploy and then choose Test to deploy the changes and see the output of the Lambda function.

## Update the workflow – configure the Get credit limit state
1. Open the Step Functions console window containing the workflow prototype you created in Tutorial 1.
2. Choose the Get credit limit state, and in the Configuration tab, do the following:
   * For Integration type, keep the default selection of Optimized.
   * For Function name, choose the RandomNumberforCredit Lambda function from the dropdown list.
   * Keep the default selections for rest of the items.
3. Keep this window open and proceed to the next tutorial for further actions.

## Implement an if-else condition in your workflow
You can implement if-else conditions in your workflows by using the Choice state. It determines the workflow execution path based on whether a specified condition evaluates to true or false.

1. Open the Step Functions console window containing the workflow prototype you created in Tutorial 1: Create the prototype for your state machine.
2. Choose the Credit applied >= 5000? state and in the Configuration tab, specify the conditional logic as follows:
   * Under Choice Rules, choose the Edit icon in the Rule #1 tile to define the first choice rule.
   * Choose Add conditions.
   * In the Conditions for rule #1 dialog box, for Variable, enter $.
   * For Operator, choose is less than.
   * For Value, choose Number constant, and then enter 5000 in the field next to the Value dropdown list.
   * Choose Save conditions.
   * For the Then next state is: dropdown list, choose Auto-approve limit.
   * Choose Add new choice rule, and then define the second choice rule when the credit amount is greater than or equal to 5000 by repeating substeps 2.b through 2.f. For Operator, choose is greater than or equal to.
   * For the Then next state is: dropdown list, choose Wait for human approval.
   * In the Default rule box, choose the Edit icon to define the default choice rule, and then choose Wait for human approval from the Default state dropdown list. You define the Default rule to specify the next state to transition to if none of the Choice state conditions evaluate to true or false.
3. Configure the Wait for human approval state as follows:
   * In the Configuration tab, for Topic, start typing the name of the Amazon SNS topic, TaskTokenTopic, and choose the name as it appears in the dropdown list.
   * For Message, choose Enter message from the dropdown list. In the Message field, you specify the message you want to publish to the Amazon SNS topic. For this tutorial, you publish a task token as the message.
   * Choose Done in the dialog box that appears.
4. Keep this window open and proceed to the next tutorial for further actions.

So far you’ve learned how to run workflows in a sequential manner. However, you can run two or more steps in parallel using the Parallel state. A Parallel state causes the interpreter to execute each branch concurrently.
## Define multiple tasks to perform in parallel
1. Select the Parallel state and submit the tasks to it.
2. Open the Step Functions console window containing the workflow prototype you created in Tutorial 1: Create the prototype for your state machine.
3. Choose the Verify identity state, and in the Configuration tab, do the following:
   * For Integration type, keep the default selection of Optimized.
   * For Function name, choose the check-identity Lambda function from the dropdown list.
   * For Payload, choose Enter payload and then replace the example payload if you wish.
4. Follow the same steps for next task.



















