# Quick gNBsim Integration with magma:

## Setup Magma:
1.  Clone the repository 
    ```bash
    git clone https://github.com/magma/magma.git 
    ```
2.  ```bash 
    cd magma/lte/gateway
    ```

3. ```bash 
    vagrant up magma 
    ```

4. ```bash 
    vagrant ssh magma
    ```

5. ```bash 
    cd magma/lte/gateway
    ```

6. enable5gfeatures in gateway.mconfig

7. ```bash 
    make run 
    ```

8. Add subscriber 

    1. ```bash 
        cd ~/magma/lte/gateway/python/scripts 
        ```

    2. ```bash
        magtivate
        ```

    3. ```bash 
        subscriber_cli.py add --lte-auth-key 465B5CE8B199B49FAA5F0A2EE238A6BC --lte-auth-opc E8ED289DEBA952E4283B54E88E6183CA IMSI001010000000001 
        ```

    4. ```bash 
        subscriber_cli.py update --lte-auth-key 465B5CE8B199B49FAA5F0A2EE238A6BC --apn-config internet,9,1,0,0,3000,4000,0,,,, --apn-config oai.ipv4,9,1,0,0,3000,4000,0,,,, --apn-config INTERNET,9,1,0,0,3000,4000,0,,,, --lte-auth-opc E8ED289DEBA952E4283B54E88E6183CA IMSI001010000000001 
        ```
- - -
<br/>


## Setup Omec gNB sim:
1. Clone the repository 
   ```bash
   git clone https://github.com/omec-project/gnbsim.git 
   ```

2. ```bash
    cd gnbsim
    ```

3. Build the image by running

   ```bash
   go build 
   ```
   A “gnbsim” executable is created.

4. Set the parameters in the config file manually.

    OR

	Fetch and replace the config file.

    [Example config](https://github.com/shashidhar-p/integration-magma/blob/main/omec-gnbsim/config/gnbsim.yaml)

5. Enable the profiles to be executed.(enable=true in config/gnbsim.yaml)

6. Run gNB sim with the config file.

    ```bash
    ./gnbsim --cfg config/gnbsim.yaml 
    ```

7. To run the enabled profiles:

    ```bash 
    curl -i -X GET 127.0.0.1:8080/gnbsim/v1/executeConfigProfile 
    ```
<br/>
<p style="text-align: center; font-size: 30px" >
OR
</p>
<br/> 

## Setup Omec gNB sim [DOCKER]:

1. Prerequisites:
    - [Install docker](https://docs.docker.com/engine/install/ubuntu/)
    - [Install Go](https://go.dev/doc/install)

2. Clone the repository 
   ```bash
   git clone https://github.com/omec-project/gnbsim.git 
   ```
3. ```bash
    cd gnbsim
    ```
4. Build the docker image

   ```bash
   make docker-build 
   ```
5. Set the parameters in the config file manually.(gnbsim/config/gnbsim.yaml)

    OR

	Fetch and replace the config file.

    [Example config](https://github.com/shashidhar-p/integration-magma/blob/main/omec-gnbsim/config/gnbsim.yaml)

6. Bring up docker
   ```bash
    docker run -t -d -P -v /home/vagrant/gnbsim/config:/gnbsim/bin/config --name gnbsim 5gc-gnbsim:0.0.1-dev
   ```

7. Run gNB sim with the config file.
   ```bash
    docker exec -it gnbsim sh -c "./gnbsim --cfg config/gnbsim.yaml"
   ```
8. To run the enabled profiles:
   ```bash
    docker exec -it gnbsim sh -c "curl -i -X GET 127.0.0.1:8080/gnbsim/v1/executeConfigProfile"
   ```
