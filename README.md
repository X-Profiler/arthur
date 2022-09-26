
Arthur uses Fork() to reduce the pausing lag, and lz4 to reduce the corefile size.

Capture a corefile by arthur,

```
arthur -p <pid>
```
file will save in acore.pid.

Acore is the new corefile format used in Arthur, 

Convert acore to corefile, 
```
arthur -c <acore.pid>
```
GNU corefile will save in core.pid.

### arthur vs gcore

based on 1.7GB node process,

| - | arthur | gcore |
| --- | --- | --- |
| process paused time | 30ms | 1.6s |
| realtime cost | 0.641s | 1.639s |
| corefile size | 16MB | 1.7GB |


arthur log,
```
arthur[50752] I: Process 50519 paused 29.138 ms.
arthur[50752] I: Compressed 1719906691 bytes into 15628790 bytes ==> 0.91%

real    0m0.641s
user    0m0.277s
sys     0m0.316s
```

gcore log,
```
Saved corefile core.50519

real    0m1.639s
user    0m0.267s
sys     0m1.371s
```

file size compare,
```
[zl131478@node-build011164002207.na620 /home/zl131478/zlei/alibaba/arthur]
$ls -al core.50519 acore.50519
-rw-r--r-- 1 zl131478 users   15712205 Mar 18 20:08 acore.50519
-rw-r--r-- 1 zl131478 users 1699545344 Mar 18 20:08 core.50519
```

### Advanced Usage

Fork coredump mode, (default)
```
arthur -p <pid> -0
```

Gcore mode,
```
arthur -p <pid> -1
```

Kernel coredump mode,
```
arthur -p <pid> -2
```

Monitor mode,

```
arthur -p <pid> -3
```

only support x86_64 Linux.