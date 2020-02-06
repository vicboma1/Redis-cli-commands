# Command-line for Redis Client | Windows cmd

## Index
  * [Connection](https://github.com/vicboma1/Redis-cli-commands/blob/master/README.md#connection)
  * [Strings](https://github.com/vicboma1/Redis-cli-commands/blob/master/README.md#strings)
  
### Connection

##### ECHO message - Echo the given string
```
127.0.0.1:6379[1]> echo vicboma1
"vicboma1"
```

##### PING [message] -Ping the server
```
127.0.0.1:6379[1]> ping
PONG
127.0.0.1:6379[1]> ping vicboma1
"vicboma1"
```

##### SELECT index - Change the selected database for the current connection
```
127.0.0.1:6379[1]> client list
id=2 addr=127.0.0.1:65353 fd=7 name= age=793 idle=0 flags=N 
db=1 
sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=32768 obl=0 oll=0 omem=18446744073709537584 events=r cmd=client
127.0.0.1:6379[1]> select 2
OK
127.0.0.1:6379[2]> client list
id=2 addr=127.0.0.1:65353 fd=7 name= age=801 idle=0 flags=N
db=2 
sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=32768 obl=0 oll=0 omem=18446744073709537584 events=r cmd=client
```

##### SWAPDB index1 index2 - Swaps two Redis databases
```
127.0.0.1:6379[2]> swapdb 2 1
(error) ERR unknown command 'swapdb'
```

##### AUTH password - Authenticate to the server
```
127.0.0.1:6379> auth
(error) ERR wrong number of arguments for 'auth' command
127.0.0.1:6379> config set requirepass redispass
OK
127.0.0.1:6379> auth redispass
OK
127.0.0.1:6379>
```

##### QUIT - Close the connection
```
127.0.0.1:6379>quit -
```


### STRINGS

##### APPEND key value - Append a value to a key |  Time complexity: O(1)
```
127.0.0.1:6379> exists var
(integer) 0
127.0.0.1:6379> append var 1986
(integer) 4
127.0.0.1:6379> append var -02-04
(integer) 10
127.0.0.1:6379> get var
"1986-02-04"
```

##### BITCOUNT key [start end] - Count set bits in a string | Time complexity: O(N)
```
27.0.0.1:6379> set var3 "vicboma1"
OK
127.0.0.1:6379> get var3
"vicboma1"
127.0.0.1:6379> set var vicboma1
OK
127.0.0.1:6379> get var3
"vicboma1"
127.0.0.1:6379> bitcount var3
(integer) 33
127.0.0.1:6379> bitcount var3 0 0
(integer) 5
127.0.0.1:6379> bitcount var3 1 1
(integer) 4
127.0.0.1:6379> bitcount var3 0 8
(integer) 33
```

##### BITFIELD key [GET type offset] [SET type offset value] [INCRBY type offset increment] [OVERFLOW WRAP|SAT|FAIL] Perform arbitrary bitfield integer operations on strings | Time complexity: O(1)
```
127.0.0.1:6379> set iterator 0123456789
OK
127.0.0.1:6379> bitfield iterator incrby i5 100 1 get u4 0
1) (integer) 1
2) (integer) 3
127.0.0.1:6379> bitfield iterator incrby i1 100 1 get u1 0
1) (integer) -1
2) (integer) 0
127.0.0.1:6379> bitfield iterator incrby u2 100 1 OVERFLOW SAT incrby u2 102 1
1) (integer) 3
2) (integer) 1
127.0.0.1:6379> bitfield iterator incrby u2 100 1 OVERFLOW SAT incrby u2 102 1
1) (integer) 0
2) (integer) 2
127.0.0.1:6379> bitfield iterator incrby u2 100 1 OVERFLOW SAT incrby u2 102 1
1) (integer) 1
2) (integer) 3
127.0.0.1:6379> bitfield iterator incrby u2 100 1 OVERFLOW SAT incrby u2 102 1
1) (integer) 2
2) (integer) 3
127.0.0.1:6379> bitfield iterator incrby u2 100 1 OVERFLOW SAT incrby u2 102 1
1) (integer) 3
2) (integer) 3
127.0.0.1:6379> bitfield iterator incrby u2 100 1 OVERFLOW SAT incrby u2 102 1
1) (integer) 0
2) (integer) 3
127.0.0.1:6379> get iterator
"0123456789\x00\x00\x03\x80"
```

##### BITOP operation destkey key [key ...] - Perform bitwise operations between strings
```
```
##### BITPOS key bit [start] [end] - Find first bit set or clear in a string
```
```
##### DECR key - Decrement the integer value of a key by one
```
```
##### DECRBY key decrement - Decrement the integer value of a key by the given number
```
```
##### GET key -  Get the value of a key
```
```
##### GETBIT key offset - Returns the bit value at offset in the string value stored at key
```
```
##### GETRANGE key start end - Get a substring of the string stored at a key
```
```
##### GETSET key value - Set the string value of a key and return its old value
```
```
##### INCR key - Increment the integer value of a key by one
```
```
##### INCRBY key increment - Increment the integer value of a key by the given amount
```
```
##### INCRBYFLOAT key increment - Increment the float value of a key by the given amount
```
```
##### MGET key [key ...] - Get the values of all the given keys
```
```
##### MSET key value [key value ...] - Set multiple keys to multiple values
```
```
##### MSETNX key value [key value ...] - Set multiple keys to multiple values, only if none of the keys exist
```
```
##### PSETEX key milliseconds value - Set the value and expiration in milliseconds of a key
```
```
##### SET key value [EX seconds|PX milliseconds] [NX|XX] [KEEPTTL] - Set the string value of a key
```
```
##### SETBIT key offset value - Sets or clears the bit at offset in the string value stored at key
```
```
##### SETEX key seconds value - Set the value and expiration of a key
```
```
##### SETNX key value - Set the value of a key, only if the key does not exist
```
```
##### SETRANGE key offset value - Overwrite part of a string at key starting at the specified offset
```
```
##### STRLEN key - Get the length of the value stored in a key
```
```


