# A ConfigMap is a way to inject a configuration file into your container to its default location dynamically.
```
kubectl create configmap nginx-config --from-file=reverseproxy.conf
```

### Here is how to check and make sure your config was injected.
```
[user@phatbox configmaps (⎈ |Epick8s:default)]$ kubectl exec -ti helloworld-nginx -c nginx -- /bin/bash
root@helloworld-nginx:/# cd /etc/nginx/conf.d/
root@helloworld-nginx:/etc/nginx/conf.d# ls -al
total 12
drwxrwxrwx 3 root root 4096 Nov 29 20:29 .
drwxr-xr-x 3 root root 4096 Apr  6  2017 ..
drwxr-xr-x 2 root root 4096 Nov 29 20:29 ..2018_11_29_20_29_35.840924041
lrwxrwxrwx 1 root root   31 Nov 29 20:29 ..data -> ..2018_11_29_20_29_35.840924041
lrwxrwxrwx 1 root root   24 Nov 29 20:29 reverseproxy.conf -> ..data/reverseproxy.conf
root@helloworld-nginx:/etc/nginx/conf.d# cat reverseproxy.conf
server {
    listen       80;
    server_name  localhost;

    location / {
        proxy_bind 127.0.0.1;
        proxy_pass http://127.0.0.1:3000;
    }

    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}
root@helloworld-nginx:/etc/nginx/conf.d# 
```

