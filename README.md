# OpenShift Route Examples


## Edge Route

create certificate
```
openssl req -x509 -newkey rsa:4096 -sha256 -days 365 -nodes \
  -keyout tls.key -out tls.crt -subj "/CN=route1.csantanapr.com"
```

print ssl cert in text form
```
openssl x509 -in tls.crt -text
```

create route edge for host route1.csantanapr.com
```
oc create route edge route1 --service=http-app --cert tls.crt --key tls.key --hostname route1.csantanapr.com
```

curl -k https://route1.csantanapr.com


## Passthrough Route

create passthrough route2 for host route2.csantanapr.com

```
openssl req -x509 -newkey rsa:4096 -sha256 -days 365 -nodes \
  -keyout tls-route2.key -out tls-route2.crt -subj "/CN=route2.csantanapr.com"
```

```
oc create secret tls tls-secret --cert=tls-route2.crt --key=tls-route2.key
```

```
oc create route passthrough https-app-passthrough --service=https-app --hostname route2.csantanapr.com
```

curl -k https://route2.csantanapr.com

## Re-encryp Route

create re-encrypt route3  for host route3.csantanapr.com

```
oc create route reencrypt https-app-reencrypt --service=https-re-app --hostname route3.csantanapr.com
```

curl -k https://route3.csantanapr.com

