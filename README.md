
## dry-run mode

```bash
ansible-playbook -i inventory.ini update_fs_filemax.yml --check --diff
```

```bash
ansible-playbook -i inventory.ini update_fs_filemax.yml -vvv`
```

```
Start
  |
  v
[Load VM Inventory]
  |
  v
[Discover Linux Distribution]
  |
  +--------------------------+
  |                          |
  v                          v
[Check if RabbitMQ is Running]
  |                          |
  v                          v
[If RabbitMQ Active]   [If RabbitMQ Not Active]
  |                          |
  v                          v
[Group by OS Family]     [Skip Updates for This VM]
  |
  +------------------------------------------+
  |                   |                      |
  v                   v                      v
[If Debian/Ubuntu] [If CentOS/RedHat] [If SUSE]
  |                   |                      |
  v                   v                      v
[Update fs.file-max] [Update fs.file-max] [Update fs.file-max]
  |                   |                      |
  v                   v                      v
[Persist Changes in /etc/sysctl.conf]
|
v
[If Other OS Family: Alert and Skip]
|
v
[Validate Parameter Update]
|
v
[Log Results]
|
v
End
```

### Optimize Connection Strategy

```ansible.cfg
[defaults]
forks = 100 
timeout = 30

[ssh_connection]
pipelining = true
```

###  --forks to increase parallelism and --limit to target subsets of hosts


```bash
ansible-playbook -i inventory.ini update_fs_filemax.yml  --forks 100
```