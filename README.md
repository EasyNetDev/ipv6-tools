## ipv6-tools
 One integrated executable to provide some simple tools for working with IPv6 addresses.

## Overview
Becasue DASH/Shell in Linux can't work properly with IPv6 and for some ISP you have to create a SIT6 tunnels that needs to have a base IPv6 computed from a prefix + IPv4 of the main interface written in HEXA, this tool has some commands to help you to check and calculate different IPv6.

## Usage
```
ipv6-tools command <IPv6[/prefix]> [IPv6]
```

## Commands supported
```
check        - Check if the given string IPv6/prefix is a valid IPv6. Just first IPv6 is mandatory.
               Prefix is optional, but recomanded to be checked.
compress     - Compress IPv6 by remove longest consecutive 0 blocks. Just first IPv6 is mandatory.
decompress   - Decompress IPv6 by expanding with 0 blocks. Just first IPv6 is mandatory.
lzc          - Remove leading 0s of each sub-block. Just first IPv6 is mandatory.
               Is an alias to decompress.
fna          - Find first address of the network. IPv6/prefix is mandatory.
lna          - Find last address of the network. IPv6/prefix is mandatory.
isin         - Check if the given IPv6 is in subnet IPv6/prefix. Boths arguments are mandatory.
```
## Exit status
Commads `check` and `isin` are returning an exit status of 0 if the IPv6 has a good format or the second IPv6 argument is part of the subnet.
Exit status 1 means IPv6 format is wrong or second IPv6 argument is not part of the subnet.
Exit status 2 for `isin` means that the second IPv6 argument has a wrong format.

The rest of the commands return 0 if is succesful, 1 is the IPv6 has a wrong format, or 3 if prefix of the first IPv6 argument is incorrect.

## Output
Except commands `check` and `isin` all other commands are returning to console the requested IPv6.

## Examples
### Check
```
$ ./ipv6-tools check fc00:0000:0000:0000:0000:0000:0000:0000
$ echo $?
0

$ ./ipv6-tools check fc00:0000:0000:0000:0000:0000:0000:000g
$ echo $?
1
```

### Compress
```
$ ./ipv6-tools compress fc00:0000:0000:0000:0000:0000:0000:0000
fc00::

$ ./ipv6-tools compress fc00:0000:0000:0000:0000:0000:0000:ffff
fc00::ffff

$ ./ipv6-tools compress 0000:0000:0000:0000:0000:0000:0000:0000
::
```

### Decompress / lzc
```
$ ./ipv6-tools decompress fc00::
fc00:0:0:0:0:0:0:0

$ ./ipv6-tools decompress fc00::ffff
fc00:0:0:0:0:0:0:ffff

$ ./ipv6-tools decompress ::
0:0:0:0:0:0:0:0
```

### Find first address of the network
```
$ ./ipv6-tools fna fc00:ffff:ffff:ffff:ffff:ffff:ffff:ffff/64
fc00:ffff:ffff:ffff::

$ ./ipv6-tools fna fc00:ffff:ffff:ffff:ffff:ffff:ffff:ffff/48
fc00:ffff:ffff::

$ ./ipv6-tools fna fc00:ffff:ffff:ffff:ffff:ffff:ffff:ffff/112
fc00:ffff:ffff:ffff:ffff:ffff:ffff:0
```

### Find last address of the network
````
$ ./ipv6-tools lna fc00:ffff:ffff:ffff::/64
fc00:ffff:ffff:ffff:ffff:ffff:ffff:ffff
```

### Check if a given IPv6 is part of a subnet
```
$ ./ipv6-tools isin fc00:ffff:ffff:ffff::/64 fc00:ffff:ffff:ffff:1234:1234:1234:1234
echo $?
0

$ ./ipv6-tools isin fc00:ffff:ffff:ffff::/64 fc00:ffff:ffff:fffe:1234:1234:1234:1234
echo $?
1

$ ./ipv6-tools isin fc00:ffff:ffff:ffff::/64 fc00:ffff:feff:ffff:1234:1234:1234:1234
echo $?
1
```
