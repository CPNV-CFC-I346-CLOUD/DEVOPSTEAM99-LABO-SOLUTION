# Accesses

## Prerequisits

You need two private keys.

* One to connect the ssh server (DMZ) transmitted in your team channel
* A second key that you obtained yourself via the CLI (key pair). This is the same key you used to launch the instances. The key is used to authenticate on your Linux instance and to calculate the password for the Windows instance.

## Try to connect the ssh server

* Open a new ssh terminal, customize this command and execute it

```
ssh devopsteam<xx>@<ssh-srv-public-ip> ^
  -i KEY-I346-DMZ-DEVOPSTEAM<XX>.pem
```

```
//OUTPUT
Linux <ssh-srv-private-ip>-cloud-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.123-1 (2025-01-02) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Tue Mar 18 17:55:49 2025 from 188.155.68.9
```

* Close the session

```
exit
```

## Set and test you ssh tunnel

* Based on the previous command, add the configuration to set up two ssh tunnels.

* One for the Linux instance.
* The other for the Windows instance.

```
-L <localPortToForward>:<Linux-private-ip>:<remotePort>
```

* Possible final command integrating both tunnels

```
ssh admin@52.59.181.213 ^
    -i <key-name>.pem ^
    -L 23:10.0.99.10:22 ^
    -L 3399:10.0.99.11:3389
```

* To get more information, you can add an optionnal "verbose" parameter "-v"

## Connection to the private Linux instance

* When the first session in established, open a second ssh teminal

```
ssh admin@localhost ^
  -p 23 (depending on your configuration) ^
  -i <key-name>.pem
```

## Connection to the private Windows instance

* Get the Windows password

* [AWS CLI - Get Windows Password](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ec2/get-password-data.html)

```
