# Codecommit deploy CFT via codepipline.

Assumptions
 - You have git and extras for codecommit installed
 - You are on a OSX or Linux machine
 - Your AWS IAM user has programmatic access configured

1. Copy the yaml file from github.
2. Create a codecommit repository and commit the yaml file to the repo.
3. Go to AWS IAM in console and for your user and Click "Attach existing policies" and add AWSCodeCommitFullAccess.
4. In a terminal ``` ssh -y -t rsa -f ~/.ssh/codecommit_rsa ``` . This generates the ssh key pair
5. Go to AWS IAM console and for your user, in the third tab "Security Credentials" clikc upload your SSH public key and paste the contents of your public key.
6. On your local machine edit ~/.ssh/config  and create a Host entry for git-codecommit.*.amazonaws.com, User <your USER-KEY-ID> and IndentityFile ~/.ssh/codecommit_rsa
7. Now commit the Cloudformation yaml file to your codecommit repository.
8. Navigate to AWS CLoudFormation in the console and select create new role, click on Permissions and add the policy PowerUserAccess. Ignore permission boudaries for this exercise.
9. Review the role name I used CustomClodFormationPowerUser (easy cleanup for deletion).
10. Navigate to AWS CodePipeline and create an new pipeline.
11. Name the pipleline CloudFormationPipline
12. Chose CodeCommir as the source, select the repository and branch Master.  Leave detection method as Amazon.
13. For Build, select no build ( this is just yaml ).
14. For Deploy, chose AWS CloudFormation as the provider; Action mode: Create or update a stack; Stack name: bob-lamp-test; Template fie: lamp-test-bob.yml; Role Name: The custom role we created above.
15. Review the information and click "Create pipeline".
16. You should now see the deployment process start.
17. Go back to AWS CloudFormation and click on bob-lamp-test and in the outputs there will be the public IP of the server
18. Point your browser to http://<public IP>/index.php
19. All done.
