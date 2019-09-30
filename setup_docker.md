# Jumpscale Docker Install

## Create Docker container
We will first create a Docker container on your system using Ubuntu 18.10 image, we can mount the code so it's easy to edit it in your favorite IDE. We  will then connect to the container using docker exec.

```
docker run --privileged --hostname=js --name=js -v /sandbox_jsx:/sandbox -i -t -d ubuntu:18.10
docker exec -it js /bin/bash
```


## SSH keys
Once the Docker has started, it's important to generate SSH keys using ssh-keygen. We will either copy your local keys to the docker or create new keys int the Docker container. Once the keys are generated, be sure they are [added to your profile in github](https://help.github.com/en/articles/adding-a-new-ssh-key-to-your-github-account).
### option A: Generate new keys

Exec following command in your Docker container, and follow wizard (default answers should work).
```
cd ~
ssh-keygen -t rsa
```

Key can be found in ~/.ssh/id_rsa.pub
Add this key in Github to be able to access your code

### option B: Use keys from your own system
```
docker cp ~/.ssh js:/root/.ssh
```
### Load keys

Now the keys are generated, we need to load them into the system.
```
eval `ssh-agent -s`
ssh-add
```

## Install JSX

Now you have SSH keys in place, we can install JSX.

```
curl https://raw.githubusercontent.com/threefoldtech/jumpscaleX_core/development_stabilize/install/jsx.py?$RANDOM > /tmp/jsx;
chmod +x /tmp/jsx;
/tmp/jsx install
```

When installed, you can run the shell.

```
. /sandbox/env.sh
kosmos -p
```

Now you are in the JSX shell, we need to setup your ZDB system.

```
j.builders.db.zdb.build()
j.builders.apps.sonic.install()

j.builders.db.zdb.start()

```
