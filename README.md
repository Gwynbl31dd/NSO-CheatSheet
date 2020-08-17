# NSO-CheatSheet
NSO cheat sheet

## pre-Installation 

Make sure your linux has : make, java-jdk 1.8 ,ant,xsltproc
On debian families operating systems (Ubuntu,kubuntu,xubuntu,…)  : 

```
apt-get update | apt-get upgrade | apt-get install make ant openjdk-8-jdk xsltproc -y
```

### Environement variable

export ncsrc

```
echo "source /home/cisco/nso-4.4/ncsrc" >> .profile
$NCS_DIR //NSO's installation directory
```

## NSO CLI

### Add a device

Instanciate from another

```
devices device <dev1> instantiate-from-other-device device-name <dev2>
```

Add device from cli

```
#configure //Go to conf mode
	/*Add a device*/
	#device device <DEVICE_NAME> //Add a device
	#address <ip_addr> port <port_num> //Configure the port and addr
	#device-type <[cli-generic-netconf-snmp]> <[ned-id-protocol-..]> <package> <protocol> <[ssh-telnet]>
	#authgroup <group_name>
	#state admin-state <state>
	//Get out of conf mode
	devices fetch-ssh-host-keys device [ <device_names> ]
devices sync-from //Sync the devices as a source.(In the WebUI, this will change the oper state to "enabled"
```

### Groups

authentification groups

```
devices authgroups group <grp_name> default-map remote-name <username> remote-password <password>
```

Device groups

```
$set devices device-group <name> device-name [ <list_of_name> ] //add devices to a group
$set devices device-group <name> device-group [ <list_of_group_name> ]//Add group to a group
$show devices device-group//Show list of group
#show devices device-group member//Show list of group (not available in conf mode)

```

Customers

```
#show running-config customers// Show customers 
$set customers customer (Name,id,rank,etc) //Set a new customer or modify one
```

### Templates

```
$set devices template <Template_Name> config <package:conf> <option> <...>
```

### Utilities

Divers

```
$switch cli //Change style between Cisco and Juniper CLI Style
$[request] packages reload //Reload the packages 
$show packages //Show used packages.
revert OR abord //Trow away the changes in configuration mode.
```

Copy past long files

```
autowizard false
ignore-leading-space true
complete-on-space false
```

## Version

```
show status ncs-state version
```

## Devices actions

sync

```
devices check-sync//Check sync of devices
devices device ce0..3 check-sync //Check for multiple devices
devices device ce0 compare-config //Compare de configuration
```

live status 

```
#devices device <name> lives-status exec <command > 
OR
#devices device <name> lives-status <device_type>-stats:exec <command > 
```

## Logs

```
file show logs/rollback<TAB> //Show rollbacks files
show devices device-module //Show devices compatible modules
```

## NCS

```
ncs [-c ncs.conf] [--with-package-reload] //Start ncs (ask for package reload must be done at every upgrade)
```

```
ncs --status //Gives the NCS server status
```

```
ncs-setup [--dest <destination>] //Create a running environnement
```

```
ncs-setup --reset //Reset the logs
```

### make packages

```
ncs-make-package <Option> <package-name>//Make a package
```

(Alway make the package (src/ make all)

## Netsim

```
ncs_load -l -m devices.xml //Load device (can be made using CLI)
```

```
ncs-netsim : //NCS network simulator
```

```
ncs-netsim create-network <package_dir> <number_devices> <Prefix>
ncs-netsim add-to-network <package_dir> <number_devices> <Prefix>
ncs-netsim add-device <package_dir> <Device_name>
```

```
ncs-netsim start //Start the device
ncs-netsim delete-network //Stop and delete
ncs-netsim -a start <device_name>//Start a device async
ncs-netsim list //gives the list of devices
ncs-netsim cli-i <DEVICE_NAME> //open a connection to the device 
```

```
ncs-setup --netsim-dir <netsim_dir> --dest <ncs_running_directory> //Start ncs configuration using netsim devices.
```

```
% ncsc --ncs-compile-module yang/interfaces.yang --ncs-device-dir ncsc-out --ncs-device-type netconf// import device data models is the ncsc
```

### Load config

```
$ ncs-netsim ncs-xml-init > devices.xml
$ ncs_load -l -m devices.xml
```

