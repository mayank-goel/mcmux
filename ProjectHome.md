Mcmux stands for memcached multiplexer. Mcmux is basically a memcached proxy that multiplexes requests for multiple memcached servers. A typical deployment scenario for mcmux is illustrated as follows:


> PHP Application                                             |-------------|
> > ||                                                     |             |
> > ||                                                     |             |
> > VV                                                     | Memcached   |

> Zynga PHP-Pecl Memcache Extension  ------> Mcmux --------->  | Server      |
> > | Pool        |
> > |             |
> > |-------------|

Mcmux essentially maintains a connection pool to the set of backend memcached servers. The zynga php pecl memcache extension is responsible for determining which memcached server a particular request must be forwarded to. The request is then prefixed with the server name/ip, port and protocol before it is forwarded to mcmux. for e.g.

a request such as "get k1" will be transmitted to mcmux as

protocol:<memcached server name>:<port name> get k1

where protocol can either be binary(B) or ascii (A). The protocol parameter determines whether mcmux should talk binary or ascii with the backend memcached servers.

Mcmux will in turn extract the memcached server address and look for an existing connection in its connection pool. If none is found, then a new connection is created and stored in the pool once the transaction completes.
