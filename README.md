# HTTPS enablement

本次實作是利用 lab6 所建設的還境實作


這次我創 1 張cert同時給 in-cse 跟 mn-cse 當然也可以二張如果是兩個不同的domain SAN(證書參數: subject alternative names)

#### 利用 java sdk build-in tool "keytool" 去生成key 跟 cert

* keystore : 是 key 的 file 的儲存路徑
請記住 你生成那個keysotre 的 keysotre password, 因為 
生成 keypair
```
keytool -genkeypair -alias incse -validity 730 -keyalg RSA -keysize 4096 -keystore /home/om2m/keystore/keystoreincse -storetype pkcs12 -dname "CN=localhost, OU=in-cse, O=om2m, L=tainan, C=taiwan" -ext san=ip:127.0.0.1,ip:[::1] -v
```
生成 csr, 這邊的 ***alias*** 要對應剛剛生成 keypair 的 ***alias***
```
keytool -certreq -alias incse -file /home/om2m/keystore/cert/incse.csr -keystore /home/om2m/keystore/keystoreincse
```

Create cert
```
keytool -gencert -infile /home/om2m/keystore/cert/incse.csr -outfile /home/om2m/keystore/cert/incse.crt -alias incse -validity 730 -keystore /home/om2m/keystore/keystoreincse
```

Import cert
```

```
:::info
詳細可以看[官方文檔](https://eclipse.dev/jetty/documentation/jetty-12/operations-guide/index.html#og-keystore)
:::


#### import cert
```
keytool -importcert -file /home/om2m/keystore/cert/incse.crt -keystore /home/om2m/keystore/keystoreincse  -trustcacerts -v
```

輸入 yes
![image](https://hackmd.io/_uploads/ByzNkJuu6.png)



