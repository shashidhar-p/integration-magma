# Quick orc8r bringup with magma:

## Setup Magma:
1.  Build magma
    ```bash
    make run 
    ```
2.  Add Subscriber 
    ```bash
    subscriber_cli.py add --lte-auth-key 465B5CE8B199B49FAA5F0A2EE238A6BC --lte-auth-opc E8ED289DEBA952E4283B54E88E6183CA IMSI001010000000001  
    
    ```
    ```bash
    subscriber_cli.py update --lte-auth-key 465B5CE8B199B49FAA5F0A2EE238A6BC --apn-config internet,9,1,0,0,3000,4000,0,,,, --apn-config oai.ipv4,9,1,0,0,3000,4000,0,,,, --apn-config INTERNET,9,1,0,0,3000,4000,0,,,, --lte-auth-opc E8ED289DEBA952E4283B54E88E6183CA IMSI001010000000001
    ```

3.  Check ports
    ```bash 
    cat /proc/net/sctp/eps
    ```

4.  (To start stop)
    ```bash 
    redis-cli -p 6380 FLUSHALL
    ```
    ```bash 
    sudo service magma@* stop
    ```
    ```bash 
    sudo service magma@magmad start
    ```


## Setup Orc8r:
### Build imges and run containers

1. HOST [magma]$  
```bash 
cd orc8r/cloud/docker
```

2. HOST [magma/orc8r/cloud/docker]$ 
```bash
./build.py --all
```
3. HOST [magma/orc8r/cloud/docker]$ 
```bash
./run.py --metrics
```

```bash
Creating orc8r_alertmanager_1     ... done
Creating orc8r_maria_1            ... done
Creating elasticsearch            ... done
Creating orc8r_postgres_1         ... done
Creating orc8r_config-manager_1   ... done
Creating orc8r_test_1             ... done
Creating orc8r_prometheus-cache_1 ... done
Creating orc8r_prometheus_1       ... done
Creating orc8r_kibana_1           ... done
Creating fluentd                  ... done
Creating orc8r_proxy_1            ... done
Creating orc8r_controller_1       ... done
```
* Check if certs are genereated

4. HOST [magma/orc8r/cloud/docker]$ 
```bash
ls ../../../.cache/test_certs
```
```bash
admin_operator.key.pem  bootstrapper.key        controller.crt          rootCA.key
admin_operator.pem      certifier.key           controller.csr          rootCA.pem
admin_operator.pfx      certifier.pem           controller.key          rootCA.srl
```
5. Change the ownership of two certs shown as follows:
```bash
sudo chown ${USER}:${USER} ../../../.cache/test_certs/admin_operator.key.pem
```
```bash
sudo chown ${USER}:${USER} ../../../.cache/test_certs/admin_operator.pfx 
```
* For chrome import the certificate manually, for firefox load certificate when prompted when you visit `Swagger-UI` else `NMS Dashboard` for the first time.
> Access `Swagger UI` at `https://localhost:9443/swagger/v1/ui`

### Bring up NMS Dashboard

1. HOST [magma]$
```bash
cd nms
```
2. hOST [magma/nms] $ 
```bash
COMPOSE_PROJECT_NAME=magmalte docker-compose build magmalte
```

3. HOST [magma/nms] $ 
```bash
docker-compose up -d
```

4. HOST [magma/nms] $ 
```bash
./scripts/dev_setup.sh
```
* Access `NMS Dashboard` at
```bash
https://magma-test.localhost/
```

> Login ID `admin@magma.test`
> Password `password1234`
