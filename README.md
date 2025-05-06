# pysp-installation
This repo serves as an installation instructions for pysp, a stochastic optimization package developed by the pyomo team, and run parallel simulations of progressive hedging algorithm in your Windows machine. 

>[!Caution]
> PySP is no longer in active development and you must follow this instruction to run your optimization models successfully.

## Installation steps
1. To parallelize any algorithm, you need a message passing protocol. Fortunately, Windows provides Microsoft MPI which you can directly install [here](https://www.microsoft.com/en-us/download/details.aspx?id=105289). Once you install this, you can test a successful installation by executing the following:
    ```bash
    mpiexec -help
    ```
    This should run successfully without error if you have a successful installation.
2. Use the python package manager of your choice and activate your python environment. In the active environment run the following to install necessary packages from `requirements.txt` file provided in this repo.
    ```bash
    pip install -r requirements.txt
    ```   
    >[!Warning]
    > You must install `python<=3.9.x`. I have successfully tested the installation with `python=3.9.5` so I recommend installing the same version for avoiding installation conflicts.
3. Now you have installed the necessary packages and ready to run your model in parallel. Note that only progressive hedging can be run in parallel. 
4. An example command to run your problem in parallel is shown below:
   
   a. First execute these commands in the command prompt one at a time:

   ```bash
   > set PYRO_NS_PORT=8080
   > set PYRO_NS_BCPORT=8080
   > set PYRO_BROKEN_MSGWAITALL=1
   ```

   b. Execute the command below to run the progressive hedging algorithm in parallel:
    ```bash
    mpiexec -np 1 pyomo_ns -n localhost : -np 1 dispatch_srvr -n localhost : -np 2 phsolverserver --pyro-host=localhost --pyro-port=8081 : -np 1 runph -m ReferenceModel.py --output-solver-logs --solver=gurobi --solver-manager=phpyro --phpyro-required-workers=2 --pyro-host=localhost --pyro-port=8081 --shutdown-pyro --default-rho=1
   ```