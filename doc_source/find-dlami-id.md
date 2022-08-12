# DLAMI ID<a name="find-dlami-id"></a>

Find the ID for the DLAMI of your choice with the AWS Command Line Interface \(AWS CLI\)\. If you do not already have the AWS CLI installed, see [Getting started with the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-started)\.

1. Make sure that your AWS credentials are configured\.

   ```
   aws configure
   ```

1. Choose a DLAMI and check the details in the [release notes](https://docs.aws.amazon.com/dlami/latest/devguide/appendix-ami-release-notes)\. Use the following command to get the ID for the DLAMI of your choice:

   ```
   aws ec2 describe-images --region us-east-1 --owners amazon \
   --filters 'Name=name,Values=Deep Learning AMI (Ubuntu 18.04) Version ??.?' 'Name=state,Values=available' \
   --query 'reverse(sort_by(Images, &CreationDate))[:1].ImageId' --output text
   ```
**Note**  
You can specify a release version for a given framework or get the latest release by replacing the version number with a question mark\.

1. The output should look similar to the following:

   ```
   ami-094c089c38ed069f2
   ```

   Copy this DLAMI ID and press `q` to exit the prompt\.

**Next Step**  
[EC2 Console](launch-from-console.md)