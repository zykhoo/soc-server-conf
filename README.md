# NUS School of Computing server configuration
This document serves as a guide for server configuration in NUS School of Computing. This is mostly used by Prof. Bressan's research group.

Connection
--------
Any connection to the servers listed below can be done as following:
1. Directly when you are in the School of Computing network. Not possible outside SoC (even non-SoC NUS network) 
2. Access via an intermediary server (suna for staff or sunfire for student). You need to have an SoC account for this (for example: ssh username@sunfire.comp.nus.edu.sg, where username is your SoC account). Not possible outside Singapore (?) 
3. Access through SoC VPN (https://webvpn.comp.nus.edu.sg/). You need to have NUSNET account for this. Available anywhere in the world (?). Reference: https://dochub.comp.nus.edu.sg/cf/guides/network/vpn (tag: forticlient)

Database Lab server
--------
### Server Lists
1. Suyeong (suyeong.d2.comp.nus.edu.sg) - 2 GeForce GTX1080 GPUs, 64 GB RAM, 12 processors 
2. Yuna (yuna.d2.comp.nus.edu.sg) - 1 GeForce GTX1080Ti GPU, 64 GB RAM, 16 processors
3. Fajrian (fajrian-e2s2-1.d1.comp.nus.edu.sg) - 1 GPU GeForce GTX1080Ti, 32 GB RAM, 8 processors
4. taeyeon (taeyeon.d2.comp.nus.edu.sg) - 1 A100 PCIe Tensor Core GPU, 64 processors

### For user
1. Generate ssh key

   Windows: Step 1 of https://system.cs.kuleuven.be/cs/system/security/ssh/setupkeys/putty-with-key.html#s1
   Then in puttygen, select the ```Conversions``` tab and convert the format of the key. This is done by ```import```-ing your current ```.ppk``` file, then ```Export``` as ```OpenSSH key``` (again in ```Conversions``` tab).
   
   OR Windows: in powershell, use the command ```ssh-keygen```. Navigate to the folder where your ssh keys have been generated. You should see two files. The first is ```id_rsa``` and the second is ```id_rsa.pub```. The latter is your public key. 

   
   Linux and Mac: https://www.ssh.com/ssh/keygen/
   
2. Give public key (e.g. id_rsa.pub) to the system administrator.
3. Wait for system administrator to add you to the server.
3. SSH to the server.

   Windows: Step 3 of https://system.cs.kuleuven.be/cs/system/security/ssh/setupkeys/putty-with-key.html#s3
   
   Linux and Mac: ssh username@suyeong.d2.comp.nus.edu.sg in terminal.
   
4. You won't have sudo access. Contact system administrator if there is a need to install something on the server with sudo access.
5. Check the server environment setup below.

Users may use this vpn to access the SOC network: https://webvpn.comp.nus.edu.sg/

### Server Environment

#### Editor
Try to use vim or gedit for the editor. You can also use Visual Studio code with remote setup to the server.

#### File Browser
For Windows or Mac, you can use Filezilla (https://filezilla-project.org/) for file transferring between server and your local computer.

#### Python
Try not to use the system's Python. We recommend to use pyenv.

Follow the "Basic GitHub checkout" installation here: https://github.com/pyenv/pyenv#installation and install the Python versions that you want. 

After that you can use *pip* command without sudo access. 

Do not forget to use *pip install tensorflow-gpu* to use GPU with Tensorflow.

#### Jupyter Notebook
If you want to use Jupyter notebook on the server, you need to do a port forwarding.

1. On the server, start the IPython notebooks server:

   jupyter notebook --no-browser --port=8889

2. On your computer, start an SSH tunnel:

   ssh -N -f -L localhost:8888:localhost:8889 remote_user@remote_host

3. Now open your browser on the computer and type in the address bar:

   localhost:8888

#### GPU Usage
Check the GPU usage by using the command nvidia-smi.
If you are using Tensorflow or PyTorch, try to not to fully use the GPU so we can share with other users (use CUDA visible devices or dynamic GPU allocation in Tensorflow).

If there are multiple GPUs, consider setting memory growth on one of the GPUs following: 
```physical_devices = tf.config.list_physical_devices('GPU') ```

```tf.config.experimental.set_memory_growth(physical_devices[0], True)```
https://stackoverflow.com/questions/60048292/how-to-set-dynamic-memory-growth-on-tf-2-1

If there is only one GPU, consider setting memory growth on that GPU, to use only a fraction of the GPU, following: 
```gpu_options = tf.GPUOptions(per_process_gpu_memory_fraction=0.333)```

```sess = tf.Session(config=tf.ConfigProto(gpu_options=gpu_options))```
https://stackoverflow.com/questions/34199233/how-to-prevent-tensorflow-from-allocating-the-totality-of-a-gpu-memory

to check that the memory growth is limited:
```tf.config.experimental.get_memory_usage('GPU:0')```
https://www.tensorflow.org/api_docs/python/tf/config/experimental/get_memory_usage

### For system administrator
1. Create user: sudo adduser username by adding a new file for the user in root
2. Copy ssh public key from user to /home/username/.ssh/authorized_keys (ensure public key is formatted ```ssh_rsa <public key, in one line> <username>```, then use ```nano authorized_keys```)
3. Add username to the end of the line of /etc/ssh/sshd_config 
4. Restart sshd: sudo systemctl restart ssh.service

#### If .ssh file is not created automatically after adduser:
1. mkdir /home/user/.ssh
2. chmod 700 /home/user/.ssh
3. cd /home/user/.ssh
4. nano authorized_keys (this creates the authorized_keys file)
5. chmod 600 authorized_keys
6. chown -R user:user /home/user/.ssh
7. sudo systemctl restart ssh.service

#### After entering the server
1. Create a virtual environment ```python3 -m venv <name_of_venv>```
2. ```exec "$SHELL"```
3. ```source <name_of_venv>/bin/activate```
4. ```pip install jupyter``` (and any other libraries)
5. ```ipython kernel install --user --name=<name_of_venv>```
6. ```jupyter notebook --no-browser --port xxxx```


School of Computing server
--------

### Setup account
<b>Source</b>: https://dochub.comp.nus.edu.sg/cf/services/compute-cluster

<b>Prerequisite</b>: Have SoC account (automatically created for you if you are an SoC student/staff, you need to request to the Prof if you are a guest)

1. Open https://mysoc.nus.edu.sg/app/myacct/
2. Go to "My SoC Resources" menu.
3. Select "Services".
4. Enable "The SoC Compute Cluster" service.

### Server Lists
The server that you can login into can be seen here: https://dochub.comp.nus.edu.sg/cf/guides/compute-cluster/hardware

If you are outside SoC network, access options are via an intermediary server (such as suna or sunfire) OR use SoC VPN to make the initial virtual jump into SoC first.

### Running MPI Program
Guide for running MPI program: https://dochub.comp.nus.edu.sg/cf/guides/compute-cluster/tembusu/mpi

### When the server memory is full
1. Go to root, and check usage via ```df -lh```.
2. Create and move files to ```/mnt/data``` for the user
a. Make directory for the user ```mkdir /mnt/data/<user>```
b. Move files (for example, moving all files for one user ```mv <user> /mnt/data```)
c. Change the folder's permission to the user ```chown <user>:<user> /mnt/data/<user>```
4. Use ```du -sh *``` to check the server memory usage of individual users


### Finding top 10 users that take up storage space:
```du -a /home |sort -n -r |head -n 10```


