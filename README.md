[![General Assembly Logo](https://camo.githubusercontent.com/1a91b05b8f4d44b5bbfb83abac2b0996d8e26c92/687474703a2f2f692e696d6775722e636f6d2f6b6538555354712e706e67)](https://generalassemb.ly/education/web-development-immersive)

# Simple Storage Service on Amazon Web Services setup

## Instructions

Read over all the instructions before proceeding.

Follow the steps outlined to create and gain programmatic access to an AWS S3
 bucket.

## Prerequisites

-   An `AWS` _(Amazon Web Services)_ account
-   A Credit card is required to verify your AWS account.

If you do not have an account, open [AWS](https://aws.amazon.com/) and click
 `Create a Free Account`.
Amazon provides a [free tier](http://aws.amazon.com/free/),
 with some limitations, for twelve months after you sign-up for an AWS account.

## Motivation

Storing large static files is a common need for a web application.
Accepting image uploads from authorized users but allowing public read access is
 a frequent example.

AWS provides a variety of APIs, one of which is easily used for this purpose.
This guide helps ensure access to these APIs is restricted.

Why is the important?

Using any metered API has financial risks.  Using many APIs may have data risks
 (information loss or exposure).

Using restrictive access control with AWS ensures that even if an identity is
 compromised, the actual risks, financial and otherwise, are limited.

## AWS S3 access control

1.  Open the [AWS Console](https://console.aws.amazon.com/console/) in your
 browser
2.  From the `AWS` console, select `Services`, and open tabs for
 `IAM` _(Identity and Access Management)_ and `S3` _(Simple Storage Service)_.

### Identity and Access Management (IAM)

Identities are how we grant access to AWS APIs.

In the [IAM](https://console.aws.amazon.com/iam) tab:

*Getting to the IAM tab:*

![iam tab](https://git.generalassemb.ly/storage/user/5688/files/96f5ca72-52c3-11e7-8d1f-03c42a2df2b4)

![iam search](https://git.generalassemb.ly/storage/user/5688/files/98790210-52c3-11e7-8fe3-3e56ff1253b2)

*Identity and Access Management (IAM)*
![iam dashboard](https://git.generalassemb.ly/storage/user/5688/files/9dc1e764-52c3-11e7-84b5-c743f27294e0)

3.  Select `Users` in the left sidebar.

![users tab](https://git.generalassemb.ly/storage/user/5688/files/9ee48afc-52c3-11e7-9f5c-fa9d9148317b)

4.  Click `Add User` near the top of the page.

5.  Enter `sei-upload` into the text box.

6.  Under access type, check `Programmatic Access`

![user name and checked box](https://media.git.generalassemb.ly/user/16103/files/dc68a000-b448-11e9-8592-ca009ade22b8)

7.  Click `Next: Permissions`

8.  Highlight Add User to Group

![highlight add user to group](https://media.git.generalassemb.ly/user/16103/files/ee4b4280-b44a-11e9-901d-2ebfb3479af5)

9.  Click `Next: Tags`

10.  We can skip adding tags for now. Click `Next: Review`

11.  Click `Create user`

![create user](https://media.git.generalassemb.ly/user/16103/files/0ae77a80-b44b-11e9-8cef-da45f1c5b9e2)

12.  Click `Close`

_Then_

13.  Click on your newly created user.
 - Make sure `sei-upload` is checked.
 - Click directly on `sei-upload`

![click on user](https://media.git.generalassemb.ly/user/16103/files/80534b00-b44b-11e9-85a0-373f1970d78e)

14.  Click on the security credentials tab.

15.  Click the small `x` to the right of your existing access key to delete it.

16.  Click `Create access key`

17.  When you recieve a `Success` response, click `Download .csv file` and save the CSV to your `sei` folder. (this is
the only time you'll be able to see your access key, but you can generate a new one anytime
and are encouraged to rotate them frequently)

![download csv](https://media.git.generalassemb.ly/user/16103/files/5fd7c080-b44c-11e9-8b1a-44874875b54e)

18.  Click `Close`
19.  Copy the `User ARN` _(Amazon Resource Name)_ at the top of the page and save it in [arn.txt](arn.txt).

![user arn](https://media.git.generalassemb.ly/user/16103/files/e7253400-b44c-11e9-8663-135b5eddbebd)

We'll need the User ARN to grant access to an S3 bucket we'll use for uploads.
We'll also need an `Access Key` _(Access Key Id and Secret Access Key)_ for this
 IAM User to upload files via the S3 API.
The Access Key is contained in the csv file that we just downloaded: `accessKeys.csv`.

**Note well:** `accessKeys.csv` contains `secrets`!
Do not share them or store them in git.
The [.gitignore](.gitignore) in this repository explicitly ignores this file. Altering the [.gitignore](.gitignore) file
in this repository could result in your AWS credentials (credentials linked to *your* credit card information) being visible on Github. *NEVER COMMIT SECRETS TO GIT*

### Simple Storage Service (S3)

S3 stores files you upload in `buckets`.  A bucket is a top-level namespace
 for your files.

In the [S3](https://console.aws.amazon.com/s3) tab:
![no buckets yet](https://media.git.generalassemb.ly/user/16103/files/7252f980-b44e-11e9-9c70-782854a03f89)

1.  Click `Create Bucket`.
 This opens the `Create a Bucket - Select a Bucket Name and Region` modal.

2.  Enter a name in the `Bucket name` box. It must be unique among all S3
 buckets and in all lowercase characters.

3.  Select a region close to you or choose `US East (N. Virginia)` for the `Region`.

4.  Click `Create` in the lower lefthand corner.

5.  Highlight your bucket and select the `Permissions` tab.

6.  Click `Block public access` near the bottom of the `Permissions` tab.

![click block public access](https://media.git.generalassemb.ly/user/16103/files/3d46a700-b44e-11e9-938d-f967247dd3b0)

7.  Click `edit` and uncheck all boxes.
8.  A popup will open and ask you to confirm this by typing 'confirm'.
9.  This should save automatically. If you experience an error, try again.

![block public access saved](https://media.git.generalassemb.ly/user/16103/files/074ae980-b3a0-11e9-8b97-5f19f25b2027)

10.  Click `Bucket Policy` near the bottom of the `Permissions` tab.

11.  At the bottom of the `Bucket policy editor` page,
 click `Policy generator`.  This opens the AWS Policy Generator page.

![policy page and generator button](https://media.git.generalassemb.ly/user/16103/files/ea6cef80-b44d-11e9-9668-7a82a0fe43c7)


#### On the AWS Policy Generator page

1.  For `Select Type of Policy` use `S3 Bucket Policy`.
2.  Under `Add Statement(s)`:
    - Select `Allow` for `Effect`.
    - Paste the User ARN that you saved in the `arn.txt` file into the `Principal` box.
    - Select `PutObject` and `PutObjectAcl` for `Actions`.

![add statements end result](https://git.generalassemb.ly/storage/user/5688/files/af19a6a0-52c3-11e7-944b-bda14c01b7ec)

3.  Enter `arn:aws:s3:::<bucket_name>/*` into the `Amazon Resource Name (ARN)` box.
    - Make sure to remove '<' & '>' and keep `/*` at the end of your user ARN for this step.

![paste ARN](https://git.generalassemb.ly/storage/user/5688/files/b02fbb2e-52c3-11e7-9e77-a95f6fceb508)

4.  Click `Add Statement`.

![click add statement](https://git.generalassemb.ly/storage/user/5688/files/b269d492-52c3-11e7-9a11-74afb54a90fc)

5.  Under `Generate Policy`:
6.  Click `Generate Policy`
7.  Copy the JSON from the `Policy JSON Document` modal.
8.  Click `Close`
9.  Return to the S3 tab.
10.  Paste the bucket policy into the `Bucket policy editor` field.

![paste bucket policy](https://media.git.generalassemb.ly/user/16103/files/e931b900-b3a0-11e9-9b84-901c9d7b52ea)

11.  Click `Save`.
12.  Click on `Access Control List`
13.  Click on your account
14.  A modal will pop up.
15.  Click `Save` in the modal.

You have now created and granted access to an S3 bucket.

These steps limit upload access to one bucket for the identity `sei-upload`.

This is one specific and restrictive way of implementing access control.
AWS provides many different mechanisms to grant and restrict access.

#### Example bucket policy JSON

```json
{
  "Version": "2012-10-17",
  "Id": "Policy1439826519004",
  "Statement": [
    {
      "Sid": "Stmt1439826516658",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::<AWS Account Id>:user/<IAM User Name>"
      },
      "Action": [
        "s3:PutObjectAcl",
        "s3:PutObject"
      ],
      "Resource": "arn:aws:s3:::<bucket_name>/*"
    }
  ]
}
```

**NEVER COMMIT SECRETS TO GIT: Your credentials.csv contains secrets! Do not share them or store them in git. If you accidently push them. to GitHub, they will be found in _minutes_ with potentially very expensive consequences.**

Have I scared you?  If not, read [this](https://medium.com/@nagguru/exposing-your-aws-access-keys-on-github-can-be-extremely-costly-a-personal-experience-960be7aad039) and note the part where the author writes:

> _Soon I received an email from Amazon to inform me that the account has been compromised and I should take action. Panicking, I went to the billing section to see there was a charge of over $3000 to date, and the projected cost was about $15000! I nearly had a heart attack._

## Checklist

-   [ ] Create (or select) an AWS Identity.
-   [ ] Create and download an access key for this identity.
-   [ ] Save said access key csv inside this repo.
-   [ ] Save your ARN to arn.txt in this repo.
-   [ ] Create an S3 bucket.
-   [ ] Set AWS Region to `US East (N. Virginia)`
-   [ ] Create a bucket policy.

## [License](LICENSE)

1.  All content is licensed under a CC­BY­NC­SA 4.0 license.
1.  All software code is licensed under GNU GPLv3. For commercial use or.
    alternative licensing, please contact legal@ga.co.
