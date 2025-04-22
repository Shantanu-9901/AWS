# AWS IAM (Identity and Access Management)       <img src="https://github.com/user-attachments/assets/9cb1841f-47a7-4511-b47f-d4ef35eff624" alt="IAM Logo" width="40" />
## Objective: To securely manage access to AWS services and resources. You'll create users, groups, roles and define access policies. 


## Create IAM Users
1. Go to IAM Dashboard:

      - Log in to the AWS Management Console.
      - Navigate to IAM by searching for it in the "Find Services" bar.

2. Create a New User:

    - In the IAM dashboard, go to the Users section.
    - Click on Add user at the top of the page.

3. Configure User:

    - Enter a User name (e.g., testUser).
    - Select Access type (you can choose both Programmatic access for AWS CLI/API access and AWS Management Console access for console access).
    - Set a Console password (choose a password or auto-generate one).
    - Click Next: Permissions.

4. Set Permissions:

    - Add user to group: Select a predefined group (e.g., AdminAccess) or create a new group and assign permissions.
    - Attach policies directly: Alternatively, select from existing policies such as AdministratorAccess, ReadOnlyAccess, etc.
    - Next: Tags (optional) – Tags can be used to organize and manage resources, but it’s optional.
    - Next: Review – Review your settings.

5. Create User:

    - Click Create user.
    - Note the User credentials (Access key ID, Secret access key, and Console URL).

6. Download Credentials:

    - Download the .csv file with credentials or copy the credentials and save them securely.

## Create IAM Groups
1. Go to IAM Groups:

    - In IAM Dashboard, go to Groups and click Create New Group.

2. Configure Group:

    - Enter a Group name (e.g., AdminGroup).
    - Select existing policies to assign to the group (e.g., AdministratorAccess).

3. Add Users to Group:

    - After creating the group, you can add existing users to it by selecting the group and clicking on Add users.
    - Select the users to add to the group and click Add users.

## Create IAM Roles
1. Go to IAM Roles:

    - In IAM Dashboard, go to Roles and click Create role.

2. Select Trusted Entity:

    - Choose the trusted entity. For example, if you’re creating a role for EC2, select AWS service > EC2.

3. Attach Permissions:

    - Attach the required policies to the role (e.g., AmazonEC2FullAccess).

4. Review and Create:

    - Provide a Role name (e.g., EC2AdminRole).
    - Review the role and click Create role.

## Attach IAM Policies
1. Go to IAM Policies:

    - In IAM Dashboard, go to Policies and click Create Policy.

2. Choose Permissions:

    - Use the visual editor to choose permissions for your policy or write it manually in JSON format.

3. Review and Attach Policy:

    - Give the policy a name and description.
    - Attach the policy to users, groups, or roles.

## Test IAM Permissions
1. IAM Policy Simulator:

    - Use the IAM Policy Simulator (found under IAM > Policy Simulator) to test the permissions of users/roles.
    - Choose the user/role, select actions (e.g., s3:ListBucket), and simulate the policy's effect.
