# Deploy Instance

## Create Key Pair

* [AWS CLI - Describe VPCs](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ec2/create-key-pair.html)

```
aws ec2 create-key-pair ^
    --key-name KEY-I346-SUB-DEVOPSTEAM99 ^
    --key-type rsa ^
    --key-format pem ^
    --tag-specifications ResourceType=key-pair,Tags=[{Key=Name,Value=KEY-I346-SUB-DEVOPSTEAM99}] ^
    --region eu-central-1 ^
    --profile devopsteam99-i346 ^
    --output text > KEY-I346-SUB-DEVOPSTEAM99.pem
```

OUTPUT
```
a file "<name>.pem" has been created
```

## Create Security Group

* [AWS CLI - Create Security Group](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ec2/create-security-group.html)

```
aws ec2 create-security-group ^
    --group-name secugrp-i346-devopsteam99b ^
    --description secugrp-i346-devopsteam99b ^
    --vpc-id vpc-0a22d771f16ae549d ^
    --tag-specifications ResourceType=security-group,Tags=[{Key=Name,Value=SG-DEVOPSTEAM99-SUBNET}] ^
    --region eu-central-1 ^
    --profile devopsteam99-i346 ^
    --output table
```

OUTPUT
```
--------------------------------------
|         CreateSecurityGroup        |
+----------+-------------------------+
|  GroupId |  sg-018c6f1ef3f734169   |
+----------+-------------------------+
||               Tags               ||
|+------+---------------------------+|
||  Key |           Value           ||
|+------+---------------------------+|
||  Name|  SG-DEVOPSTEAM99-SUBNET   ||
|+------+---------------------------+|
```

## Authorize Security Group - Ingress

* [AWS CLI - Modify security group](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ec2/authorize-security-group-ingress.html)

```
aws ec2 authorize-security-group-ingress ^
    --group-id sg-070fcc0050052d4c1 ^
    --ip-permissions "IpProtocol=tcp,FromPort=22,ToPort=22,IpRanges=[{CidrIp=10.0.0.0/28,Description=SSH-FROM-DMZ}]" ^
    --region eu-central-1 ^
    --profile devopsteam99-i346 ^
    --output table
```

OUTPUT
```
----------------------------------------------------
|           AuthorizeSecurityGroupIngress          |
+---------------------------+----------------------+
|  Return                   |  True                |
+---------------------------+----------------------+
||               SecurityGroupRules               ||
|+----------------------+-------------------------+|
||  CidrIpv4            |  10.0.0.0/28            ||
||  Description         |  SSH-FROM-DMZ           ||
||  FromPort            |  22                     ||
||  GroupId             |  sg-070fcc0050052d4c1   ||
||  GroupOwnerId        |  709024702237           ||
||  IpProtocol          |  tcp                    ||
||  IsEgress            |  False                  ||
||  SecurityGroupRuleId |  sgr-00611ff95bbc17c85  ||
||  ToPort              |  22                     ||
|+----------------------+-------------------------+|
```

## Run Instance

* [AWS CLI - Run instance](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ec2/run-instances.html)

```
aws ec2 run-instances ^
    --image-id ami-0584590e5f0e97daa ^
    --instance-type t2.micro ^
    --key-name KEY-I346-SUB-DEVOPSTEAM99 ^
    --subnet-id subnet-026ee5b4de5a53a01 ^
    --security-group-ids sg-070fcc0050052d4c1 ^
    --private-ip-address 10.0.99.10 ^
    --region eu-central-1 ^
    --profile devopsteam99-i346 ^
    --output table
```

