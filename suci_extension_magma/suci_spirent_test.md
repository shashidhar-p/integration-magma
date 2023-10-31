# Test Suci Extension in Magma with spirent

### I. Generate the keys
1. Generate the private key:
   ```
   openssl genpkey -algorithm x25519 -out x25519.key.pem
   ``` 
2. To view the keys:
   ```
   openssl pkey -in x25519.key.pem -text
   ``` 
   ![image](https://github.com/shashidhar-p/integration-magma/assets/42925091/1923cc77-8f82-4818-87de-4424713b7c73)

3. Format Private Key:
   
    - Private key(hex): ```48:d4:6f:5a:89:db:04:7b:f5:c1:ea:dc:5a:1c:bf:b2:06:79:22:80:af:9b:db:87:77:c9:09:0a:74:86:db:78```
   
    - Remove Colons and add "0x"(hex): ```0x48d46f5a89db047bf5c1eadc5a1cbfb206792280af9bdb8777c9090a7486db78```

    - Convert to private key to base64:
      ```
      SNRvWonbBHv1wercWhy/sgZ5IoCvm9uHd8kJCnSG23g=
      ```
      
4. Format Public Key:
   
    - Public key(hex): ```a1:1b:f9:0a:da:a0:3a:0c:df:57:9a:1d:86:33:e0:2f:43:d8:f5:3e:f8:e1:52:63:07:81:df:1f:0d:15:ef:0c```
   
    - Remove Colons and add "0x"(hex): ```0xa11bf90adaa03a0cdf579a1d8633e02f43d8f53ef8e152630781df1f0d15ef0c```

    - Convert to public key to base64:
      ```
      oRv5CtqgOgzfV5odhjPgL0PY9T744VJjB4HfHw0V7ww=
      ```
### II. Configure the suci profiles through swagger:
- **Network ID:** Check NMS to get the network id to which the AGW is connected.
- **home_network_private_key** : (generated in step 3)
      ```
      SNRvWonbBHv1wercWhy/sgZ5IoCvm9uHd8kJCnSG23g=
      ```
- **home_network_public_key** : (generated in step 4)
      ```
      oRv5CtqgOgzfV5odhjPgL0PY9T744VJjB4HfHw0V7ww=
      ```
- **home_network_public_key_identifier**: 22 (Any number between 0-255)
- **protection_scheme**: ProfileA (X25519 algo,this profile has been used as an example in this guide) **OR**
                         ProfileB (ECDH_SECP256R1 algo)
  
![image](https://github.com/shashidhar-p/integration-magma/assets/42925091/fe7bc90d-7bd6-4b11-85f9-80317cb73e40)

### III. Configure Spirent test case:
1. Session builder -> Amf nodal -> NAS-5G -> MM
   ![Screenshot from 2023-11-01 03-09-14](https://github.com/shashidhar-p/integration-magma/assets/42925091/69cac443-abf7-4572-ab7a-b1bd26cf643d)

   ![Screenshot from 2023-11-01 03-12-15](https://github.com/shashidhar-p/integration-magma/assets/42925091/455d1521-f219-4b5e-a738-b1ad3d9da4d2)


### Working pcap and logs
[suci_extension.zip](https://github.com/shashidhar-p/integration-magma/files/13221623/suci_extension.zip)

*** 
### References:
1. Magma Docs: https://magma.github.io/magma/docs/lte/suci_extensions
2. OpenSsl Output breakdown: https://stackoverflow.com/questions/60303561/how-do-i-pass-a-44-bytes-x25519-public-key-created-by-openssl-to-cryptokit-which
3. Hex to Base64 online converter: https://base64.guru/converter/encode/hex


      

