I chose 2UserManagement alongside the service Amazon Cognito with the CloudFormation infrastructure tool.

I used AWS, GitHub and Google to look for templates that could help me with structuring the template

Following the workshop I created a user pool allowing users to sign in and out via Amazon Cognito, I
believe multi-factor authentication is important as it not only protects the user but also the website.
'''
}
  "AWSTemplateFormatVersion": "2010-09-09",
  "Description": "Custom Resource Types for managing SWF resources",
  "Resources": {
     "CognitoUserPool": {
       "Type": "Custom::CognitoUserPool",
       "Properties": {
         "ServiceToken": {
           "Fn::GetAtt": [
             "CognitoUserPoolFunction",
             "Arn"
            ]
         }
         "Region": {
           "Ref": "AWS::Region"
         },
         "PoolName": "WildRydes",
         "AliasAttributes": [
           "phone_number",
           "email",
           "preferred_username"
         ],
         "AutoVerifiedAttributes": [
           "phone_number",
           "email"
         ],
         "EmailVerificationMessage": "EmailVerificationMessage",
         "EmailVerificationSubject": "EmailVerificationSubject",
         "LambdaConfig": {
           "CustomMessage": "CustomMessage",
           "PostAuthentication": "PostAuthentication",
           "PostConfirmation": "PostConfirmation",
           "PreAuthentication": "PreAuthentication",
           "PreSignUp": "PreSignUp"
         }
         "MfaConfiguration": "ON",
         "Policies": {
           "PasswordPolicy": {
             "MinimumLength": 1,
             "RequireLowercase": true,
             "RequireNumbers": true,
             "RequireSymbols": false,
             "RequireUppercase": true,
             "TemporaryPasswordValidityDays": 3
            }
         },
         "SmsAuthenticationMessage": "SmsAuthenticationMessage",
         "SmsVerificationMessage": "SmsVerificationMessage"
       },
       "LambdaExecutionRole": {
          "Type": "AWS::IAM::Role",
          "Properties": {
            "AssumeRolePolicyDocument": {
              "Version": "2012-10-17",
              "Statement": [
                {
                    "Effect: "Allow",
                   "Principal": {
                     "Service": [
                       "lambda.amazon.com"
                     ]
                   },
                   "Action": [
                      "sts:AssumeRole"
                   ]
                 }
               ]
             },
             "Path": "/",
             "Policies": [
               {
                 "PolicyName": "root",
                 "PolicyDocument": {
                   "Version": "2012-10-17",
                   "Statement": [
                     {
                        "Effect": "Allow",
                        "Action": [
                          "logs:CreateLogGroup",
                          "logs::CreateLogStream",
                          "logs:PutLogEvents"
                       ],
                       "Resource": "arn:aws:logs:*:*:*"
                     },
                     {
                       "Effect": "Allow",
                       "Action": [
                        "*"
 
                      ],
                      "Resource": "*"
                  }
                ]
              }
            }
          ]
        }   
      }
    },
    "Outputs": {} 

 
}

'''

I did find it challenging trying to stick to just Amazon Cognito as while researching I realised howmuch Lambda and IAM can be heavily involved. As well as figuring out the placement of the syntax andand which policies were most important when dealing with user management and settled on the passwordpolicy. But I tried my best with what I understood and asked for help where I did not understand.  