OUTPUT
```
------------------------------------------------------------------------------
|                                RunInstances                                |
+-------------------------------+--------------------------------------------+
|  OwnerId                      |  709024702237                              |
|  ReservationId                |  r-07955ab3d8ebc7226                       |
+-------------------------------+--------------------------------------------+
||                                 Instances                                ||
|+--------------------------+-----------------------------------------------+|
||  AmiLaunchIndex          |  0                                            ||
||  Architecture            |  x86_64                                       ||
||  ClientToken             |  3f189b3e-299f-4e3f-8781-0daedfb02059         ||
||  CurrentInstanceBootMode |  legacy-bios                                  ||
||  EbsOptimized            |  False                                        ||
||  EnaSupport              |  True                                         ||
||  Hypervisor              |  xen                                          ||
||  ImageId                 |  ami-0584590e5f0e97daa                        ||
||  InstanceId              |  i-031ea87861027aca8                          ||
||  InstanceType            |  t2.micro                                     ||
||  KeyName                 |  KEY-I346-SUB-DEVOPSTEAM99                    ||
||  LaunchTime              |  2025-03-17T18:10:05+00:00                    ||
||  PrivateDnsName          |  ip-10-0-99-14.eu-central-1.compute.internal  ||
||  PrivateIpAddress        |  10.0.99.14                                   ||
||  PublicDnsName           |                                               ||
||  RootDeviceName          |  /dev/xvda                                    ||
||  RootDeviceType          |  ebs                                          ||
||  SourceDestCheck         |  True                                         ||
||  StateTransitionReason   |                                               ||
||  SubnetId                |  subnet-026ee5b4de5a53a01                     ||
||  VirtualizationType      |  hvm                                          ||
||  VpcId                   |  vpc-0a22d771f16ae549d                        ||
|+--------------------------+-----------------------------------------------+|
|||                    CapacityReservationSpecification                    |||
||+----------------------------------------------------------+-------------+||
|||  CapacityReservationPreference                           |  open       |||
||+----------------------------------------------------------+-------------+||
|||                               CpuOptions                               |||
||+--------------------------------------------------------+---------------+||
|||  ThreadsPerCore                                        |  1            |||
||+--------------------------------------------------------+---------------+||
|||                             EnclaveOptions                             |||
||+---------------------------------------+--------------------------------+||
|||  Enabled                              |  False                         |||
||+---------------------------------------+--------------------------------+||
|||                           MaintenanceOptions                           |||
||+------------------------------------------+-----------------------------+||
|||  AutoRecovery                            |  default                    |||
||+------------------------------------------+-----------------------------+||
||+-------------------------------------------------+----------------------+||
|||  HttpEndpoint                                   |  enabled             |||
|||  HttpProtocolIpv6                               |  disabled            |||
|||  HttpPutResponseHopLimit                        |  1                   |||
|||  HttpTokens                                     |  optional            |||
|||  InstanceMetadataTags                           |  disabled            |||
|||  State                                          |  pending             |||
||+-------------------------------------------------+----------------------+||
|||                               Monitoring                               |||
||+------------------------------+-----------------------------------------+||
||+------------------------------+-----------------------------------------+||
|||                            NetworkInterfaces                           |||
||+-------------------------------+----------------------------------------+||
|||  Description                  |                                        |||
|||  InterfaceType                |  interface                             |||
|||  MacAddress                   |  02:64:10:74:68:27                     |||
|||  NetworkInterfaceId           |  eni-07c1e27faad9be407                 |||
|||  OwnerId                      |  709024702237                          |||
|||  PrivateIpAddress             |  10.0.99.14                            |||
|||  SourceDestCheck              |  True                                  |||
|||  SubnetId                     |  subnet-026ee5b4de5a53a01              |||
|||  VpcId                        |  vpc-0a22d771f16ae549d                 |||
||+-------------------------------+----------------------------------------+||
||||                              Attachment                              ||||
|||+----------------------------+-----------------------------------------+|||
||||  AttachTime                |  2025-03-17T18:10:05+00:00              ||||
||||  AttachmentId              |  eni-attach-0f9b4c59cde5a422b           ||||
||||  DeleteOnTermination       |  True                                   ||||
||||  DeviceIndex               |  0                                      ||||
||||  NetworkCardIndex          |  0                                      ||||
|||+----------------------------+-----------------------------------------+|||
||||                                Groups                                ||||
|||+--------------------+-------------------------------------------------+|||
||||  GroupId           |  sg-070fcc0050052d4c1                           ||||
||||  GroupName         |  secugrp-i346-devopsteam99                      ||||
|||+--------------------+-------------------------------------------------+|||
||||                          PrivateIpAddresses                          ||||
|||+----------------------------------------+-----------------------------+|||
||||  Primary                               |  True                       ||||
-- M  PrivateIpAddress                      |  10.0.99.14                 ||||
3m|||
|||                                Placement                               |||
||+--------------------------------------+---------------------------------+||
|||  AvailabilityZone                    |  eu-central-1a                  |||
|||  GroupName                           |                                 |||
|||  Tenancy                             |  default                        |||
||+--------------------------------------+---------------------------------+||
|||                          PrivateDnsNameOptions                         |||
||+------------------------------------------------------+-----------------+||
|||  EnableResourceNameDnsAAAARecord                     |  False          |||
|||  EnableResourceNameDnsARecord                        |  False          |||
      |||
||+------------------------------------------------------+-----------------+||
|||                             SecurityGroups                             |||
||+---------------------+--------------------------------------------------+||
|||  GroupId            |  sg-070fcc0050052d4c1                            |||
|||  GroupName          |  secugrp-i346-devopsteam99                       |||
||+---------------------+--------------------------------------------------+||
|||                                  State                                 |||
||+-----------------------------+------------------------------------------+||
|||  Code                       |  0                                       |||
|||  Name                       |  pending                                 |||
||+-----------------------------+------------------------------------------+||
|||                               StateReason                              |||
||+-----------------------------------+------------------------------------+||
|||  Code                             |  pending                           |||
|||  Message                          |  pending                           |||
||+-----------------------------------+------------------------------------+||
```
