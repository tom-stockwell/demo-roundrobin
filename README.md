# Demo

- Deploy with `oc apply -k .`
    - For OCP3 just apply each individual file with something like `oc apply -f *.yml`
- See connections without cookies roundrobin between pods
```shell
$ host=$(oc get route hello-kubernetes -o template='{{.spec.host}}')
$ curl -sk https://$host | grep 'pod' -A1
      <th>pod:</th>
      <td>hello-kubernetes-68bb84b57-j4bx7</td>
$ curl -sk https://$host | grep 'pod' -A1
      <th>pod:</th>
      <td>hello-kubernetes-68bb84b57-c26ww</td>
$ curl -sk https://$host | grep 'pod' -A1
      <th>pod:</th>
      <td>hello-kubernetes-68bb84b57-89zck</td>
$ curl -sk https://$host | grep 'pod' -A1
      <th>pod:</th>
      <td>hello-kubernetes-68bb84b57-j4bx7</td>
```

- See connections with cookies continuously route to the same pod
```shell
$ host=$(oc get route hello-kubernetes -o template='{{.spec.host}}')
$ curl -sk -b cookie.txt -c cookie.txt https://$host | grep 'pod' -A1
      <th>pod:</th>
      <td>hello-kubernetes-68bb84b57-c26ww</td>
$ curl -sk -b cookie.txt -c cookie.txt https://$host | grep 'pod' -A1
      <th>pod:</th>
      <td>hello-kubernetes-68bb84b57-c26ww</td>
$ curl -sk -b cookie.txt -c cookie.txt https://$host | grep 'pod' -A1
      <th>pod:</th>
      <td>hello-kubernetes-68bb84b57-c26ww</td>
```
