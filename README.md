# Connect_to_AWS_S3_by_Python
#Bulk Download from AWS S3 Folders to your local folder
## Make the connection to S3
import os
import boto3
from botocore.exceptions import NoCredentialsError
def download_files_from_s3(aws_access_key_id, aws_secret_access_key, bucket_name, file_keys, local_dir):
    session = boto3.Session(
        aws_access_key_id = aws_access_key_id,
        aws_secret_access_key = aws_secret_access_key)

    s3 = session.client('s3')

    for file_key in file_keys:
        try:
            file_obj = s3.get_object(Bucket=bucket_name, Key=file_key)
            file_content = file_obj['Body'].read().decode('utf-8')


            local_filename = file_key.replace("/", ".")
            local_path = os.path.join(local_dir, local_filename)

 
            with open(local_path, 'w', encoding="utf-8") as f:
                f.write(file_content)

 
            print(f'Successfully downloaded {file_key} to {local_path}')

 
        except NoCredentialsError:
            print('No AWS credentials were provided.')

## Then if the connection is successful, run the below query and replace it with your local location and other variables. It downloads all files from your specified folder at S3 into your computer.

download_files_from_s3(
    aws_access_key_id="xxxx",
    aws_secret_access_key="xxx",
    bucket_name="s3-xx-xxx",
    file_keys=[ 'Pronto/sales-order.csv'],
    local_dir=r'C:\Users\xxx\OneDrive -xxx\xx'
)
