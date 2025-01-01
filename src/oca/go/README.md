# Quick Start Guide
## Build

https://gist.github.com/mackmoe/6f8abafeeea71858d4c0385af8c6c0d0

docker build . -t opennebula
docker run -d -p 9869:9869 -p 2633:2633 -p 2474:2474 -p 2616:2616 opennebula /bin/bash -c "one start; sunstone-server start; oneflow-server start; onegate-server start; fireedge-server start; sleep infinity"
docker exec -it e71d5e98f9ef /bin/bash

https://github.com/trfore/docker-centos9-systemd/tree/main

tail -f /var/log/one/oned.log


```
cd src/goca
go build
```

## Unit Tests

For unit tests you need dummy monitoring driver, dummy VM driver and user ``oneadmin`` with password ``opennebula``.
You can setup it by running:
```
echo "oneadmin:opennebula" > $HOME/.one/one_auth
echo 'IM_MAD = [ NAME="dummy", SUNSTONE_NAME="Dummy", EXECUTABLE="one_im_sh", ARGUMENTS="-r 3 -t 15 -w 90 dummy"]' >> /etc/one/monitord.conf
echo 'VM_MAD = [ NAME="dummy", SUNSTONE_NAME="Testing", EXECUTABLE="one_vmm_dummy",TYPE="xml" ]' >> /etc/one/oned.conf
one restart
```

Download go test moodule:
```
cd src/goca
go get gopkg.in/check.v1
```

Make sure opennebula is running. Exuecute tests:
```
cd src/goca
go test
```

## Examples

```
cd src/goca
go run ../../share/examples/example_name.go
```
