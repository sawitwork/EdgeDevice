# Install Banana PI

**Create EP**

**Create things**


**tuya_config**

```bash
{
  "state": {
    "desired": {
      "welcome": "aws-iot",
      "pid": "c6xc1vggppoha95w",
      "uuid": "uuidxxxxxxxxxxxxxxxxxxxxx",
      "authkey": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
      "storage_path": "./",
      "cache_path": "/tmp/",
      "backup_path": "./",
      "log_level": 4,
      "tuya": {
        "zigbee": {
          "dev_name": "/dev/ttyS1",
          "cts": 1,
          "thread_mode": 1
        }
      }
    },
    "reported": {
      "welcome": "aws-iot"
    }
  }
}
```

**scp file to edge**

```bash
scp -r C:/path/to/file/device.pem.crt trinity@192.168.xxx.xxx:/home/trinity

scp -r C:/path/to/file/private.pem.key trinity@192.168.xxx.xxx:/home/trinity

scp -r C:/path/to/file/public.pem.key trinity@192.168.xxx.xxx:/home/trinity
```

**edit file GreengrassInstaller/config.yaml**

```bash
sudo nano GreengrassInstaller/config.yaml
```

```bash
---

system:
  certificateFilePath: "/greengrass/v2/device.pem.crt"
  privateKeyPath: "/greengrass/v2/private.pem.key"
  rootCaPath: "/greengrass/v2/AmazonRootCA1.pem"
  rootpath: "/greengrass/v2"
  thingName: "mac-address-separate-with-dash"
services:
  aws.greengrass.Nucleus:
    componentType: "NUCLEUS"
    version: "2.9.1"
    configuration:
      awsRegion: "ap-southeast-1"
      iotRoleAlias: "GreengrassCoreTokenExchangeRoleAlias"
      iotDataEndpoint: "a5jn23p5uy8fw-ats.iot.ap-southeast-1.amazonaws.com"
      iotCredEndpoint: "c3tgw5t1kalxdl.credentials.iot.ap-southeast-1.amazonaws.com"
```

```bash
sudo mv device.pem.crt private.pem.key public.pem.key /greengrass/v2/
```

Check version Greengrass
```bash
java -jar ./GreengrassInstaller/lib/Greengrass.jar --version
```

```bash
sudo -E java -Droot="/greengrass/v2" -Dlog.store=FILE -jar ./GreengrassInstaller/lib/Greengrass.jar --init-config ./GreengrassInstaller/config.yaml --component-default-user trinity:trinity --setup-system-service true
```

Policies of Things `trinity-iot-thing-policy` * for AWS role `scg-trinity-prod`


**Manually uninstall Greengrass**
Stop the service
```bash
sudo systemctl stop greengrass.service
```
Disable the service
```bash
sudo systemctl disable greengrass.service
```

Remove the service
```bash
sudo rm /etc/systemd/system/greengrass.service
```

Verify that the service is deleted
```bash
sudo systemctl daemon-reload && sudo systemctl reset-failed
```

X Remove Greengrass root folder X
```bash
sudo rm -rf /greengrass/v2
```
