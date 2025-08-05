# Libra Archive Mirror
Publicly Accessible Libra Archives provided by community members. All archives should be accessible by common S3 compatible storage tools (e.g. AWS cli, `rclone`);

## Contents
1. `current-continuous`: diem backup format. A continuous backup since V7 upgrade, to be used with streaming backup cli. For uses with `continuous` restores
1. `epoch-archive`: diem backup format. An archive of the github repo for daily backup files in formats of v5, v6, and v7. For use with `oneoff` restores, e.g. twin testnet.
1. `db-snapshots`: rocks db local files. These are local files as saved on disk on a diem node. Includes end state of v5, v6, and ongoing for v7/8 network.
1. `explorer-data`: json files. All v5 transactions as saved from the v5 explorer.


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
Does a server to server copy, without making a copy on the local host
```
# note: the name "my-mirror" refers to whatever you entered in the rclone config step above
# important: always call your bucket `libra-archives` for consisency so others can sync
rclone sync remote-mirror:libra-archives my-mirror:libra-archives
```

# Continuous Backups
The diem backup cli tool has a continuous streaming service for backing up to a cloud provider.
We have a standard file for using `rclone` to connect to any S3 compatible service. See `rclone.backup.yaml` in this repo.
This file serves as a dictionary to system tools which are used to compress (gzip) and save (rclone) files afer calling the diem-node backup service port.

You must modify the "REMOTE" reflect your local rclone configuration. By convention "BUCKET" should be called `libra-archives`, and the "SUB_DIR" to `current-continuous`. 

IMPORTANT: If the commands in rclone.backup.yaml are modified, it is not guaranteed to work with any previous backups hosted by community members

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
Sample rclone config files found at: `$HOME/.config/rclone/rclone.conf`

## 0D - Digital Ocean
```
[do-od-remote]
type = s3
provider = DigitalOcean
access_key_id = DO801YTLHT9FZ6N3U7BK
secret_access_key = 8NiBqCi8XFrdCO/HVOEfQy00vIJ3SRq9r8MLyJjrhKg
endpoint = nyc3.digitaloceanspaces.com
acl = private
```

## 0D - Cloudflare

```
[cf-od-remote]
type = s3
provider = Cloudflare
access_key_id = f60833651da1504ecdff70053e6a5120
secret_access_key = 8c1e4ed3724e6a5c2bc41059f0041900788ad9f406f39a3e30d83231288db717
endpoint = https://afcdb03dd0764818ac9aec7fe0c0b8b5.r2.cloudflarestorage.com
acl = private
```

