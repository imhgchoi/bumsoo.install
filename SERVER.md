# SERVER MANAGEMENT
This is the management guide for server installation.

## Remote Server control

### 1. SCP call

```bash

# Upload your local file to server
$ scp -P 22 <LOCAL_DIR>/file [:username]@[:server_ip]:<SERVER_DIR>

# Download the server file to your local
$ scp -P 22 [:username]@[:server_ip]:<SERVER_DIR>/file <LOCAL_DIR>

# Upload your local directory to server
$ scp -P 22 -r <LOCAL_DIR>/file [:username]@[:server_ip]:<SERVER_DIR>

# Download the server file to your local
$ scp -P 22 -r [:username]@[:server_ip]:<SERVER_DIR>/file <LOCAL_DIR>

```

### 2. Save sessions by name
```bash
$ sudo vi ~/.bashrc

# Press [Shift] + [G] and enter the function on the bottom.

function server_func() {
    echo -n "[Enter the name of server]: "
    read server_name
                
    # echo "Logging in to server $server_name ..."
    if [ $server_name == [:servername] ]; then
        ssh [:usr]@[:ip].[:ip].[:ip].[:ip]
    fi
}
alias dmis_remote=server_func
```

Now, Enjoy!
