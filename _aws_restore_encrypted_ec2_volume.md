# Linux Users - Restoring Encrypted EC2 Volumes

Key Association: When you create an encrypted EC2 volume, you specify a KMS key to be used for encrypting and decrypting the data on that volume. This KMS key is known as the "customer master key" (CMK) in KMS terminology. The CMK is used to generate a unique data encryption key (DEK) for each volume.

Restoration Process: When you want to restore an encrypted EC2 volume, you need the appropriate KMS key to decrypt the data on the volume. This is because the data encryption key (DEK) used to encrypt the data is itself encrypted using the CMK. Without the CMK, it is not possible to decrypt the DEK, and therefore, the data on the volume remains inaccessible.

Identify: 
- a snapshot-id to restore from
- the kms key used to encrypt the ec2 volume

`restore_encrypted_volume.txt`

```shell
## eg: /dev/xvdf
## aws cli syntax
 
$ aws ec2 create-volume --region "us-east-1" \
--availability-zone "us-east-1d" --encrypted --iops 3000 --size 128 \
--volume-type gp3 --kms-key-id 123d3456-x66x-44xx-abc5-xxxxxxxxxxx \
--snapshot-id snap-123456789abcde

```


