## AWS VPC Deployment
For Team who will own the xcalr operations.
### VPC Comps. Requirement
#### xcalr EC2 Requrement:
1) AMI with built-in docker.
2) CPU x2
3) 4G Memory.

#### xcalr AWS comp. Requirement:
1) Access Key/Secret. E.g. AKIARVS6YSG2BLAH / Blahblahpwd
2) AMI ID# to bootstrap the subscription's watcher's instances/vm. E.g. ami-02f706d959cedf892
3) Subnet ID(s)# for the ingress of the watcher instances/vms. E.g. subnet-ac714fc4, subnet-a97307d3, etc.
4) Security Group ID#. E.g. sg-03138795f467b83b0.
5) Region ID#. E.g. "us-east-2", "us-east-1"

#### EC Watcher Env Requirement:
1) AMI with built-in Docker.
2) CPU x1
3) 1G Memory.
4) Please refer to [the agent build repo](https://github.build.ge.com/Enterprise-Connect/agent#D1) for the more detail.

EC Watcher Requirement:
### Internal Onboarding Steps
- [ ] A PUBLIC FQDN to identify the watcher endpoint. Generally it will be the AWS LB in the target vpc. (power.ge.com, etc.) This can be found in the corporate [formsA](https://dnsrequest.corporate.ge.com), and [instruction](http://sc.ge.com/*GEKB2011856)
- [ ] OPTIONAL: A PUBLIC TLS cert matching the DNS, if the traffic requires the secured TLS/HTTPS channel. [Forms can be found here](https://certificates.ge.com/)
- [ ] OPTIONAL: Upload the certificate acquired in step 2 to the LB via the AWS admin UI console. This step will require an existing AWS LB.
- [ ] Aquire a credential set for xcalrii with the permission set below-
- [ ] Running xcalrii as a separate instance for Power. This step does not need a client VPC, it may likely be deployed in EC's VPC.
    * Running xcalr command: docker run -p 8080:8080 -v /home/xcalr/:/opt/xcalr -e TC_PROJECT=bangalore -e TC_REV=v2beta -e TC_BUILD="75" dtr.predix.io/dig-digiconnect/xcalrii:v2beta.bangalore.75 &
- [ ] Deploy/create a ALB via xcalrii, if step 3 is ignored.
- [ ] Configure [the watcher.yml here](https://github.com/Enterprise-connect/ec-x-sdk/blob/v1.1beta.watcher/watcher.yml), this can be done by the either Product/Ops team.
### The xcalr Permission Set
The permission setis required to cover the complete functionality of xcalr.
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "elasticloadbalancing:ModifyListener",
                "ec2:DescribeInstances",
                "ec2:AttachInternetGateway",
                "ec2:DescribeSnapshots",
                "ec2:DescribeHostReservationOfferings",
                "ec2:DescribeVolumeStatus",
                "ec2:StartInstances",
                "acm:GetCertificate",
                "ec2:DescribeScheduledInstanceAvailability",
                "ec2:DescribeVolumes",
                "ec2:DescribeFpgaImageAttribute",
                "iam:UploadSSHPublicKey",
                "ec2:DescribeExportTasks",
                "elasticloadbalancing:AddListenerCertificates",
                "acm:ImportCertificate",
                "ec2:DescribeKeyPairs",
                "ec2:DescribeReservedInstancesListings",
                "ec2:DescribeCapacityReservations",
                "ec2:DescribeClientVpnRoutes",
                "ec2:DescribeSpotFleetRequestHistory",
                "ec2:CreateTags",
                "ec2:DescribeVpcClassicLinkDnsSupport",
                "ec2:DescribeSnapshotAttribute",
                "ec2:DescribeIdFormat",
                "ec2:ModifyNetworkInterfaceAttribute",
                "acm:AddTagsToCertificate",
                "ec2:RunInstances",
                "ec2:StopInstances",
                "ec2:DescribeVolumeAttribute",
                "ec2:CreateVolume",
                "ec2:DescribeImportSnapshotTasks",
                "ec2:GetPasswordData",
                "ec2:DescribeVpcEndpointServicePermissions",
                "ec2:DescribeTransitGatewayAttachments",
                "ec2:DescribeScheduledInstances",
                "ec2:CreateSnapshots",
               "elasticloadbalancing:DescribeAccountLimits",
                "ec2:DescribeImageAttribute",
                "ec2:DescribeFleets",
                "ec2:DescribeReservedInstancesModifications",
                "ec2:DescribeSubnets",
                "ec2:AttachVolume",
                "ec2:DescribeMovingAddresses",
                "ec2:DescribeFleetHistory",
                "ec2:ImportVolume",
                "ec2:DescribePrincipalIdFormat",
                "ec2:DescribeFlowLogs",
                "ec2:DescribeRegions",
                "ec2:DescribeTransitGateways",
                "ec2:DescribeVpcEndpointServices",
                "ec2:DescribeSpotInstanceRequests",
                "elasticloadbalancing:SetRulePriorities",
                "elasticloadbalancing:RemoveListenerCertificates",
                "ec2:DescribeVpcAttribute",
                "ec2:ExportClientVpnClientCertificateRevocationList",
                "ec2:DescribeTransitGatewayRouteTables",
                "ec2:DescribeAvailabilityZones",
                "ec2:DescribeNetworkInterfaceAttribute",
                "ec2:CreateSnapshot",
                "elasticloadbalancing:DescribeListenerCertificates",
                "ec2:DescribeVpcEndpointConnections",
                "ec2:DescribeInstanceStatus",
                "ec2:RebootInstances",
                "elasticloadbalancing:CreateLoadBalancer",
                "ec2:DescribeHostReservations",
                "ec2:DescribeBundleTasks",
                "ec2:DescribeIdentityIdFormat",
                "ec2:DescribeClassicLinkInstances",
                "ec2:DescribeVpcEndpointConnectionNotifications",
                "ec2:DescribeSecurityGroups",
                "ec2:DescribeFpgaImages",
                "ec2:DescribeVpcs",
                "elasticloadbalancing:DescribeTargetGroups",
                "elasticloadbalancing:DeleteListener",
                "ec2:DescribeStaleSecurityGroups",
                "acm:DeleteCertificate",
                "ec2:DescribeAggregateIdFormat",
                "ec2:ExportClientVpnClientConfiguration",
                "ec2:DescribeVolumesModifications",
                "ec2:CreateImage",
                "ec2:GetHostReservationPurchasePreview",
                "ec2:DescribeClientVpnConnections",
               "ec2:DescribeByoipCidrs",
                "acm:RenewCertificate",
                "ec2:GetConsoleScreenshot",
                "ec2:DescribePlacementGroups",
                "elasticloadbalancing:SetWebAcl",
                "ec2:DescribeInternetGateways",
                "elasticloadbalancing:DescribeLoadBalancers",
                "ec2:SearchTransitGatewayRoutes",
                "ec2:GetLaunchTemplateData",
                "acm:RequestCertificate",
                "ec2:DescribeSpotDatafeedSubscription",
                "elasticloadbalancing:CreateRule",
                "ec2:DescribeAccountAttributes",
                "ec2:DescribeNetworkInterfacePermissions",
                "ec2:DescribeReservedInstances",
                "ec2:DescribeNetworkAcls",
                "ec2:DescribeRouteTables",
                "ec2:DescribeClientVpnEndpoints",
                "ec2:DescribeEgressOnlyInternetGateways",
                "ec2:DetachVolume",
                "ec2:DescribeLaunchTemplates",
                "ec2:DescribeVpnConnections",
                "ec2:DescribeVpcPeeringConnections",
                "ec2:DescribeReservedInstancesOfferings",
                "ec2:GetTransitGatewayAttachmentPropagations",
                "ec2:DescribeFleetInstances",
                "ec2:DescribeClientVpnTargetNetworks",
                "ec2:DescribeVpcEndpointServiceConfigurations",
                "ec2:DescribePrefixLists",
                "ec2:GetReservedInstancesExchangeQuote",
                "acm:ListTagsForCertificate",
                "ec2:DescribeInstanceCreditSpecifications",
                "ec2:DescribeVpcClassicLink",
                "elasticloadbalancing:DescribeLoadBalancerAttributes",
                "elasticloadbalancing:DescribeTargetGroupAttributes",
                "acm:DescribeCertificate",
                "acm:RemoveTagsFromCertificate",
                "ec2:CreateFlowLogs",
                "elasticloadbalancing:ModifyRule",
                "ec2:GetTransitGatewayRouteTablePropagations",
                "elasticloadbalancing:DescribeRules",
                "ec2:DescribeVpcEndpoints",
                "ec2:DescribeElasticGpus",
                "ec2:DescribeVpnGateways",
                "ec2:DescribeAddresses",
                "ec2:DeleteTags",
                "ec2:DescribeInstanceAttribute",
                "ec2:DescribeDhcpOptions",
                "ec2:GetConsoleOutput",
                "ec2:DescribeSpotPriceHistory",
                "elasticloadbalancing:DescribeListeners",
                "ec2:DescribeNetworkInterfaces",
                "acm:ListCertificates",
                "ec2:ModifyInstanceAttribute",
                "elasticloadbalancing:DeleteRule",
                "ec2:GetTransitGatewayRouteTableAssociations",
                "elasticloadbalancing:DescribeSSLPolicies",
                "iam:UploadSigningCertificate",
                "ec2:AttachClassicLinkVpc",
                "acm:ExportCertificate",
                "ec2:DescribeIamInstanceProfileAssociations",
                "elasticloadbalancing:DescribeTags",
                "ec2:DescribeTags",
                "ec2:DescribeLaunchTemplateVersions",
                "elasticloadbalancing:*",
                "ec2:DescribeImportImageTasks",
                "ec2:DescribeNatGateways",
                "ec2:DescribeCustomerGateways",
                "iam:UploadServerCertificate",
                "ec2:DescribeSpotFleetRequests",
                "ec2:DescribeHosts",
                "ec2:DescribeImages",
                "ec2:DescribeSpotFleetInstances",
                "ec2:DescribeSecurityGroupReferences",
                "ec2:DescribePublicIpv4Pools",
                "ec2:DescribeClientVpnAuthorizationRules",
                "elasticloadbalancing:DescribeTargetHealth",
                "ec2:AttachNetworkInterface",
                "ec2:DescribeTransitGatewayVpcAttachments",
                "ec2:DescribeConversionTasks"
            ],
            "Resource": "*"
        }
    ]
}
```