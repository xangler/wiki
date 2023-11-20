# aws 操作指南

```
set -e

export AWS_ACCESS_KEY_ID='demoID' 
export AWS_SECRET_ACCESS_KEY='demoKey'
S3_OUTPOINT="https://demo.aoss.com"

S3_BUCKET="s3://demo"
S3_FILE="demo.csv.gz"

# aws s3 --endpoint-url ${S3_OUTPOINT} ls ${S3_BUCKET}
aws s3 --endpoint-url ${S3_OUTPOINT} cp ${S3_FILE} ${S3_BUCKET}/${S3_FILE} 
# aws s3 --endpoint-url ${S3_OUTPOINT} presign ${S3_BUCKET}/${S3_FILE}
```