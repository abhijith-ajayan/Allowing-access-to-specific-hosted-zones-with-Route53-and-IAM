# Allowing access to specific hosted zones with Route53 and IAM
## Description

This documentation explain the steps to give access to a specific Hosted Zone in Route53 service in AWS for a IAM User. Here, in this case i'm creating a subdomain delegation that uses Amazon Route53 as the DNS service and giving access to a IAM user for this specific subdomain hosted Zone.

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

## Steps Included:

1. Create a the Hosted Zone with the desired subdomain name from Route53 > Hosted Zones

>![alt text](https://i.ibb.co/xFxpdWj/Screenshot-from-2022-02-13-23-05-52.png)

if the main domain Hosted Zone is in the same account itself, make sure that you have entered the NS record details in the hosted Zone as below,

>![alt text](https://i.ibb.co/r2wzPVF/DNS-ZONE.png)

Also, you can create a aws route53 subdomain delegation that uses Amazon Route53 as the DNS service without migrating the parent domain from another DNS service, you can start by creating a hosted zone for the subdomain in Route53. Route53 stores information about your subdomain in the hosted zone and make sure that you have entered the NS details of subdomain in the main domain DNS aswell.



2. Create a New User under IAM service.
3. Create a Group and attach the user.
4. Create a Policy and add the below JSON details in the policy, to give permission to the specific user to specific Hosted Zone. Find the IDs of the hosted zone. Also , attach this policy to the above Group.

```sh
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "route53:GetHostedZone",
                "route53:ListHostedZones",
                "route53:ChangeResourceRecordSets",
                "route53:ListResourceRecordSets",
                "route53:GetHostedZoneCount",
                "route53:ListHostedZonesByName"
            ],
            "Resource": [
                "arn:aws:route53:::hostedzone/<hosted zone id 1>",
                "arn:aws:route53:::hostedzone/<hosted zone id 2>"
            ]
        }
    ]
}
```



5. Now try accessing the Hosted Zone using the newly created user. You need to access the hosted Zone with each Hosted Zone url, easiest way to do this is to go to the hosted zone in the console and look for the string at the end of the URL: https://console.aws.amazon.com/route53/home?#resource-record-sets:Z016140111MP5BH . Try accessing this url with the new user credentials.

Please note that the above policy let them do anything they want to in the specific hosted zone. So revoke the changes once everything is done.

## Sample Screenshot

>![alt text](https://i.ibb.co/yBF7yWh/screenshot1.png)
