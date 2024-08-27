# Linux Users - AWS SSO Cli Login Setup

AWS SSO is a service that allows users to centrally manage access to multiple AWS accounts and applications with a single set of credentials.

The AWS CLI command: `aws sso login` is used to initiate the AWS Single Sign-On (SSO) login process for a specific AWS CLI profile. 

`aws-sso-cli-setup.txt`

```shell

## working path is user's .aws path
linuxuser@LINUX .aws % pwd
/Users/linuxuser/.aws

## display contents of ~/.aws
linuxuser@LINUX .aws % ls  
config sso

## sample content under the config file
linuxuser@LINUX .aws % cat config
[default]
region = us-west-1
output = json

[sso-session console-xyz-com]
sso_start_url = https://d-01234567890.awsapps.com/start
sso_region = us-west-1
sso_registration_scopes = sso:account:access

[profile PROF-FOOAdmin]
sso_session = console-xyz-com
sso_account_id = 909080807070
sso_role_name = FOOAdmin
region = us-west-1
output = json

## create a local .functions file and create a local awslogin function
cat .functions

function awslogin {
    unset AWS_ACCESS_KEY_ID
    unset AWS_SECRET_ACCESS_KEY
    unset AWS_SESSION_TOKEN
    unset AWS_SECURITY_TOKEN

    export PS_SYSTEM=${PS1}

    aws sso login --profile PROF-FOOAdmin 
    #aws sso login --profile PROF-FOOAdmin --no-browser

    eval "$(aws configure export-credentials --profile PROF-FOOAdmin --format env)"

    export PS1='\[\e[1;38;5;208m\]\u@\h:\w PROF-FOOAdmin$\[\e[m\] '

    aws sts get-caller-identity
}

## call your local function by .bashrc
## append to .bashrc
if [ -f ~/.functions ]; then
    . ~/.functions
fi


## reload your updated .bashrc file
linuxuser@LINUX % source .bashrc

## call your awslogin function
linuxuser@LINUX ~ % awslogin
Browser will not be automatically opened.
Please visit the following URL:

https://device.sso.us-west-1.amazonaws.com/

Then enter the code:

ABCD-EFGH

Alternatively, you may visit the following URL which will autofill the code upon loading:
https://device.sso.us-west-1.amazonaws.com/?user_code=ABCD-EFGH
Successfully logged into Start URL: https://d-01234567890.awsapps.com/start

linuxuser:IN-AWS-SESSION:~$aws sts get-caller-identity
{
    "UserId": "AAAA3ZZZ3DDDDDDEEEEEE:foobar-admin@xyz.com",
    "Account": "909080807070",
    "Arn": "arn:aws:sts::909080807070:assumed-role/AWSReservedSSO_FOOAdmin_abc600b0000a4888/foobar-admin@xyz.com"
}

```


Here's a breakdown of the command:

- `aws sso login`: This part of the command instructs the AWS CLI to initiate the AWS SSO login process. It means you want to log in to your AWS environment using AWS SSO.

- `--profile PROF-FOOAdmin`: This is an option used to specify the AWS CLI profile you want to use for the SSO login. In AWS CLI configuration, profiles are used to store credentials and other configuration settings. In this case, you are using the profile named "PROF-FOOAdmin" to initiate the SSO login. This profile would typically contain information about the AWS SSO user or role you want to assume when logging in.

Once you run this command, AWS SSO will prompt you to enter your credentials, and upon successful authentication, it will provide you with temporary credentials or access tokens for the AWS account or role associated with the "PROF-FOOAdmin" profile. These temporary credentials will then be used by the AWS CLI for any subsequent AWS CLI commands you execute with this profile, allowing you to interact with AWS resources using the permissions associated with the specified AWS SSO user or role.


