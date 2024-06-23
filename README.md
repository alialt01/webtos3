# webtos3
This repository contains a htmtl code which uploads content to aws s3. Note: need to update s3bucket policy and Cors code in aws.

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Upload to S3</title>
    <script src="https://sdk.amazonaws.com/js/aws-sdk-2.823.0.min.js"></script>
</head>
<body>
    <h1>Upload File to S3</h1>
    <input type="file" id="fileInput">
    <button onclick="uploadFile()">Upload</button>

    <script>
        // Configure the AWS SDK
        AWS.config.update({
            accessKeyId: '',
            secretAccessKey: '',
            region: ''
        });

        const s3 = new AWS.S3({
            params: { Bucket: 'anotherwebtos3' }
        });

        function uploadFile() {
            const fileInput = document.getElementById('fileInput');
            const file = fileInput.files[0];

            if (!file) {
                alert('Please select a file.');
                return;
            }

            const params = {
                Bucket: 'anotherwebtos3',
                Key: file.name,
                Body: file,
                ContentType: file.type
            };

            s3.upload(params, function (err, data) {
                if (err) {
                    console.error(err);
                    alert('Error uploading file.');
                } else {
                    console.log('Successfully uploaded file.', data);
                    alert('File uploaded successfully!');
                }
            });
        }
    </script>
</body>
</html>


Bucket Policy

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:PutObject",
            "Resource": "arn:aws:s3:::anotherwebtos3/*"
        }
    ]
}


Cors

[
    {
        "AllowedHeaders": [
            "*"
        ],
        "AllowedMethods": [
            "GET",
            "POST",
            "PUT"
        ],
        "AllowedOrigins": [
            "*"
        ],
        "ExposeHeaders": [
            "ETag"
        ],
        "MaxAgeSeconds": 3000
    }
]
