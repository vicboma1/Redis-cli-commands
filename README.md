# Command-line for Redis Client | Windows cmd

## Index
  * [Connection](https://github.com/vicboma1/Redis-cli-commands/blob/master/README.md#connection)
  * [Strings](https://github.com/vicboma1/Redis-cli-commands/blob/master/README.md#strings)
  * [Key]()
  
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

##### BITOP operation destkey key [key ...] - Perform bitwise operations between strings | Time complexity: O(N)
```
127.0.0.1:6379> set key1 fooo
OK
127.0.0.1:6379> get key1
"fooo"
127.0.0.1:6379> set key2 buuu
OK
127.0.0.1:6379> get key2
"buuu"
127.0.0.1:6379> bitop AND dest key1 key2
(integer) 4
127.0.0.1:6379> get dest
"beee"
127.0.0.1:6379> get key1
"fooo"
127.0.0.1:6379> get key2
"buuu"
127.0.0.1:6379> bitop OR dest2 key1 key2
(integer) 4
127.0.0.1:6379> get dest2
"f\x7f\x7f\x7f"
127.0.0.1:6379> get keu1
(nil)
127.0.0.1:6379> get key1
"fooo"
127.0.0.1:6379> get key2
"buuu"
127.0.0.1:6379> bitop XOR dest3 key1 key2
(integer) 4
127.0.0.1:6379> get dest3
"\x04\x1a\x1a\x1a"
127.0.0.1:6379> bitop NOT dest4 dest3
(integer) 4
127.0.0.1:6379> get dest4
"\xfb\xe5\xe5\xe5"
127.0.0.1:6379>
```

##### BITPOS key bit [start] [end] - Find first bit set or clear in a string | Time complexity: O(N)
```
127.0.0.1:6379> set position "\x04\x1a\x1a\x1a"
OK
127.0.0.1:6379> get position
"\x04\x1a\x1a\x1a"
127.0.0.1:6379> bitpos position 0
(integer) 0
127.0.0.1:6379> bitpos position 1 0
(integer) 5
127.0.0.1:6379> bitpos position 1 2
(integer) 19
127.0.0.1:6379> get position
"\x04\x1a\x1a\x1a"
127.0.0.1:6379> bitpos position 0 10
(integer) -1
127.0.0.1:6379> bitpos position 0 6
(integer) -1
127.0.0.1:6379> bitpos position 1 5
(integer) -1
127.0.0.1:6379> bitpos position 1 4
(integer) -1
127.0.0.1:6379> bitpos position 1 4
(integer) -1
127.0.0.1:6379> bitpos position 1 2
(integer) 19
127.0.0.1:6379> bitpos position 1 3
(integer) 27
127.0.0.1:6379> bitpos position 1 4
(integer) -1
```

##### DECR key - Decrement the integer value of a key by one | Time complexity: O(1)
```
127.0.0.1:6379> set decrement 1986
OK
127.0.0.1:6379> get decrement
"1986"
127.0.0.1:6379> decr decrement
(integer) 1985
127.0.0.1:6379> get decrement
"1985"
```

##### DECRBY key decrement - Decrement the integer value of a key by the given number | Time complexity: O(1)
```
127.0.0.1:6379> set decrementBy 1986
OK
127.0.0.1:6379> decrby decrementBy 1000
(integer) 986
127.0.0.1:6379> DECRBY decrementBy 985
(integer) -985                     **** WHAT ??? ****
127.0.0.1:6379> get decrementby
"-985"                             **** WHAT ??? ****
127.0.0.1:6379> set key10 1000
OK
127.0.0.1:6379> DECR key10
(integer) 999
127.0.0.1:6379> DECR key10
(integer) 998
127.0.0.1:6379> DECRBY key10 10
(integer) 988
127.0.0.1:6379> DECRBY key10 100
(integer) 888
```

##### GET key -  Get the value of a key | Time complexity: O(1)
```
127.0.0.1:6379> set ptr1 1986
OK                  
127.0.0.1:6379> get ptr1
"1986"                          
```

##### GETBIT key offset - Returns the bit value at offset in the string value stored at key |  Time complexity: O(1)
```
127.0.0.1:6379> setbit key11 7 1
(integer) 0
127.0.0.1:6379> getbit key11 0
(integer) 0
127.0.0.1:6379> getbit key11 7
(integer) 1
127.0.0.1:6379> getbit key11 2
(integer) 0
127.0.0.1:6379> getbit key11 8
(integer) 0
```

##### GETRANGE key start end - Get a substring of the string stored at a key | Time complexity: O(N) & w/small string O(1)
```
127.0.0.1:6379> set token Hola mundo
(error) ERR syntax error
127.0.0.1:6379> set token "Hola Mundo"
OK
127.0.0.1:6379> get token
"Hola Mundo"
127.0.0.1:6379> GETRANGE token 0 2
"Hol"
127.0.0.1:6379> GETRANGE token 0 -1
"Hola Mundo"
127.0.0.1:6379> getrange token 5 -1
"Mundo"
127.0.0.1:6379> getrange token 0 -6
"Hola "
```

##### GETSET key value - Set the string value of a key and return its old value | Time complexity: O(1)
```
127.0.0.1:6379> set pass "123telacomootravez"
OK
127.0.0.1:6379> get pass
"123telacomootravez"
127.0.0.1:6379> getset pass "0987"
"123telacomootravez"
127.0.0.1:6379> get pass
"0987"
```

##### INCR key - Increment the integer value of a key by one | Time complexity: O(1)
```
127.0.0.1:6379> set increment 1986
OK
127.0.0.1:6379> get increment
"1986"
127.0.0.1:6379> incr increment
(integer) 1987
127.0.0.1:6379> get increment
"1987"
```

##### INCRBY key increment - Increment the integer value of a key by the given amount | Time complexity: O(1)
```
127.0.0.1:6379> set increment 1986
OK
127.0.0.1:6379> get increment
"1986"
127.0.0.1:6379> incrby increment 14
(integer) 2000
127.0.0.1:6379> get increment
"2000"
```

##### INCRBYFLOAT key increment - Increment the float value of a key by the given amount | Time complexity: O(1)
```
127.0.0.1:6379> set myFloat 10.66
OK
127.0.0.1:6379> get myFloat
"10.66"
127.0.0.1:6379> INCRBYFLOAT myFloat -5
"5.66"
127.0.0.1:6379> INCRBYFLOAT myFloat 1.0e2
"105.66"
```

##### MGET key [key ...] - Get the values of all the given keys | Time complexity: O(N)
```
127.0.0.1:6379> mget key1 key2 token noexiste myFloat
1) "fooo"
2) "buuu"
3) "Hola Mundo"
4) (nil)
5) "105.66"
```

##### MSET key value [key value ...] - Set multiple keys to multiple values | Time complexity: O(N)
```
127.0.0.1:6379> mset key1 0 key2 2 token 3 noexist 4 myFloat 5
OK
127.0.0.1:6379> mget key1 key2 token noexist noexiste myFloat
1) "0"
2) "2"
3) "3"
4) "4"
5) (nil)
6) "5"
```

##### MSETNX key value [key value ...] - Set multiple keys to multiple values, only if none of the keys exist | Time complexity: O(N)
```
127.0.0.1:6379> msetnx key1 hola key2 mundo key3 !
(integer) 0
127.0.0.1:6379> mget key1 key2 key3
1) "0"
2) "buuu"
3) (nil)
127.0.0.1:6379> msetnx key1 hola key2 mundo
(integer) 0
127.0.0.1:6379> msetnx key1 "hola" key2 "mundo"
(integer) 0
127.0.0.1:6379> get key1
"0"
127.0.0.1:6379> get key2
"buuu"
127.0.0.1:6379> msetnx p1 0
(integer) 1
127.0.0.1:6379> msetnx p2 2
(integer) 1
127.0.0.1:6379> msetnx newVar1 hola newVar2 Mundo
(integer) 1
127.0.0.1:6379> mget newVar1 newVar2
1) "hola"
2) "Mundo"
```

##### PSETEX key milliseconds value - Set the value and expiration in milliseconds of a key | Time complexity: O(1)
```
127.0.0.1:6379> psetex p 5000 password
OK
127.0.0.1:6379> get p
"password"
127.0.0.1:6379> get p
"password"
127.0.0.1:6379> get p
(nil)
127.0.0.1:6379> get p
(nil)
```

##### SET key value [EX seconds|PX milliseconds] [NX|XX] [KEEPTTL] - Set the string value of a key | Time complexity: O(1)
```
127.0.0.1:6379> set str vicboma1
OK
127.0.0.1:6379> get str
"vicboma1"
127.0.0.1:6379> set str vicboma1 EX 5
OK
127.0.0.1:6379> get str
"vicboma1"
127.0.0.1:6379> get str
"vicboma1"
127.0.0.1:6379> get str
(nil)
127.0.0.1:6379> set str vicboma1 PX 1000
OK
127.0.0.1:6379> get str
(nil)
127.0.0.1:6379> set str vicboma PX 2000
OK
127.0.0.1:6379> get str
"vicboma"
127.0.0.1:6379> get str
(nil)
```

##### SETBIT key offset value - Sets or clears the bit at offset in the string value stored at key | Time complexity: O(1)
```
127.0.0.1:6379> set key10 10
OK
127.0.0.1:6379> get key10
"10"
127.0.0.1:6379> setbit key10 10 1
(integer) 1
127.0.0.1:6379> setbit key10 10 0
(integer) 1
127.0.0.1:6379> setbit key10 5
(error) ERR bit is not an integer or out of range
127.0.0.1:6379> get key10
"1\x10"
```

##### SETEX key seconds value - Set the value and expiration of a key | Time complexity: O(1)
```
127.0.0.1:6379> setex str 4 vicbo
OK
127.0.0.1:6379> get str
"vicbo"
127.0.0.1:6379> get str
(nil)
```

##### SETNX key value - Set the value of a key, only if the key does not exist | Time complexity: O(1)
```
127.0.0.1:6379> setnx hola hola
(integer) 1
127.0.0.1:6379> setnx hola hello
(integer) 0
127.0.0.1:6379> get hola
"hola"
```

##### SETRANGE key offset value - Overwrite part of a string at key starting at the specified offset | Time complexity: O(1)
```
127.0.0.1:6379> set key10 vicboma1
OK
127.0.0.1:6379> setrange key10 8 !
(integer) 9
127.0.0.1:6379> get key10
"vicboma1!"
127.0.0.1:6379> setrange key10 10 ???
(integer) 13
127.0.0.1:6379> get key10
"vicboma1!\x00???"
```

##### STRLEN key - Get the length of the value stored in a key | Time complexity: O(1)
```
redis> SET str "Hello world"
"OK"
redis> STRLEN str
(integer) 11
redis> STRLEN newStr
(integer) 0
```




#### Key

##### DEL key [key ...] - Delete a key
```
```

##### DUMP key - Return a serialized version of the value stored at the specified key.
```
```

##### EXISTS key [key ...] - Determine if a key exists
```
```

##### EXPIRE key seconds - Set a key's time to live in seconds
```
```

##### EXPIREAT key timestamp - Set the expiration for a key as a UNIX timestamp
```
```

##### KEYS pattern - Find all keys matching the given pattern
```
```

##### MIGRATE host port key|"" destination-db timeout [COPY] [REPLACE] [AUTH password] [KEYS key [key ...]] - Atomically transfer a key from a Redis instance to another one.
```
```

##### MOVE key db - Move a key to another database
```
```

##### OBJECT subcommand [arguments [arguments ...]] - Inspect the internals of Redis objects
```
```

##### PERSIST key - Remove the expiration from a key
```
```

##### PEXPIRE key milliseconds - Set a key's time to live in milliseconds
```
```

##### PEXPIREAT key milliseconds-timestamp - Set the expiration for a key as a UNIX timestamp specified in milliseconds
```
```

##### PTTL key - Get the time to live for a key in milliseconds
```
```

##### RANDOMKEY - Return a random key from the keyspace
```
```

##### RENAME key newkey - Rename a key
```
```

##### RENAMENX key newkey - Rename a key, only if the new key does not exist
```
```

##### RESTORE key ttl serialized-value [REPLACE] [ABSTTL] [IDLETIME seconds] [FREQ frequency] - Create a key using the provided serialized value, previously obtained using DUMP.
```
```

##### SORT key [BY pattern]  - [LIMIT offset count] [GET pattern [GET pattern ...]] [ASC|DESC] [ALPHA] [STORE destination] Sort the elements in a list, set or sorted set
```
```

##### TOUCH key [key ...] - Alters the last access time of a key(s). Returns the number of existing keys specified.
```
```

##### TTL key - Get the time to live for a key
```
```

##### TYPE key - Determine the type stored at key
```
```
##### UNLINK key [key ...] - Delete a key asynchronously in another thread. Otherwise it is just as DEL, but non blocking.
```
```

##### ##### WAIT numreplicas timeout -Wait for the synchronous replication of all the write commands sent in the context of the current connection
```
```

##### SCAN cursor [MATCH pattern] [COUNT count] [TYPE type] - Incrementally iterate the keys space
```
```




##### STRLEN key - Get the length of the value stored in a key
```
```


