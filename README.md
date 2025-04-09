# ccips-controller
Pilot 3 CCIPS controller component

The CCIPS controller is developed in go using the [`go-netconf-client`](https://github.com/openshift-telco/go-netconf-client) library.


## API Endpoints

## How to launch
If you are running the CCIPS Controller, directly using the code, you need to first install golang using the instructions from [here](https://go.dev/doc/install).

Then inside the directory of the CCIPS Controller run 
```bash!
go mod tidy
```
This will automatically download all the needed dependencies.

To launch the controller you can go to the folder `./cmd/server/` and run the following command
```bash!
go run main.go
```
It will prompt the following message `INFO: 2023/10/23 15:24:19 main.go:12: HTTP server started`
### Docker version
First build the controller
```bash
docker build -t ccips_controller .
```
To run it, by default it runs at port 5000, so you can run the docker image as follows:
```bash
docker run -it --rm -p 5000:5000 ccips_controller
```

It will prompt the following message `INFO: 2025/04/09 09:24:19 main.go:12: HTTP server started`

# Requests for the controller:

* Nodes: Information of the nodes with the following:
    - ipData: IP with which the other agent is going to see it and the one it is going to use to raise the tunnel.
    - ipControl: IP that it has in the control network.
    - ipDMZ: Agent's private IP. (G2G)
    - networkInternal: Private subnet. (G2G)
* encAlg: Algorithm used by the tunnel to encrypt. supports:
 - des
 - 3des
 - aes
* intAlg: Algorithm used by the tunnel to check the integrity of the packets. supports:
 - hmac-md5-96
 - hmac-md5-128
 - hmac-sha1-96
 - hmac-sha1-160
 - hmac-sha2-256
* softLifeTime: Time for initialising the rekey process.
* hardLifeTime: Time in which if the rekey has not been performed, it throws the ipsec link.



```
# H2H

```xml
curl -X 'POST' \
  'http://controller_ip:5000/ccips' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "nodes": [
      {
        "ipData": "10.0.0.201",
        "ipControl": "192.168.165.128"
      },
    {
      "ipData": "10.0.0.10",
      "ipControl": "192.168.165.169"
    }
  ],
  "encAlg": [
    "aes-cbc"
  ],
  "intAlg": [
    "sha2-256"
  ],
  "softLifetime": {
    "nTime": 25
  },
  "hardLifetime": {
    "nTime": 50
  }
}'


```
