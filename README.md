# msvsdaa


# Installing open source tools and Sky130pdk
To run the opensource skywater 130 pdk and understand the design flow, the following tools are to be installed;
Magic
Netgen
Xschem
Ngspice
open_pdk
ALIGN

## MAGIC
Magic is an opensource VLSI tool used to do layout of a circuit/schematic.

Follow these steps to install MAGIC:

Open terminal ;
```
$  git clone git://opencircuitdesign.com/magic
$  cd magic
$	 ./configure
$  make
$  sudo make install
```
Complete information regarding MAGIC can be found at; http://opencircuitdesign.com/magic/index.html

## Netgen

Netgen is used to compare netlists, perform LVS (Layout vs Schematic)

Open terminal;
```
$  git clone git://opencircuitdesign.com/netgen
$  cd netgen
$	./configure
$  make
$  sudo make install
```
Complete information regarding Netgen can be found at; http://opencircuitdesign.com/netgen/index.html

## Xschem
Xschem is a software used to do the schematic 

Open terminal;
``` 
git clone https://github.com/StefanSchippers/xschem.git xschem-src
cd xschem-src; ./configure
make
sudo make install
```
Complete information regarding Xschem can be found at; http://repo.hu/projects/xschem/index.html

## Ngspice
Ngspice is the open-source spice simulator for electric and electronic circuits.

First downlaod the tarball from https://sourceforge.net/projects/ngspice/files/ng-spice-rework/old-releases/ to a local directory. It gets downloaded in .tar.gz format. Then, open terminal;
```
 $ tar -zxvf ngspice-37.tar.gz
 $ cd ngspice-37
 $ mkdir release
 $ cd release
 $ ../configure  --with-x --with-readline=yes --disable-debug
 $ make
 $ sudo make install
 ```
 Complete information regarding ngspice can be found at; https://ngspice.sourceforge.io/index.html
 
 To view the simulation graphs of ngspice, xterm is required and can be installed using;
 ```
 $ sudo apt-get update
 $ sudo apt-get install xterm
 ```
 
 ## open_pdk
 Open_PDKs is distributed with files that support the Google/SkyWater sky130 open process description https://github.com/google/skywater-pdk. Open_PDKs   will set up an environment for using the SkyWater sky130 process with open-source EDA tools and tool flows such as magic, qflow, openlane, netgen, klayout, etc.
 
 To install, open terminal;
 ```
 $  git clone git://opencircuitdesign.com/open_pdks
 $  cd open_pdks
 $	./configure --enable-sky130-pdk
 $  make
 $  sudo make install
 ```
 
 ## ALIGN
 
Prerequisites

gcc >= 6.1.0 (For C++14 support)
python >= 3.7

Open terminal;
```
export CC=/usr/bin/gcc
export CXX=/usr/bin/g++
git clone https://github.com/ALIGN-analoglayout/ALIGN-public
cd ALIGN-public

#Create a Python virtualenv
python3 -m venv general
source general/bin/activate
python3 -m pip install pip --upgrade

# Install ALIGN as a USER
pip install -v .
```
If error occurs as shown below;

