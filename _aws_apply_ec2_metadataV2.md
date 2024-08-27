# Linux Users - Apply AWS EC2 IMDSv2

AWS EC2 (Elastic Compute Cloud) instances provide a feature called Instance Metadata, which allows an instance to retrieve information about itself, its configuration, and its environment. This metadata is useful for various purposes, such as configuring and managing instances, automating tasks, and ensuring security. 

`apply_ec2_metadataV2.txt`

```shell
## describe instance
$ aws ec2 describe-instances --instance-ids i-1234abcd237b15ef1

## apply metadata options IMDSv2
$ aws ec2 modify-instance-metadata-options --http-tokens required --http-endpoint enabled --instance-id i-1234abcd237b15ef1
"MetadataOptions": {
    "State": "applied",
    "HttpTokens": "required",
    "HttpPutResponseHopLimit": 1,
    "HttpEndpoint": "enabled",
    "HttpProtocolIpv6": "disabled",
    "InstanceMetadataTags": "disabled"
}

## rollback metadata options IMDSv2
$ aws ec2 modify-instance-metadata-options --http-tokens optional --http-endpoint enabled --instance-id i-1234abcd237b15ef1
"MetadataOptions": {
    "State": "applied",
    "HttpTokens": "optional",
    "HttpPutResponseHopLimit": 1,
    "HttpEndpoint": "enabled",
    "HttpProtocolIpv6": "disabled",
    "InstanceMetadataTags": "disabled"
}

```

Instance Metadata can be accessed through the Instance Metadata Service (IMDS), and historically, it has used version 1 of the IMDS (IMDSv1). However, due to security concerns and potential vulnerabilities, AWS introduced Instance Metadata Service version 2 (IMDSv2) as an improved and more secure version. Here's why it's important to apply AWS EC2 Metadata version 2 (IMDSv2):

- Enhanced Security
- Protects Against SSRF Attacks
- Token-based Authentication
- Prevents Token Theft
- Auditing and Accountability
- AWS Best Practice



