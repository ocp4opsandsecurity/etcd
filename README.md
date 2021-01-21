# etcd
Encryption notes

Check if fips is enabled to have access to proper ciphers
```
oc project openshift-config

oc debug node/master01.ocp4.disconnect.blue --to-namespace=node-scan --image=ubi7

chroot /host

fipscheck

```

Add encryption to resource defintion
```
oc edit apiserver


-------------------------
spec:
  encryption:
    type: aescbc 
    
```

After an hour verify that it is encrypted with 
```
oc get openshiftapiserver -o=jsonpath='{range .items[0].status.conditions[?(@.type=="Encrypted")]}{.reason}{"\n"}{.message}{"\n"}'
oc get kubeapiserver -o=jsonpath='{range .items[0].status.conditions[?(@.type=="Encrypted")]}{.reason}{"\n"}{.message}{"\n"}'
```

Outputs should say
```
EncryptionCompleted
...
```
