# packer-builder-upcloud

This is a Packer builder which can be used to generate storage templates on UpCloud.

## Installation

To install the builder you'll need to download it, install it, then copy the binary to Packer's plugin directory.

```
go get github.com/jalle19/packer-builder-upcloud
cd $GOPATH/src/github.com/jalle19/packer-builder-upcloud
go install
cp $GOPATH/bin/packer-builder-upcloud ~/.packer.d/plugins
```

## Usage

Here is a sample template:

```json
{
  "variables": {
    "UPCLOUD_API_USERNAME": "your username",
    "UPCLOUD_API_PASSWORD": "your password"
  },
  "builders": [
    {
      "type": "upcloud",
      "username": "{{ user `UPCLOUD_API_USERNAME` }}",
      "password": "{{ user `UPCLOUD_API_PASSWORD` }}",
      "plan": "1xCPU-1GB",
      "zone": "fi-hel1",
      "storage_uuid": "01000000-0000-4000-8000-000030040200"
    }
  ]
}
```

If everything goes according to plan, you should see something like this:

```
packer build packer.json 
upcloud output will be in this color.

==> upcloud: Creating temporary SSH key ...
==> upcloud: Creating server "packer-builder-upcloud-1468327456" ...
==> upcloud: Waiting for server "packer-builder-upcloud-1468327456" to enter the "started" state ...
==> upcloud: Server "packer-builder-upcloud-1468327456" is now in "started" state
==> upcloud: Waiting for SSH to become available...
==> upcloud: Connected to SSH!
==> upcloud: Provisioning with shell script: scripts/update.sh
...
==> upcloud: Stopping server "packer-builder-upcloud-1468327456" ...
==> upcloud: Waiting for server "packer-builder-upcloud-1468327456" to enter the "stopped" state ...
==> upcloud: Server "packer-builder-upcloud-1468327456" is now in "stopped" state
==> upcloud: Templatizing storage device "packer-builder-upcloud-1468327456-disk1" ...
==> upcloud: Waiting for storage "packer-builder-upcloud-1468327456-disk1-template-1468327515" to enter the "online" state
==> upcloud: Waiting for server "packer-builder-upcloud-1468327456" to exit the "maintenance" state ...
==> upcloud: Deleting server "packer-builder-upcloud-1468327456" ...
Build 'upcloud' finished.

==> Builds finished. The artifacts of successful builds are:
--> upcloud: Private template (UUID: 01875f67-4eb5-4d90-982c-d7a164646fcb, Title: packer-builder-upcloud-1468327456-disk1-template-1468327515, Zone: fi-hel1)
```

## License

MIT
