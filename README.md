## 内容介绍

AWS MediaLive 系列是AWS 广播级的媒体服务组件，可以在任意区域构建并在全球组成相应的媒体网络，目前挑战是当用户在组成直播网络后，每个MediaLive 系列的服务都是在其所在区域进行监控，这样就缺少全局的视角来监控全球范围的整体服务，对于全球的直播场景这个功能比较重要，本次采用了aws-media-servces-application-mapper的框架，实现对全球服务单一视图的可视化，并标记相应的媒体流向。

**Go [here](docs/FEATURES.md) 更多的MSAM 架构介绍.**

# Solution Overview
[//]: # (What does the solution do? What customer problem does it solve? Mention specific use cases)
* AWS Media Services Application Mapper (MSAM) is a browser-based tool that allows operators to visualize the structure and logical connections among AWS Media Services and supporting services in the cloud.
* MSAM can be used as a top-down resource monitoring tool when integrated with CloudWatch.
* MSAM offers two different visualization options: Diagrams and Tiles. 
* MSAM can be configured to automatically display AWS Media Services alerts from AWS Elemental MediaLive channels and multiplex and AWS Elemental MediaConnect.

**Go [here](docs/FEATURES.md) for more information on MSAM's capabilities and features.**

<a name="installation"></a>
## Installation Guide
Go [here](docs/INSTALL.md) 安装MSAM 到AWS account.

<a name="architecture-diagram"></a>
## Architecture Diagram
全球网络媒体流程设计：通过该流程设计，更好的监控整个全球的媒体网络及了解相应的预案
<img width="1108" alt="预案" src="https://user-images.githubusercontent.com/37872657/140631605-10353ee6-77d3-48ee-a2cd-352c2f630103.png">

架构参考：
(https://github.com/hades1712/aws-media-services-application-mapper/blob/master/docs/images/custom-nodes.jpeg)



# Customizing the Solution

<a name="prerequisites-for-customization"></a>
## 前期准备
[//]: # (Add any prerequisites for customization steps. e.g. Prerequisite: Node.js>10)

* Install/update to Python 3.x
* Install the AWS Command Line Interface (CLI)
* Create a Python [virtual environment](https://docs.python.org/3.8/library/venv.html) using [requirements.txt](deployment/requirements.txt) and activate it
* Configure the bucket name of your target Amazon S3 distribution bucket
```
export DIST_OUTPUT_BUCKET=my-bucket-name # bucket where customized code will reside
export SOLUTION_NAME=my-solution-name
export VERSION=my-version # version number for the customized code
```
_Note:_ You would have to create an S3 bucket with the prefix '_my-bucket-name-<aws_region>_'.  aws_region is where you are testing the customized solution. Also, the assets in bucket should be publicly accessible.

<a name="deploy"></a>
## 部署
[//]: # (Add commands to deploy the solution's stacks from the root of the project)

Deploy the distributable to an Amazon S3 bucket in your account. 

1. From the deployment directory run the _deploy.sh_ script. 

Script usage:
```
./deploy.sh [-b BucketBasename] [-s SolutionName] [-v VersionString] [-r RegionsForDeploy] [-p AWSProfile] [-a ACLSettings(public-read|none)] [-t DeployType(dev|release)] 
```

Example usage:
```
 ./deploy.sh -b mybucket -s aws-media-services-application-mapper -v v1.8.0 -r "us-west-2 us-east-1 us-east-2" -p default -a public-read -t dev


```

All CloudFormation templates and lambda binaries will end up in:

``` 
s3://my-bucket-aws-region/solution-name/version/
```

If deploying with type _release_, CloudFormation templates will also be written to:
``` 
s3://my-bucket-aws-region/solution-name/latest/
```

2.  Get the link of the solution template uploaded to your Amazon S3 bucket.

``` 
s3://my-bucket-aws-region/solution-name/latest/aws-media-services-application-mapper-release.template

OR

s3://my-bucket-aws-region/solution-name/version/aws-media-services-application-mapper-timestamp.template
```

3. Deploy the solution to your account by launching a new AWS CloudFormation stack using the link of the solution template in Amazon S3.

<a name="file-structure"></a>
# File structure

AWS Media Services Application Mapper consists of:

<pre>
|- deployment
|   |- assets                       [ Digest values for the templates and packaged code go to this folder and hosted on S3 by the project sponsors ]
|   |- build-s3-dist.sh             [ Script for building distributables and preparing the CloudFormation templates ]
|   |- deploy.sh                    [ Script for deploying distributables and CloudFormation templates to user's S3 bucket ]
|   |- global-s3-assets             [ CloudFormation templates get written here during custom build ]
|   |- regional-s3-assets           [ Packaged code for Lambda get written here during custom build ]
|- docs
|   |- ARCHITECTURE.md              [ 4+1 architectural views of MSAM ]
|   |- EXTENDING_MSAM.md            [ Instructions to extend MSAM with your own types ]
|   |- FEATURES.md                  [ Overview of solution features ]
|   |- INSTALL.md                   [ Installation guide for MSAM ]
|   |- MANAGED_INSTANCES.md         [ Using AWS Systems Manager and on-premise hardware ]
|   |- RESOURCE_TAGS.md             [ Tagging resources for tile and diagram creation ]
|   |- REST_API.md                  [ Overview of the MSAM REST API and use ]
|   |- UNINSTALL.md                 [ Steps to remove MSAM from your AWS account ]
|   |- USAGE.md                     [ Getting started and usage tips for the browser tool ]
|   |- WORKSHOP.md                  [ Steps for a workshop presented at re:Invent 2019 ]
|   |- behavioral-views.drawio      [ diagrams.net source for behavioral view ]
|   |- deployment-view.drawio       [ diagrams.net source for deployment view ]
|   |- images                       [ Images used in documentation ]
|   |- logical-view.drawio          [ diagrams.net source for logical view ]
|   |- physical-view.drawio         [ diagrams.net source for physical view ]
|   |- use-cases.drawio             [ diagrams.net source for use case view ]
|- source
    |- events                       [ Source files for CloudWatch Event and Alarm handling ]
    |- html                         [ Source files for browser application ]
    |- msam                         [ Source files for the MSAM REST API and scheduled tasks ]
    |- tools                        [ Scripts used in the development of MSAM ]
    |- web-cloudformation           [ Source files for the web template and custom resources ]
</pre>

<a name="license"></a>
# License

See license [here](https://github.com/awslabs/aws-media-services-application-mapper/blob/master/LICENSE).


## Navigate
Navigate to [Architecture](docs/ARCHITECTURE.md) | [Workshop](docs/WORKSHOP.md) | [Install](docs/INSTALL.md) | [Usage](docs/USAGE.md) | [Uninstall](docs/UNINSTALL.md) | [Rest API](docs/REST_API.md) | [Contributing](CONTRIBUTING.md)
