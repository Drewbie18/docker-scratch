# Create Swarm on Windows 10 Pro

Windows 10 pro comes wither Hypervisor installed so in order to run a local swarm Hypervisor needs to be used. 

## Known Issue: docker-machine hangs on create
Sometimes on windows using the docker-machine command to create an instance of a virtual machine that is running docker the process will hang somewhere in the initialization.

The solution to this is to create a new virtual switch in Hyper-V Manager that will be eplicitly used to run virtual machines for docker. 

### Solution
- Open 'Hyper-V Manager'
- In the right hand 'Actions' pane open 'Virtual Switch Manager'
- In the 'Virtual Switch Manager' window select 'External' and press 'Create Virtual Switch'
- Give the virtual switch a name that you will remember easily eg: 'DockerNAT'
- Make sure that the 'Connection type' is set to 'External network' and the connector has access to the internet. (i.e if you are ) 


## Cerate Docker Swarm
- Open an elevated powershell session (run as administrator)
- docker-machine create --driver hyperv --hyperv-virtual-switch "Virtual Switch Name" "name of virtual machine" 
-Example: docker-machine crete --driver hyperv --hyperv-virtual-switch "DockerNat"


