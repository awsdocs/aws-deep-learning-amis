# Elastic Inference Prerequisites<a name="ei-prerequisites"></a>

The following steps are required only if you plan to use [Elastic Inference](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-inference.html) \(EI\)\. Skip ahead to [Step 1: Launch a DLAMI ](launch.md), or go through the following three steps to prepare for launching your DLAMI with EI\.

**Note**  
Verify that EI is available in the zone you wish to use\. Make sure you launch your DLAMI in the same zone and your AWS PrivateLink Endpoint Services\.
+ [Configure Your Security Groups for Amazon EI](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/working-with-ei.html#ei-security)
+ [Configure AWS PrivateLink Endpoint Services](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/working-with-ei.html#eia-privatelink)
+ [Configure an Instance Role with an Amazon EI Policy](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/working-with-ei.html#ei-role-policy)