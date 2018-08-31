## Setting up JuMP + Julia on Remote/Local machine 
---
#### 1. First Step is to install Miniconda which works as a package manager and allows user to create seperate environments for different projects
```bash
[spd13@comp-ic-0014 ~]$ cd Downloads

[spd13@comp-ic-0014 Downloads]$ wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh

[spd13@comp-ic-0014 Downloads]$ sh Miniconda3-latest-Linux-x86_64.sh

Welcome to Miniconda3 4.5.4

In order to continue the installation process, please review the license
agreement.
Please, press ENTER to continue
...
...
...
```

---

#### 2. The Final Step before installation is to specify the installtion location, its better to install in your work directory
```bash
Miniconda3 will now be installed into this location:
/storage/home/spd13/miniconda3

  - Press ENTER to confirm the location
  - Press CTRL-C to abort the installation
  - Or specify a different location below

[/storage/home/spd13/miniconda3] >>> /storage/work/spd13/miniconda3

```
---

#### 3. Once we have Miniconda setup, first we create a new environment for Julia-0.6 with the name jl-0.6 and specifying the version of julia we need 
```bash
[spd13@comp-ic-0014 ~]$ conda create -n jl-0.6 julia=0.6
```
---

#### 4. Next we need to setup/install solvers

    - CPLEX # already available in ACI
        - Load : module load cplex
        - Direct Julia to CPLEX, next line should only be exceuted once
        ```bash
          [spd13@comp-ic-0014 ~]$ export LD_LIBRARY_PATH="/opt/aci/sw/cplex/12.8.0/cplex/bin/x86-64_linux":$LD_LIBRARY_PATH >> ~/.bash_profile
          
          [spd13@comp-ic-0014 ~]$ CPLEX_STUDIO_BINARIES=/opt/aci/sw/cplex/12.8.0/cplex/bin/x86-64_linux julia -e 'Pkg.add("CPLEX"); Pkg.build("CPLEX")'
        ```
        - Drawback only supported till Julia-0.6, JuMP-0.18; Under development for Julia-1.0, JuMP-0.19, MOI(MathOptInterface)
    - Gurobi # needs installing plus registration for a license 
        - Copy gurobi8.0.1_linux64.tar.gz from the group directory and install gurobi in your work directory
        ```bash
          [spd13@comp-ic-0014 ~]$ cp /gpfs/group/mdw18/default/setup/gurobi8.0.1_linux64.tar.gz ~/work/
        
          [spd13@comp-ic-0014 ~]$ cd ~/work && tar xzf gurobi8.0.1_linux64.tar.gz
        
          [spd13@comp-ic-0014 ~]$ echo "# Gurobi library" >> ~/.bashrc
          
          [spd13@comp-ic-0014 ~]$ export GUROBI_HOME="/storage/work/programs/gurobi801/linux64" >> ~/.bashrc

          [spd13@comp-ic-0014 ~]$ export PATH="${PATH}:${GUROBI_HOME}/bin" >> ~/.bashrc

          [spd13@comp-ic-0014 ~]$ export LD_LIBRARY_PATH="${LD_LIBRARY_PATH}:${GUROBI_HOME}/lib" >> ~/.bashrc

          [spd13@comp-ic-0014 ~]$ export GRB_LICENSE_FILE="/storage/home/spd13/gurobi.lic" >> ~/.bashrc

        ```
        -  Once you get your License, activate gurobi and save the License in the default location
        ```bash
          [spd13@comp-ic-0014 ~]$ grbgetkey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
          
          [spd13@comp-ic-0014 ~]$ export GRB_LICENSE_FILE="/storage/home/spd13/gurobi.lic" >> ~/.bashrc
        ```
     - GLPK, CLP, CBC
         - All can be installed in Julia 
    
---

#### 4. Actiavte the new env, to access the julia 

```bash
[spd13@comp-ic-0014 ~]$ source activate jl-0.6

(jl-0.6) [spd13@comp-ic-0014 ~]$ module load cplex  

(jl-0.6) [spd13@comp-ic-0014 ~]$ julia
               _
   _       _ _(_)_     |  A fresh approach to technical computing
  (_)     | (_) (_)    |  Documentation: https://docs.julialang.org
   _ _   _| |_  __ _   |  Type "?help" for help.
  | | | | | | |/ _` |  |
  | | |_| | | | (_| |  |  Version 0.6.1
 _/ |\__'_|_|_|\__'_|  |  conda-forge-julia release
|__/                   |  x86_64-redhat-linux


julia> 
```
---

#### 5. We are now ready to add packages, for the Solvers and our Modeling Language
```julia
julia> Pkg.init() # This is only need for the first time you run julia

julia>  Pkg.add("JuMP")

julia>  Pkg.add("Gurobi")

julia>  ENV['']

julia>  Pkg.add("GLPK")

julia>  Pkg.add("CLP")

julia>  Pkg.add("CBC")

julia>  exit()
```
---

#### 6. Currently Julia hosts a variety of solvers and modeling packages with you can solve 
    - MILP
    - NLP
    - Convex programs
    - Semidefinite programs
    - Mixed-integer convex optimization
    - Second-order cone programs
    - Exponential cone programs
    - Power cone programs
    All this is available in [JuliaOpt](https://github.com/JuliaOpt) 
    
---  
  
#### 7. Try a example problem in JuMP

```bash
(jl-0.6) [spd13@comp-ic-0014 ~]$ julia -e 'include("/gpfs/group/mdw18/default/setup/gurobi_example.jl")'
```
Or 
```julia
julia> include("/gpfs/group/mdw18/default/setup/gurobi_example.jl")
```