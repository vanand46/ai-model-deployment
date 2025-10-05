# Apache Spark Lab Setup using containerd (on WSL or Linux)

This documents shows how to set up and run **Apache Spark + PySpark** using **containerd**.  
It’s lightweight, open, and perfect if you’re using **WSL2 (Ubuntu)** or a **native Linux** environment.

## Why containerd?

`containerd` is the container runtime used under the hood by Docker and Kubernetes.  
It’s efficient, daemon-based, and can run containers directly without the overhead of Docker Desktop.

We’ll use `nerdctl`, a Docker-compatible CLI for containerd, to run Spark containers easily.

## 1. Prerequisites

### System Requirements
- Windows 10/11 with **WSL2 + Ubuntu LTS Version** / any modern **Linux Distribution**
- Admin access (sudo privileges)
- Internet connection

## 2. Install containerd + nerdctl in WSL or Linux

```bash
sudo apt update
sudo apt install containerd -y

# Enable and Start containerd
sudo systemctl enable containerd
sudo systemctl start containerd

# Download nerdctl (Docker-compatible CLI for containerd)
curl -L https://github.com/containerd/nerdctl/releases/download/v2.0.0/nerdctl-2.0.0-linux-amd64.tar.gz -o nerdctl.tgz
sudo tar -C /usr/local/bin -xzf nerdctl.tgz
rm nerdctl.tgz

# Check version
containerd --version
nerdctl --version
```

## 3. Enable user access
This is to allow non-root user to run containerd commands

```bash
sudo usermod -aG sudo $USER
sudo usermod -aG systemd-journal $USER
```
Then log out and back in, or restart WSL session:

```bash
exec su -l $USER
```

## 4. Containerd networking setup

### 1. Generate a default `config.toml`
```bash
sudo mkdir -p /etc/containerd
sudo containerd config default | sudo tee /etc/containerd/config.toml
```

### 2. Edit the CNI plugin section
Open the file
```bash
sudo nano /etc/containerd/config.toml
```
Then find this section (around halfway down):
```toml
[plugins."io.containerd.grpc.v1.cri".cni]
  bin_dir = "/opt/cni/bin"
  conf_dir = "/etc/cni/net.d"
```
Make sure both paths are set like that.
If those lines are commented out or missing, add or uncomment them.

### 3. Install the CNI plugins
```bash
sudo mkdir -p /opt/cni/bin
cd /opt/cni/bin
sudo curl -L -o cni-plugins.tgz https://github.com/containernetworking/plugins/releases/download/v1.5.0/cni-plugins-linux-amd64-v1.5.0.tgz
sudo tar xvf cni-plugins.tgz
```
Verify
```bash
ls /opt/cni/bin/bridge
```

### 4. Install iptables
```bash
sudo apt update
sudo apt install -y iptables
```
Verify it
```bash
which iptables
```

### 5. Restart containerd
```bash
sudo systemctl restart containerd
```

### 6. Test it
```bash
sudo nerdctl run --rm -it alpine sh
```
Inside:
```bash
ping -c 2 8.8.8.8
```

## 5. Run the PySpark Notebook Container
Pull and start the official Jupyter PySpark Notbook container using `containerd`

```bash
sudo nerdctl run -it --rm \
 -p 8881:8888 \
 -v $HOME/storage:/home/jovyan/work \
 --name sparkbook \
 jupyter/pyspark-notebook:latest \
start.sh jupyter lab --LabApp.token='' 
```
#### Command Breakdown

| Flag                        | Meaning                                        |
| --------------------------- | ---------------------------------------------- |
| `-it`                       | Interactive terminal                           |
| `--rm`                      | Auto-remove container when stopped             |
| `-p 8881:8888`              | Map container port 8888 → local 8881           |
| `-v $HOME/storage:/home/jovyan/work` | Mount storage directory to container workspace |
| `--name sparkbook`          | Name of the container                          |
| `--LabApp.token=''`         | Disable Jupyter auth token (for dev use only)  |

## 6. Access JupyterLab

Open your browser and go to:

http://localhost:8881

## 7. Test the Spark Setup
Inside JupyterLab, create a Python 3 notebook and run:
```py
from pyspark.sql import SparkSession

spark = SparkSession.builder.appName("ContainerdSparkLab").getOrCreate()

data = [(1, "Alice"), (2, "Bob"), (3, "Charlie")]
df = spark.createDataFrame(data, ["id", "name"])
df.show()
```
Result:
```pgsql
+---+-------+
| id|  name |
+---+-------+
|  1| Alice |
|  2|   Bob |
|  3|Charlie|
+---+-------+
```

## 7. Managing the Container

Stop the container
```bash
sudo nerdctl stop sparkbook
```
Run in background
```bash
sudo nerdctl run -d \
  -p 8881:8888 \
  -v $PWD:/home/jovyan/work \
  --name sparkbook \
  jupyter/pyspark-notebook:latest \
  start.sh jupyter lab --LabApp.token=''
```
Remove Container
```bash
sudo nerdctl rm -f sparkbook
```