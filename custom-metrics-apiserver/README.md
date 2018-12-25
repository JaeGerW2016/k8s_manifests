#Creating a Secret Using kubectl create secret

```
export PURPOSE=serving

openssl req -x509 -sha256 -new -nodes -days 365 -newkey rsa:2048 -keyout ${PURPOSE}.key -out ${PURPOSE}.crt -subj "/CN=ca"

kubectl -n custom-metrics create secret generic cm-adapter-serving-certs --from-file=./serving.crt --from-file=./serving.key
```
