# AWS.S3.Dotnet.Beta

![Banner Animation](https://your-animation-url.com/banner.gif)

## Overview

AWS.S3.Dotnet.Beta is a .NET project that provides a set of examples and utilities for interacting with AWS S3 buckets. This project demonstrates how to perform common operations such as uploading, downloading, and listing objects in an S3 bucket.

![Overview Animation](https://your-animation-url.com/overview.gif)

## Features

- **Upload files to an AWS S3 bucket**
- **Download files from an AWS S3 bucket**
- **List objects within an S3 bucket**
- **Basic error handling and logging**

![Features Animation](https://your-animation-url.com/features.gif)

## Getting Started

### Prerequisites

- **.NET Core SDK** (version 6.0 or later)
- **An AWS account**
- **AWS CLI configured** with your credentials or environment variables set for AWS access

### Installation

1. **Clone the repository:**

    ```bash
    git clone https://github.com/maduradevDotNet/AWS.S3.Dotnet.Beta.git
    ```

    ![Clone Repo Animation](https://your-animation-url.com/clone-repo.gif)

2. **Navigate to the project directory:**

    ```bash
    cd AWS.S3.Dotnet.Beta
    ```

3. **Restore the project dependencies:**

    ```bash
    dotnet restore
    ```

4. **Install the AWS SDK for .NET:**

    ```bash
    dotnet add package AWSSDK.S3
    ```

5. **Update the `appsettings.json` file** with your AWS credentials and S3 bucket information:

    ```json
    {
      "AWS": {
        "AccessKey": "your-access-key",
        "SecretKey": "your-secret-key",
        "Region": "your-region"
      },
      "S3": {
        "BucketName": "your-bucket-name"
      }
    }
    ```

6. **Run the application:**

    ```bash
    dotnet run
    ```

    ![Run App Animation](https://your-animation-url.com/run-app.gif)

## Usage

- **Upload File**

    ```csharp
    await S3Helper.UploadFileAsync("local-file-path", "s3-key");
    ```

- **Download File**

    ```csharp
    await S3Helper.DownloadFileAsync("s3-key", "local-file-path");
    ```

- **List Objects**

    ```csharp
    var objects = await S3Helper.ListObjectsAsync();
    ```

![Usage Animation](https://your-animation-url.com/usage.gif)

## Example Code

**Uploading a File**

```csharp
using Amazon.S3;
using Amazon.S3.Model;
using System.Threading.Tasks;

public class S3Helper
{
    private readonly IAmazonS3 _s3Client;
    private readonly string _bucketName;

    public S3Helper(IAmazonS3 s3Client, string bucketName)
    {
        _s3Client = s3Client;
        _bucketName = bucketName;
    }

    public async Task UploadFileAsync(string filePath, string keyName)
    {
        var fileTransferUtility = new TransferUtility(_s3Client);
        await fileTransferUtility.UploadAsync(filePath, _bucketName, keyName);
    }

    public async Task DownloadFileAsync(string keyName, string filePath)
    {
        var fileTransferUtility = new TransferUtility(_s3Client);
        await fileTransferUtility.DownloadAsync(filePath, _bucketName, keyName);
    }

    public async Task<List<S3Object>> ListObjectsAsync()
    {
        var listObjectsResponse = await _s3Client.ListObjectsAsync(new ListObjectsRequest
        {
            BucketName = _bucketName
        });

        return listObjectsResponse.S3Objects;
    }
}
