A mutate webhook to replace gcr.io k8s.gcr.io quay.io to azk8s.cn mirror 

Useful when you use a component that will automatic create pod ,such as sonobuoy CNCF conformance test

Origin Code is from kubernetes

## BUILD

CGO_ENABLED=0 GOARCH=amd64 go build -a -installsuffix cgo  -o webhook ./.


## USAGE

Generate ca cert file & server cert file & key file

```bash
openssl genrsa -out ca.key 2048
openssl genrsa -out server.key 2048
openssl req -new -x509 -key ca.key -out ca.crt
openssl req -new -key server.key -out server.csr
openssl x509 -req -in server.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out server.crt
`````
Notice: The domain when you create server.crt must same as the domain you use in ValidatingWebhookConfiguration


Deploy the webhook on your way

#./webhook -tls-cert-file=server.crt -tls-private-key-file=server.key -alsologtostderr


kubectl create -f mutate.yaml to create the  MutatingWebhookConfiguration


kubectl create -f gcr.yaml   you will find the image be replaced automatically

````
