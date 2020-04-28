1.  You are providing AWS consulting services to an IT company. This company owns dozens of AWS accounts and prefers to set up an AWS Organization so that all of these accounts can be managed in together under a root account. The AWS administrator planned to create invitations for other accounts and asked for your advice. About inviting other accounts to join an AWS Organization, which statements are correct? (Select TWO.)
    - Good
        - One AWS account can join only one Organization even if it receives multiple invitations
        - If an invitation is not accepted or rejected for over 15 days, the invitation will expire.
    - Bad
        - Organization invitations can only be created through AWS Organization console.
        - Only the root user of an AWS account can create invitations.
        - User can create unlimited invitations per day per organization.

2.  As an AWS Solutions Architect, you are in charge of the configuration of a new AWS Organization among several AWS accounts. You already created an Organization and sent invitations for other accounts to join. Most AWS accounts can join the Organization successfully. However for one AWS account, it did not receive the invitation email so that it did not know how to join. How should you fix the problem?
    - In the root AWS account, cancel the invitation and then create a new invitation to this AWS account.

3. You are an AWS Solutions Architect in a financial company. The company recently started working on migrating legacy applications to AWS. You planned to use a new AWS Organization to manage all AWS accounts so that you can easily configure accounts, assign organizational units, configure security policies, etc. Which methods are valid for you to add accounts to the Organization? (Select TWO.)
    - Answers: 
        - In AWS Organization console, create accounts within your organization.
        - In the root account of the Organization, create invitations to other accounts and wait for them to accept the invitations.
    - Note:  There are basically two methods to add accounts to AWS Organization either through creating new accounts within an Organization or creating invitations. 

4. You have signed in an AWS Organization's master account using an admin IAM user. You need to move accounts in this Organization from one OU (Organizational Unit) to another, or back to the root from an OU. However the operation was disallowed due to lack of permissions. So you started looking at the IAM policies attached to this user. What are the minimum permissions you need to move account among OUs? (Select TWO.)
    - organizations:DescribeOrganization
    - organizations:MoveAccount

