# NUS School of Computing server configuration
This document serves as a guide for server configuration in NUS School of Computing. This is mostly used by Prof. Bressan's research group.

Connection
--------
Any connection to the servers listed below can be done as following:
1. Directly when you are in the School of Computing network. Not possible outside SoC (even non-SoC NUS network) 
2. Access via an intermediary server (suna for staff or sunfire for student). You need to have an SoC account for this (for example: ssh username@sunfire.comp.nus.edu.sg, where username is your SoC account). Not possible outside Singapore (?) 
3. Access through SoC VPN (https://webvpn.comp.nus.edu.sg/). You need to have NUSNET account for this. Available anywhere in the world (?). Reference: https://dochub.comp.nus.edu.sg/cf/guides/network/vpn

Database Lab server
--------
### Server Lists
1. Suyeong (suyeong.d2.comp.nus.edu.sg) - 2 GPUs GeForce GTX1080
2. Yuna (yuna.d2.comp.nus.edu.sg) - 1 GPU GeForce GTX1080Ti
3. Fajrian (fajrian-e2s2-1.d1.comp.nus.edu.sg) - 1 GPU GeForce GTX1080Ti, 32 GB RAM, 8 processors

### For user
1. Generate ssh key

   Windows: https://www.ssh.com/ssh/putty/windows/puttygen
   Linux and Mac: https://www.ssh.com/ssh/keygen/
   
2. Give public key to the system administrator (id_rsa.pub)
3. SSH to the server (e.g. ssh username@suyeong.d2.comp.nus.edu.sg)
4. You won't have sudo access. Contact system administrator if there is a need to install something on the server with sudo access.
5. Check the server environment setup below.

### For system administrator
1. Create user: sudo adduser username
2. Copy ssh public key from user to /home/username/.ssh/authorized_keys
3. Add username to the end of the line of /etc/ssh/sshd_config 
4. Restart sshd: sudo systemctl restart ssh.service

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

   ipython notebook --no-browser --port=8889

2. On your computer, start an SSH tunnel:

   ssh -N -f -L localhost:8888:localhost:8889 remote_user@remote_host

3. Now open your browser on the computer and type in the address bar:

   localhost:8888



#### GPU Usage
Check the GPU usage by using the command nvidia-smi.
If you are using Tensorflow or PyTorch, try to not to fully use the GPU so we can share with other users (use CUDA visible devices or dynamic GPU allocation in Tensorflow).

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




