# Monk & Memcached

This repository contains Monk.io template to deploy Memcached system either locally or on cloud of your choice (AWS, GCP, Azure, Digital Ocean).


## Start

Set up Monk - https://docs.monk.io/docs/monk-in-10/

Start `monkd` and login.

```bash
monk login --email=<email> --password=<password>
```

## Clone Monk Memcached repository

In order to load templates and change configuration simply use below commands: 
```bash
git clone https://github.com/monk-io/monk-memcached

# and change directory to the monk-memcached template folder
cd monk-memcached

```

## Configuration

You can add/remove configuration of the template.

The current variables can be found in `memcached/stack/variables` section

```yaml
  variables:
    memcached-image-tag: "1.6.17-debian-11-r24"
    memcached-cache-size: 128
    memcached-max-connections: 2000
    memcached-threads: 4
    memcached-max-item-size: 8388608
    memcached-username: "my_user"
    memcached-password: "my_password"
    memcached-extra-flags: ""
```


##  Template variables

| Variable | Description | Type | Example |
|----------|-------------|------|---------|
| **memcached-image-tag** | Memcached image version. | string | latest |
| **memcached-cache-size** | Cache size in MB. | int | 128 |
| **memcached-max-connections** |  Maximum int of concurrent connections . | int | 2000 |
| **memcached-threads** | Amount of threads for which to process requests for  | int | 4 |
| **memcached-max-item-size** | Max item size . | int | 8388608 |
| **memcached-username** | Memcached admin user. | string | my_user |
| **memcached-password** | Memcached admin user password. | string | my_password |
| **memcached-extra-flags** | Passing extra command-line flags. | string | "" |



## Local Deployment

First clone the repository simply run below command after launching `monkd`:
:

```bash
➜  monk load MANIFEST

✨ Loaded:
 ├─🔩 Runnables:
 │  └─🧩 memcached/memcached
 ├─🔗 Process groups:
 │  └─🧩 memcached/stack
 └─⚙️ Entity instances:
    └─🧩 memcached/memcached/metadata
✔ All templates loaded successfully
➜  monk list -l memcached

✔ Got the list
Type      Template             Repository  Version  Tags
runnable  memcached/memcached  local       -        database, SQL, open-source
group     memcached/stack      local       -        -


➜  monk run  memcached/stack

✔ Started local/memcached/stack

```

This will start the entire memcached/stack.

## Cloud Deployment

To deploy the above system to your cloud provider, create a new Monk cluster and provision your instances.

```bash
➜  monk cluster new
? New cluster name memcached
✔ Cluster created
Your cluster has been created successfully.

➜  monk cluster provider add -p gcp -f <path/to/your-key.json>
✔ Provider added successfully

➜  monk cluster grow -p gcp
? Cloud provider gcp
? Name of the new instance memcached-instance
? Tags (split by whitespace) memcached
? Region europe-central2
? Zone europe-central2-a
? Instance type e2-medium
? Number of instances (or press ENTER to use default = 1) 3
? Default disk type for gcp is HDD (pd-standard). Would you like to change it? No
? Disk size (or press ENTER to use default = 250 GBs) 50
✔ Start creation of new instance(s) on gcp... DONE
✔ Creating node: my-instance-1 DONE
✔ Initializing node: my-instance-1 DONE
✔ Creating node: my-instance-2 DONE
✔ Initializing node: my-instance-2 DONE
✔ Creating node: my-instance-3 DONE
✔ Initializing node: my-instance-3 DONE
✔ Connecting: my-instance-1 DONE
✔ Syncing peer: my-instance-1 DONE
✔ Connecting: my-instance-2 DONE
✔ Connecting: my-instance-3 DONE
✔ Syncing peer: my-instance-2 DONE
✔ Syncing peer: my-instance-3 DONE
✔ Syncing nodes DONE
✔ Cluster grown successfully
```

Once cluster is ready execute the same command as for local and select your cluster (the option will appear automatically).


```bash
➜  monk load MANIFEST

✨ Loaded:
 ├─🔩 Runnables:
 │  └─🧩 memcached/memcached
 ├─🔗 Process groups:
 │  └─🧩 memcached/stack
 └─⚙️ Entity instances:
    └─🧩 memcached/memcached/metadata
✔ All templates loaded successfully
➜  monk list -l memcached

✔ Got the list
Type      Template             Repository  Version  Tags
runnable  memcached/memcached  local       -        database, SQL, open-source
group     memcached/stack      local       -        -


➜  monk run  memcached/stack

✔ Started local/memcached/stack

```

## Logs & Shell

```bash
# show Memcached logs
➜  monk logs -l 1000 -f memcached/memcached

# access shell in the container running Memcached
➜  monk shell memcached/memcached

```

## Stop, remove and clean up workloads and templates

```bash
➜ monk purge -x memcached/stack memcached/memcached

✔ memcached/stack purged
✔ memcached/memcached purged
```