![Screenshot from 2023-02-06 21-09-58](https://user-images.githubusercontent.com/67609171/218326447-5aca3ddd-2fce-408f-bf98-5e4002d1aed9.png)

refer these links for solution;
https://bobbyhadz.com/blog/python-note-this-error-originates-from-subprocess;
https://candid.technology/error-subprocess-exited-with-error/

or, 
```
sudo apt-get install libboost-all-dev
sudo apt-get update
sudo apt-get install lp-solve
```
### Making ALIGN portable with SKY130 Technology
Clone the following Repository inside ALIGN-public directory
```
git clone https://github.com/ALIGN-analoglayout/ALIGN-pdk-sky130
```
Search for "SKY130_pdk" and move "SKY130_pdk" folder to "ALIGN-public/pdks".

![Screenshot from 2023-02-12 23-07-41](https://user-images.githubusercontent.com/67609171/218327344-059a9c1e-ac2e-4eb6-9ef6-30c083ff54e8.png)

### Running ALIGN tool
Everytime we start running tool in new terminal run following commands.
```
python3 -m venv general
source general/bin/activate
```
Inside ALIGN-public directory, open terminal;
```
mkdir work
cd work
```
## Understanding the design flow
A working directory can be made by copying the required files as follows;

Run these commands everytime you start a new project;
```
$ mkdir <Folder_name>
$ cd <Folder_name>
$ mkdir mag
$ mkdir netgen
$ mkdir xschem
$ cd xschem
$ cp /usr/local/share/pdk/sky130A/libs.tech/xschem/xschemrc .
$ cp /usr/local/share/pdk/sky130A/libs.tech/ngspice/spinit .spiceinit
$ cd ../mag
$ cp /usr/local/share/pdk/sky130A/libs.tech/magic/sky130A.magicrc .magicrc
$ cd ../netgen
$ cp /usr/local/share/pdk/sky130A/libs.tech/netgen//sky130A_setup.tcl .
```

## Creating inverter schematic using xschem
Open the xschem 



![Screenshot from 2023-02-11 13-37-58](https://user-images.githubusercontent.com/67609171/218327729-61a96c95-3227-4501-9308-2d7710cdad94.png)

![Screenshot from 2023-02-11 13-44-13](https://user-images.githubusercontent.com/67609171/218328219-6039c611-e00a-419d-893a-15797f46fe95.png) 


Go to Tools-->/user/loca/share/pdk/sky130A/libs.tech/xschem-->sky130_fd_pr (here you can select nmos and pmos)

For pins; /user/local/share/xschem/xschem_library-->devices

Draw the schematic

![Screenshot from 2023-02-11 13-41-43](https://user-images.githubusercontent.com/67609171/218328073-7d76ca68-372a-412f-8ea0-35f0a57c3f46.png)


Convert schematic to symbol

![Screenshot from 2023-02-11 13-43-09](https://user-images.githubusercontent.com/67609171/218328112-4568ecf7-f935-47ed-9463-16cfd2a2a5f0.png)


Using the generated symbol, build the test_bench to simulate the circuit;

![Screenshot from 2023-02-11 13-42-12](https://user-images.githubusercontent.com/67609171/218328305-cf2a6250-7b08-46bb-870d-b027a3392dec.png)


A file by name .spiceinit is present in xschem folder. Copy this file and paste it in .xschem folder-->simulations (.xschem folder will be hidden, select show hidden folders to access .xschem)

After creating the test_bench, click on Netlist, the Simulate;


![Screenshot from 2023-02-11 13-50-02](https://user-images.githubusercontent.com/67609171/218328496-52f96eee-da6f-47da-81ac-c3c2cccfb04a.png)


The pre-layout characterisation using xschem and ngspice is obtained.

## Inverter Post-layout characterization using Magic and SKY130 PDKs

Open Magic tool

Open terminal;
```
magic -d XR
```
![Screenshot from 2023-02-12 23-35-29](https://user-images.githubusercontent.com/67609171/218328748-1e5807fe-81fa-4b4c-bdee-eaef222e46db.png)


![Screenshot from 2023-02-12 23-36-59](https://user-images.githubusercontent.com/67609171/218328812-9c101357-4c8d-4d84-80f1-597ec1d23abc.png)



Click on File-->Import SPICE, select the generated spice file .xschem folder (.xschem-->simulations--> <filename>.spice)


![Screenshot from 2023-02-11 22-19-25](https://user-images.githubusercontent.com/67609171/218328936-bd918a8c-1b38-4acd-b806-f2828b86b7e8.png)



Click v to see the pins;

![Screenshot from 2023-02-11 22-48-16](https://user-images.githubusercontent.com/67609171/218328988-86ed6bb0-4042-48fd-9322-8e5ca2adcb73.png)



Move the components by hovering the cursor on the component, click i and click m where you want to move them. If this doesn't work, draw a transparent block on the component to be moved, go to edit-->select area and click m where you want to move. Do the connections.

![Screenshot from 2023-02-12 20-58-30](https://user-images.githubusercontent.com/67609171/218329113-402020fe-3274-4ca6-9388-72f26b56740f.png)


Now, go to File --> save and select autowrite.

### Performing LVS checks on testbench and layout netlists
In the tkcon window, use the commands to extract netlist of the layout.

![Screenshot from 2023-02-12 23-44-38](https://user-images.githubusercontent.com/67609171/218329227-71bedb5d-3b31-4926-bbd9-e1fa3d3cd783.png)


```
% extract do local
% extract all
% ext2spice lvs
% ext2spice
```
To peform SPICE simulation, paste the pre-layout netlist of inverter testbench into the magic generated inverter spice netlist which is inside the mag folder.

### Pre- Layout Inverter Testbench Spice Netlist

```
** sch_path: /home/rahul/Documents/Inverter/xschem/inverter_tb.sch
**.subckt inverter_tb in out
*.ipin in
*.opin out
x1 net1 in out GND inverter
V1 in GND PWL(0 0 20n 0 900n 1.8)
.save i(v1)
V2 net1 GND 1.8
.save i(v2)
**** begin user architecture code

.lib /usr/local/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt


.control
save all
tran 1n 1u
plot v(in) v(out)
.endc

**** end user architecture code
**.ends

* expanding   symbol:  inverter.sym # of pins=4
** sym_path: /home/rahul/Documents/Inverter/xschem/inverter.sym
** sch_path: /home/rahul/Documents/Inverter/xschem/inverter.sch
.subckt inverter vdd vin vout vss
*.ipin vin
*.opin vout
*.iopin vdd
*.iopin vss
XM1 vout vin vss vss sky130_fd_pr__nfet_01v8 L=0.18 W=4.5 nf=3 ad='int((nf+1)/2) * W/nf * 0.29' as='int((nf+2)/2) * W/nf * 0.29'
+ pd='2*int((nf+1)/2) * (W/nf + 0.29)' ps='2*int((nf+2)/2) * (W/nf + 0.29)' nrd='0.29 / W' nrs='0.29 / W'
+ sa=0 sb=0 sd=0 mult=1 m=1
XM2 vout vin vdd vdd sky130_fd_pr__pfet_01v8 L=0.18 W=3 nf=3 ad='int((nf+1)/2) * W/nf * 0.29' as='int((nf+2)/2) * W/nf * 0.29'
+ pd='2*int((nf+1)/2) * (W/nf + 0.29)' ps='2*int((nf+2)/2) * (W/nf + 0.29)' nrd='0.29 / W' nrs='0.29 / W'
+ sa=0 sb=0 sd=0 mult=1 m=1
.ends

.GLOBAL GND
.end
```

After selectively pasting this netlist into the inverter.spice generated(extracted) from Magic Tool, the inverter.spice netlist looks like this;

```
* NGSPICE file created from inverter.ext - technology: sky130A
** sch_path: /home/rahul/Documents/Inverter/xschem/inverter_tb.sch
**.subckt inverter_tb in out
*.ipin in
*.opin out
x1 net1 in out GND inverter
V1 in GND PWL(0 0 20n 0 900n 1.8)
.save i(v1)
V2 net1 GND 1.8
.save i(v2)
**** begin user architecture code

.lib /usr/local/share/pdk/sky130A/libs.tech/ngspice/sky130.lib.spice tt


.control
save all
tran 1n 1u
plot v(in) v(out)
.endc

**** end user architecture code
**.ends

* expanding   symbol:  inverter.sym # of pins=4
** sym_path: /home/rahul/Documents/Inverter/xschem/inverter.sym
** sch_path: /home/rahul/Documents/Inverter/xschem/inverter.sch
.subckt inverter vdd vin vout vss
*.ipin vin
*.opin vout
*.iopin vdd
*.iopin vss
XM1 vout vin vss vss sky130_fd_pr__nfet_01v8 L=0.18 W=4.5 nf=3 ad='int((nf+1)/2) * W/nf * 0.29' as='int((nf+2)/2) * W/nf * 0.29'
+ pd='2*int((nf+1)/2) * (W/nf + 0.29)' ps='2*int((nf+2)/2) * (W/nf + 0.29)' nrd='0.29 / W' nrs='0.29 / W'
+ sa=0 sb=0 sd=0 mult=1 m=1
XM2 vout vin vdd vdd sky130_fd_pr__pfet_01v8 L=0.18 W=3 nf=3 ad='int((nf+1)/2) * W/nf * 0.29' as='int((nf+2)/2) * W/nf * 0.29'
+ pd='2*int((nf+1)/2) * (W/nf + 0.29)' ps='2*int((nf+2)/2) * (W/nf + 0.29)' nrd='0.29 / W' nrs='0.29 / W'
+ sa=0 sb=0 sd=0 mult=1 m=1
.ends

.GLOBAL GND
.end
.subckt sky130_fd_pr__nfet_01v8_N39HBR a_n33_172# a_n275_n324# a_18_n150# a_114_n150#
+ a_n129_n238# a_63_n238# a_n78_n150# a_n173_n150#
X0 a_114_n150# a_63_n238# a_18_n150# a_n275_n324# sky130_fd_pr__nfet_01v8 ad=4.425e+11p pd=3.59e+06u as=4.5e+11p ps=3.6e+06u w=1.5e+06u l=180000u
X1 a_n78_n150# a_n129_n238# a_n173_n150# a_n275_n324# sky130_fd_pr__nfet_01v8 ad=4.5e+11p pd=3.6e+06u as=4.425e+11p ps=3.59e+06u w=1.5e+06u l=180000u
X2 a_18_n150# a_n33_172# a_n78_n150# a_n275_n324# sky130_fd_pr__nfet_01v8 ad=0p pd=0u as=0p ps=0u w=1.5e+06u l=180000u
.ends

.subckt sky130_fd_pr__pfet_01v8_KG2LE3 a_n78_n100# a_n173_n100# a_n129_n197# a_18_n100#
+ a_63_n197# a_114_n100# w_n311_n319# a_n33_131#
X0 a_18_n100# a_n33_131# a_n78_n100# w_n311_n319# sky130_fd_pr__pfet_01v8 ad=3e+11p pd=2.6e+06u as=3e+11p ps=2.6e+06u w=1e+06u l=180000u
X1 a_114_n100# a_63_n197# a_18_n100# w_n311_n319# sky130_fd_pr__pfet_01v8 ad=2.95e+11p pd=2.59e+06u as=0p ps=0u w=1e+06u l=180000u
X2 a_n78_n100# a_n129_n197# a_n173_n100# w_n311_n319# sky130_fd_pr__pfet_01v8 ad=0p pd=0u as=2.95e+11p ps=2.59e+06u w=1e+06u l=180000u
.ends

.subckt inverter vout vdd vss
XM1 m1_n114_692# VSUBS vout vout m1_n114_692# m1_n114_692# vout vout sky130_fd_pr__nfet_01v8_N39HBR
XM2 vout vout m1_132_1786# vout m1_132_1786# vout vout m1_n114_1798# sky130_fd_pr__pfet_01v8_KG2LE3
R0 m1_n114_1798# m1_132_1786# sky130_fd_pr__res_generic_m1 w=210000u l=2.775e+06u
.ends
```
Place .spiceinit file in the directory (mag) to perform the simulations without any error

 Run the following command;
 ```
 ngspice inverter.spice
 ```
 
![Screenshot from 2023-02-12 23-57-13](https://user-images.githubusercontent.com/67609171/218329748-3d6c8d18-4865-4ba4-a30b-fad2f84f29da.png)







