# Boto 3 Setup

## Install boto3
The official Amazon AWS SDK (Software Development Kit) for Python is a library called Boto 3. Install it in your project environment:

```bash
$ pipenv install boto3
```

## Configure Credentials
Do not ever put your Amazon AWS, or any other secret keys, in your source code!

If you do, and push your code to GitHub, it will be discovered within minutes and could result in a major financial liability!

During development (not in a deployed app), boto3 will automatically look in a special file for your AWS keys.

First let's create the folder it needs:

```bash
$ mkdir ~/.aws
```

Now let's create the file:

```bash
$ touch ~/.aws/credentials
```

Note that there is no file extension on the credentials file.

Now  open it and put your keys in there:

```bash
$ code ~/.aws/credentials
```

Then type the following in the file, substituting your real keys:

```env
[default]
aws_access_key_id=YOUR_ACCESS_KEY
aws_secret_access_key=YOUR_SECRET_KEY
```

> If you use AWS S3 in your project, you will need to set these same keys on Heroku using heroku `config:set`, however, the key names will need to be CAPITALIZED when setting them on Heroku.
