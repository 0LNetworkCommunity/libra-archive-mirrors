# Libra Archive Mirror
Publicly Accessible Libra Archives provided by community members. All archives should be accessible by common S3 compatible storage tools (e.g. AWS cli, `rclone`);

## Contents
1. `libra-continuous/backup`: a continuous backup since genesis of V7, to be used with streaming backup cli
1. `libra-archive/archive-node-databases`: rocks db local files (the files saved on disk) for end state of v5, v6, and ongoing for v7 network.
1. `libra-archive/epoch-archive`: archive of the github repo for daily backup files in formats of v6 and v7
1. `libra-archive/v5-json-transactions` transactions as saved by the v5 explorer.


# Quick Start
Examples using `rclone` https://rclone.org/

Examples below assume you have keys, and that you named the remote filestorage as `remote-mirror`
## config
```
rclone config
# follow steps for S3 compatible provider
# repeat for your own S3 provider
```

## sync to a local directory
```
# note the example name "remote-mirror" is whatever you entered in the config step above
# however "libra-archives" cannot be changed, it is the name of the hosted bucket
rclone sync remote-mirror:libra-archives ./path/to/local --progress
```

# create own mirror 
Does a server to server copy, without passing through local host
```
# note: the name "my-mirror" refers to whatever you entered in the config step above
rclone sync remote-mirror:libra-archives my-mirror:libra-archives
```

# Continuous Backups
The diem backup cli tool has a continuous streaming service for backing up to a cloud provider.
We have a standard file for using `rclone` to connect to any S3 compatible service. See `rclone.backup.yaml` in this repo.
This file serves as a dictionary to system tools which are used to compress (gzip) and save (rclone) files afer calling the diem-node backup service port.

IMPORTANT: If the rclone.backup.yaml is modified, it is not guaranteed to work with any previous backups hosted by community members

## restoring
Using the libra binary you should be able to restore from this backup with:
```bash
libra ops storage db restore bootstrap-db --target-db-dir --command-adapter-config <path/to>/rclone.backup.yaml
```

## creating backups
Note: to create new backups the diem-node must be running (and the standard "backup service" port must be accessible).

```bash
libra ops storage db backup continuously --command-adapter-config <path/to>/rclone.backup.yaml
```


# Sources
## 0D
Digital Ocean Spaces

Endpoint
https://libra-archives.nyc3.digitaloceanspaces.com

bucket:
libra-archives
(or libra-continuous for backup streams)

Key ID: 
DO801YTLHT9FZ6N3U7BK

Secret Key:
8NiBqCi8XFrdCO/HVOEfQy00vIJ3SRq9r8MLyJjrhKg
