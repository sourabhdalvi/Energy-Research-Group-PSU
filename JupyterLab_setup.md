# Energy-Research-Group-PSU
---
## Guide for running jupyter lab on a Remote Server (ACI-ICS)
#### 1. Login into ACI-I on ICS
```bash
Sourabhs-MacBook-Pro:~ sourabhdalvi$ ssh spd13@aci-b.aci.ics.psu.edu 
```
---
#### 2. Submit an interactive job and activate/create a conda env 
```bash
(env_name)[spd13@aci-lgn-003 spd13]$ conda install jupyterlab
...
...

(env_name)[spd13@aci-lgn-003 spd13]$ qsub -I -A open -l nodes=1:ppn=2 -l walltime=2:00:00 
Job will run under the 'open' account, as requested.
qsub: waiting for job xxxxxx.torque01.util.production.int.aci.ics.psu.edu to start
qsub: job xxxxxx.torque01.util.production.int.aci.ics.psu.edu ready

[spd13@comp-bc-0224 ~]$ source activate jl-0.6
(jl-0.6) [spd13@comp-bc-0224 ~]$ conda install jupyterlab
(jl-0.6) [spd13@comp-bc-0224 ~]$ hostname
comp-bc-0224.acib.production.int.aci.ics.psu.edu
(jl-0.6) [spd13@comp-bc-0224 ~]$ jupyter lab
...
http://localhost:8888/?token=c8de56fa4deed24899803e93c227592aef6538f93025fe01

```
---
#### 3. Now use local machine to create SSH tunnel to remote Jupyter server remember the hostname (comp-bc-0224.acib.production.int.aci.ics.psu.edu)
```bash
Sourabhs-MacBook-Pro:~ sourabhdalvi$ ssh -N -f -L 9999:comp-bc-0224.acib.production.int.aci.ics.psu.edu:8888 aci-b.aci.ics.psu.edu -l spd13
```
---
#### 4. The SSH tunnel is now ready to use, open any browser and login to the http://localhost:9999
        The jupyter lab interface will ask for a token/password 'c8de56fa4deed24899803e93c227592aef6538f93025fe01' paste this,
        the token will be randomly generated everytime.
        
#### 4. To disconnect on your local machine open a terminal
```bash
Sourabhs-MacBook-Pro:julia-feedstock sourabhdalvi$ ps aux | grep aci-b.aci.ics.psu.edu
sourabhdalvi     41441   0.0  0.0  4306428    764   ??  Ss    3:36PM   0:00.41 ssh -N -f -L 9999:comp-bc-0221.acib.production.int.aci.ics.psu.edu:9988 aci-b.aci.ics.psu.edu -l spd13
sourabhdalvi     41380   0.0  0.1  4306428   5096 s001  S+    3:30PM   0:00.07 ssh spd13@aci-b.aci.ics.psu.edu
sourabhdalvi     42478   0.0  0.0  4277992    924 s002  S+    4:38PM   0:00.00 grep aci-b.aci.ics.psu.edu
```
 Note the id for the ssh tunnel to the jupyter server ( the id will change everytime you start a ssh tunnel)
 
```bash
Sourabhs-MacBook-Pro:julia-feedstock sourabhdalvi$ kill -15 41441
```        
---