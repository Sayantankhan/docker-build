# 🛡️ Redis Sentinel Cheat Sheet — home-server:26379

## 1. Quick Connectivity Check  
```bash
redis-cli -h home-server -p 26379 PING  
# → should reply: PONG
```

## 2. Sentinel Overview Commands

- **List monitored masters**  
  ```bash
  redis-cli -h home-server -p 26379 SENTINEL masters
  ```

- **Inspect a master**  
  ```bash
  redis-cli -h home-server -p 26379 SENTINEL master mymaster
  ```

- **List replicas**  
  ```bash
  redis-cli -h home-server -p 26379 SENTINEL replicas mymaster
  ```

- **Get current master address**  
  ```bash
  redis-cli -h home-server -p 26379 SENTINEL get-master-addr-by-name mymaster
  ```

- **Check quorum**  
  ```bash
  redis-cli -h home-server -p 26379 SENTINEL ckquorum mymaster
  ```

## 3. Management Commands

- **Force a manual failover**  
  ```bash
  redis-cli -h home-server -p 26379 SENTINEL failover mymaster
  ```

- **Reset Sentinel state**  
  ```bash
  redis-cli -h home-server -p 26379 SENTINEL reset mymaster
  ```

## 4. Runtime Reconfiguration

- **Add a new master**  
  ```bash
  redis-cli -h home-server -p 26379 SENTINEL monitor newmaster 10.0.0.5 6379 2
  redis-cli -h home-server -p 26379 SENTINEL set newmaster down-after-milliseconds 10000 quorum 2
  ```

- **Change global settings**  
  ```bash
  redis-cli -h home-server -p 26379 SENTINEL CONFIG GET *
  redis-cli -h home-server -p 26379 SENTINEL CONFIG SET resolve-hostnames yes
  ```

- **Force config rewrite (persist new state)**  
  ```bash
  redis-cli -h home-server -p 26379 SENTINEL CONFIG REWRITE
  ```

## 5. Monitoring & Events

- **Subscribe to Sentinel events**  
  ```bash
  redis-cli -h home-server -p 26379 PSUBSCRIBE "__sentinel__:*"
  ```

- **Get internal stats**  
  ```bash
  redis-cli -h home-server -p 26379 INFO sentinel
  ```

## ✅ Example Health Check Script

```bash
status=$(redis-cli -h home-server -p 26379 SENTINEL ckquorum mymaster)
master_count=$(redis-cli -h home-server -p 26379 SENTINEL masters | grep -c mymaster)
echo "Quorum: $status | Master count: $master_count"
```

## ℹ️ Notes

- Make sure Sentinel is configured with `resolve-hostnames yes`.
- Use at least **three Sentinels** in production for reliable failover.

## ✅ Summary of Commands

| Task                      | Command |
|---------------------------|---------|
| Connectivity check        | `PING` |
| Show masters              | `SENTINEL masters` |
| Inspect specific master   | `SENTINEL master mymaster` |
| List replicas             | `SENTINEL replicas mymaster` |
| Get current master addr   | `SENTINEL get-master-addr-by-name mymaster` |
| Check quorum              | `SENTINEL ckquorum mymaster` |
| Force failover            | `SENTINEL failover mymaster` |
| Reset state               | `SENTINEL reset mymaster` |
| Reconfigure master        | `SENTINEL monitor` / `SENTINEL set` |
| Modify config             | `SENTINEL CONFIG GET/SET` |
| Persist state             | `SENTINEL CONFIG REWRITE` |
| Subscribe to events       | `PSUBSCRIBE "__sentinel__:*"` |
| View internal info        | `INFO sentinel` |

