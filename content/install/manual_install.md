+++
title = "Manual Install"
description = "Installing SCOT Step by Step"
weight = 3
+++

## Manual Mode

For those with trust issues, we get you.  Here's an explanation of the actions being taken by the `install.sh`.  You can perform each step below and achieve the successful deployment of SCOT.

## Create Scot4 User

Deployment of SCOT requires the existence of a scot4 user.  

```
sudo useradd -m -s /bin/bash -c "SCOT4 User" scot4
```


### Install K3s (as root)

K3s is the Kubernetes implementation we use.  Here's how to install it.  

```
curl -sfLl https://get.k3s.io | INSTALL_K3S_EXEC="--prefer-bundled-bin --disable-cloud-controller" sh -
```
Go to [k3s](https://docs.k3s.io/installation) for detailed installation instructions.

Using a different implementation of Kubernetes is an exercise left to the reader.  

### Install Helm (as root)

The installer downloads a specific version of Helm.  This is mainly because they don't have a *latest* alias on their downloads.  The Helper then extracts the tar file and installs the helm executable into the /usr/local/bin directory.

```
HELM_VERSION="v3.14.3"
HELM_TAR="helm-$HELM_VERSION-linux-amd64.tar.gz"
curl -sfl -o $HELM_TAR https://get.helm.sh/$HELM_TAR
tar zxvf /tmp/$HELM_TAR -C /tmp
mv /tmp/linux-amd64/helm /usr/local/bin/helm
```

Addition Helm installation information can be found [here](https://helm.sh/docs/intro/install/)

### TLS Certificates

For deploying SCOT for testing purposes, you can use self signed certificates.  If you
are planning on using SCOT in production, you will need a valid certificate for the URL
that you are planning on serving SCOT from.  

To create self-signed certificate for testing:

```
$ export KEYFILENAME="/home/test/.ssl/scot4.key"
$ export CSRFILENAME="/home/test/.ssl/scot4.csr"
$ export CRTFILENAME="/home/test/.ssl/scot4.crt"
$ openssl genrsa -out $KEYFILENAME 2048
$ openssl req -key $KEYFILENAME -new -out $CSRFILENAME
$ openssl x509 -signkey $KEYFILENAME -in $CSRFILENAME -req -days 365 -out $CRTFILENAME
```

### IP Address

You will need to know the IP address of your server.  This command will help:

`ip -4 -o addr show scope global | awk '{gsub(/\/.*/,"",$4); print $4}'`

### PROXY Configuration 

If you are behind a proxy, you will need to ensure that your HTTPS_PROXY, HTTP_PROXY, and NO_PROXY environment variables are set correctly.

```
$ export HTTPS_PROXY=https://proxy.widget.com:1234
$ export HTTP_PROXY=http://proxy.widget.com:5678
$ export NO_PROXY=localhost,127.0.0.1,.widget.com,172.16.,192.168.,*.local,.local,$IPADDR
```

where $IPADDR is the IP address you discovered in the previous step.


### Firewall Configuration (as root)

Next the helper adjusts the system firewall rules.  Please see [k3s documentation](https://docs.k3s.io/installation/requirements) for more details.  K3s recommends disabling the firewall , however, if you are prevented from doing so by policy, add the following rules.

For RHEL:

```
firewall-cmd --permanent --zone=trusted --add-source=10.42.0.0/16 #pods
firewall-cmd --permanent --zone=trusted --add-source=10.43.0.0/16 #services
firewall-cmd --reload
```

For Ubuntu:

```
ufw allow from 10.42.0.0/16 to any
ufw allow from 10.43.0.0/16 to any
```


*note*: a rule for port 6443 is not necessary for single node installs like SCOT.

### Install Tab Completions (as scot4)

The following commands creates aliases and tab completions to make working on the command line easier.

```
echo "alias k=kubectl" >> /home/scot4/.bashrc
echo "source <(kubectl completion bash)" >> /home/scot4/.bashrc
echo "complete -o default -F __start_kubectl k" >> /home/scot4/.bashrc
echo "kubectl config set-context --current --namespace=scot4" >> /home/scot4/.bashrc
```

These are not absolutely necessary, but make administrating your Kubernetes system easier.

### Disable Swap (as root)

Disabling swap is recommended for k3s.

```
swapoff -a
sed -i '/ swap / s/^/#/' /etc/fstab
```

### Create a scot4 namespace in Kubernetes 

```
su -c 'kubectl create ns scot4' scot4
```

### Ensure that PyYAML is not Ancient (as scot4)

The helper then makes sure the major version number of PyYAML is at least 5.  If it is older, then pip is used to upgrade the module.

```
PYYAMLVER=$(python3 -m pip freeze | grep -i pyyaml | awk -F== '{print $2}')
PYYAMLMAJ=$(echo $PYYAMLVER | awk -F. '{print $1}')

if [ "$PYYAMLMAJ" -lt "5" ]; then
    echo "Upgrading PyYAML..."
    python3 -m pip install --upgrade PyYAML
fi
```

### Merge Secrets into Kubernetes (as scot4)

First, run the program `auto_gen_secrets.py` in the root of the repository.  This script generates secrets necessary for operation and creates the necessary files for the merge.

Now that `auto_gen_secrets.yaml` and `auto_gen_flair_secrets.yaml` have been created in the scot4-chart directory, you are ready to merge them into Kubernetes.

```
kubectl -n scot4 apply -f scot4-chart/auto_gen_secrets.yaml
kubectl -n scot4 apply -f scot4-chart/auto_gen_flair_secrets.yaml
```

### Update values.yaml (as scot4)

Update the `scot4-chart/OS_values.yaml` file with information about your environment. 

Important values to update:

scot4.api:internalDB
: set to false if you will be using SCOT with an external database server

scot4.api.externalApiUri
: set to the `https://{your server name}/api/v1`

scot4.api.enrichmentHost
: set to the URI of your Airflow server.  See [SOAR](/usage/soar.html) for more info.

scot4.frontend.externalHostName
: set to your servername as above

scot4.frontend.vueAppApiBase
: set to the same as scot4.api.externalApiUri

scot4.flair.frontendAccessibleURL
: set to the same as scot4.api.externalApiUri

### Run Helm to Deploy (as scot4)

If this is your first time installing, you will run the following command.  *NOTE*: Running this command will delete all data in your scot4 database.

```
$ cd scot4-chart

$ helm upgrade --kubeconfig /etc/rancher/k3s/k3s.yaml \
               -n scot4 \
               --install \
               --reset-values \
               -f OS_values.yaml \
               --set-string scot4.clean_flair_install="true" \
               --set-string scot4.wipe_api_database="true" \
               scot4 .
```

If you only wish to redeploy, use this command, which will not wipe your database.

```
$ helm upgrade --kubeconfig /etc/rancher/k3s/k3s.yaml \
               -n scot4 \
               --install \
               --reset-values \
               -f OS_values.yaml \
               scot4 .`
```

Congratulations, you have deployed SCOT4.  Proceed to [Configuration](/install/nextsteps.md) for the final setup of your SCOT instance.
