# Libra Archive Mirror
Publicly Accessible Libra Archives provided by community members. All archives should be accessible by common S3 compatible storage tools (e.g. AWS cli, `rclone`);

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
rclone sync remote-mirror:libra-archives ./path/to/local
```

# create own mirror 
Does a server to server copy, without passing through local host
```
# note: the name "my-mirror" refers to whatever you entered in the config step above
rclone sync remote-mirror:libra-archives my-mirror:libra-archives
```


# Sources
## 0D
Digital Ocean Spaces

Endpoint
https://libra-archives.nyc3.digitaloceanspaces.com

bucket:
libra-archives

Key ID: 
DO801YTLHT9FZ6N3U7BK

Secret Key:
8NiBqCi8XFrdCO/HVOEfQy00vIJ3SRq9r8MLyJjrhKg
