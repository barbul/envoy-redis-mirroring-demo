## Envoy as a write mirroring proxy for redis - demo

``` bash
./docker-compose up
```

#### Testing key mirroring

- We are setting a local-only key and one that will be mirrored over to the remote redis according to the filter defined in envoy/envoy.yaml

``` bash
# connecting to the proxy
% redis-cli -a password
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
127.0.0.1:6379> SET localkey 123456
OK
127.0.0.1:6379> SET mirror_key 67890
OK
127.0.0.1:6379> GET localkey
"123456"
127.0.0.1:6379> GET mirror_key
"67890"
127.0.0.1:6379>
```

``` bash
# connecting to local redis (redis-a) - both keys are present
% redis-cli -p 6479 -a password
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
127.0.0.1:6479> GET localkey
"123456"
127.0.0.1:6479> GET mirror_key
"67890"
127.0.0.1:6479>
```

``` bash
# connecting to remote redis (redis-b) - only mirror_key is present
% redis-cli -p 6579 -a password
Warning: Using a password with '-a' or '-u' option on the command line interface may not be safe.
127.0.0.1:6579> GET localkey
(nil)
127.0.0.1:6579> GET mirror_key
"67890"
127.0.0.1:6579>
```

You may visit http://localhost:8001/ for Envoy stats and other goodies
