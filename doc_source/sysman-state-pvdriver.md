# Walkthrough: Automatically update PV drivers on EC2 instances for Windows Server \(console\)<a name="sysman-state-pvdriver"></a>

Amazon Windows AMIs contain a set of drivers to permit access to virtualized hardware\. These drivers are used by Amazon EC2 to map instance store and Amazon EBS volumes to their devices\. We recommend that you install the latest drivers to improve stability and performance of your EC2 instances for Windows Server\. For more information about PV drivers, see [AWS PV Drivers](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/xen-drivers-overview.html#xen-driver-awspv)\.

The following walkthrough shows you how to configure a State Manager association to automatically download and install new AWS PV drivers when the drivers become available\.

**Before you begin**  
Before you complete the following procedure, verify that you have at least one EC2 instance for Windows Server running that is configured for Systems Manager\. For more information, see [Systems Manager prerequisites](systems-manager-prereqs.md)\. 

**To create a State Manager association that automatically updates PV drivers**

1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/), expand **Systems Manager Services** in the navigation pane, and then choose **State Manager**\.

1. Choose **Create Association**\.

1. In the **Association Name** field, type a descriptive name\.

1. In the **Select Document** list, choose **AWS\-ConfigureAWSPackage**\.

1. In the **Select Targets by** section, choose an option\.
**Note**  
If you choose to target instances by using tags, and you specify tags that map to Linux instances, the association succeeds on the Windows instance, but fails on the Linux instances\. The overall status of the association shows **Failed**\.

1. In the **Schedule** section, choose an option\. Updated PV drivers are only released a few times a year, so you can schedule the association to run once a month, if you want\.

1. In the **Parameters** section, choose **Install** from the **Action** list\.

1. For **Name** list, enter **AWSPVDriver**\. You can leave the **Version** field empty\.

1. In the **Advanced** section, choose **Write to S3** if you want to write association details to an Amazon S3 bucket\.

1. Disregard the **S3Region** field\. This field is deprecated\. Specify the name of your bucket in the **S3Bucket Name** field\. If want to write output to a sub\-folder, specify the sub\-folder name in the **S3Key Prefix** field\. 

1. Choose **Create Association**, and then choose **Close**\. The system attempts to create the association on the instance\(s\) and immediately apply the state\. The association status shows **Pending**\.

1. In the right corner of the **Association** page, choose the refresh button\. If you created the association on one or more EC2 instances for Windows Server, the status changes to **Success**\. If your instances are not properly configured for Systems Manager, or if you inadvertently targeted Linux instances, the status shows **Failed**\.

1. If the status is **Failed**, choose the **Instances** tab and verify that the association was successfully created on your EC2 instances for Windows Server\. If EC2 instances for Windows Server show a status of **Failed**, verify that SSM Agent is running on the instance, and verify that the instance is configured with an IAM role for Systems Manager\. For more information, see [Systems Manager prerequisites](systems-manager-prereqs.md)\.