# AWS
Some useful and frequently used commands for `AWS` CLI

### AWS S3 Bucket
```bash
# configure (access id, key, none, none)
aws configure

# list files 
aws s3 ls <s3 url>

# copy files
aws s3 cp <SRC> <DEST>
aws s3 cp <SRC> s3://.....

# copy folders
aws s3 sync <SRC> <DEST>
aws s3 sync <FOLDER> s3://....