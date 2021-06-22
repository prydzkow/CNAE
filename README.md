Candid Offline Data Collection
==============================

This script is used to collect data from a APIC/DCNM/MSO Cluster

Information is collected from APIC/DCNM/MSO controller, leaf & spine switches that are part of fabric.
REST API/VSH commands are listed below


Operation Systems Supported :
-----------------------------

We support following OS with python version 2.7.11 or later (2.7.11+)

Ubuntu 14.04/16.04
OS X El Capitan 10.11.6
CentOS 7.x

Common Prerequisites:
--------------
Linux/Unix/Mac machine
Need internet connection
Python 2.7.11 or later (Not python 3)
python pip installed.

Python Packages:
----------------

Check if pip is installed , by doing "which pip" in a bash prompt.
If the location of pip is not returned, follow the instructions to install pip

Step1 : Check for prerequisites   :
------------------------------------
    a) Install pip if its not present:
       -------------------------------

       For Ubuntu:
       -----------
            If python pip is not installed:-
            sudo apt-get install python-pip
            sudo apt-get install build-essential libssl-dev libffi-dev python-dev


       For CentOS:
       -----------
            sudo yum install python-pip

       For Mac OS-X:
       --------------
            sudo easy_install pip

     b) Verify pip installation:
        ------------------------

        bash-4.3$ which pip
        /usr/local/bin/pip

     c) Install wget

        For Ubuntu:
        ------------
        apt install wget


        For CentOS
        ----------
        sudo yum install wget


        For Mac OS-X:
        --------------
          1) Install brew if  its not present
            /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

          2) Check brew installation
             terminal:~ which brew
             /usr/local/bin/brew
          3) Install wget using brew
             terminal:~ tshakil$ brew install wget


      d) Verify wget installation
            terminal:~ which wget
            /usr/local/bin/wget

      e) Install openssl

         For CentOS:
         ----------
            terminal:~ yum install openssl

         For Ubuntu:
         ----------
            terminal:~ sudo apt-get install openssl

         For Mac OS-X:
         -------------
            terminal:~ brew install openssl

       f) Verify openssl installation
            terminal:~ which openssl
            /usr/bin/openssl

2. Test the Python version:
---------------------------

    /usr/local/lib/python2.7.11/bin/python -V   (or) python -V
    Python 2.7.11


Note:
-----
If you have python version less than 2.7.11, please refer "Upgrading to Python"

3. Creating virtualenv:
-----------------------

Please verify if virtualenv is installed using ‘which virtualenv’.
If not installed please use ‘pip install virtualenv’ before proceeding.

After upgrading to python 2.7.11, create a new virtualenv with python 2.7.11
for running the script.

    1) Create a virtual environment with python2.7.11+.
        Note to pass the correct python path in --python variable
        If python version was upgraded to 2.7.11:
            virtualenv --python=/usr/local/lib/python2.7.11/bin/python venv
        If existing python verison is later than 2.7.11
            virtualevn venv

        Example:
        [root@CentOS7 ~]# virtualenv --python=/usr/local/lib/python2.7.11/bin/python venv
        Running virtualenv with interpreter /usr/local/lib/python2.7.11/bin/python
        New python executable in /root/venv/bin/python
        Installing setuptools, pip, wheel...done.
        [root@CentOS7 ~]#

	2) Activate the new virtualenv as below.

		[root@CentOS7 ~]# source venv/bin/activate
		(venv) [root@CentOS7 ~]#
		(venv) [root@CentOS7 ~]# python -V
		Python 2.7.11
		(venv) [root@CentOS7 ~]#

    3) Do pip install of paramiko and requests package in virtualenv

		For both Mac/Linux inside the virtualenv install the following packages
		(venv) [root@CentOS7 ~]# pip install paramiko==2.0.1
		(venv) [root@CentOS7 ~]# pip install requests==2.10.0
		(venv) [root@CentOS7 ~]# pip install -U pip setuptools==34.3.3

	4) Verify if the packages got installed

		(venv) [root@CentOS7 ~]# pip list | grep 'paramiko\|requests'
		paramiko (2.0.1)
		requests (2.10.0)
		(venv) [root@CentOS7 ~]#

    5) Install and verify optional packages

        5.1) pyVmomi (for VMware VCenter)

        (venv) [root@CentOS7 ~]# pip install pyvmomi==6.5.0.2017.5.post1
        (venv) [root@CentOS7 ~]#
        (venv) [root@CentOS7 ~]# pip list | grep pyvmomi
        pyvmomi (6.5.0.2017.5.post1)
        (venv) [root@CentOS7 ~]#

        5.2) nitro-python (for Citrix NetScaler)

        a) Download complete SDK (ns-10.5-60.7-sdk.tar.gz) from https://www.citrix.com/downloads/netscaler-adc/sdks/netscaler-sdk-release-105.html
        b) Extract ns-10.5-60.7-nitro-python.tgz inside ns-10.5-60.7-sdk.tar.gz to get nitro-python-1.0
        c) Go inside nitro-python-1.0 directory and install (as per readme_start.txt) and verify as follows

        (venv) [root@CentOS7 ~]# python setup.py install
        (venv) [root@CentOS7 ~]#
        (venv) [root@CentOS7 ~]# pip list | grep nitro-python
        nitro-python (1.0)
        (venv) [root@CentOS7 ~]#

        5.3) f5-sdk (for F5 Load Balancer)

        (venv) [root@CentOS7 ~]# pip install f5-sdk==3.0.21
        (venv) [root@CentOS7 ~]#
        (venv) [root@CentOS7 ~]# pip list | grep f5-sdk 
        f5-sdk  3.0.2 
        (venv) [root@CentOS7 ~]#

    6) Untar cnae_data_collection.zip/analysis-collectors.tar.gz inside virtualenv and
       verify version.properties is present in the same folder as cnae_data_collection.py

	7) Collect the offline data, after collection finally deactivate the virtual env
           Refer to section : Running the script  to collect the data

	8) To deactivate the virtualenv,

		(venv) [root@CentOS7 ~]# deactivate
		[root@CentOS7 ~]#python -V
		Python 2.7.5
		[root@CentOS7 ~]#


Upgrading to Python 2.7.11 if on any version < 2.7.11
-------------------------------------------------------

    For Operating System : OS X
    ----------------------------

         7) Download Python2.7.11 and install

            OPENSSL_ROOT="$(brew --prefix openssl)"
            wget https://www.python.org/ftp/python/2.7.11/Python-2.7.11.tgz
            tar xfz Python-2.7.11.tgz
            cd Python-2.7.11/
            ./configure CPPFLAGS="-I$OPENSSL_ROOT/include" LDFLAGS="-L$OPENSSL_ROOT/lib"  --prefix /usr/local/lib/python2.7.11 --enable-ipv6
            make
            sudo make install

    Ubuntu:
    ----------------------------

          1) Get Python Source Code and Compile:
              ------------------------------------

              wget https://www.python.org/ftp/python/2.7.11/Python-2.7.11.tgz
              tar xfz Python-2.7.11.tgz
              cd Python-2.7.11/
              ./configure --prefix /usr/local/lib/python2.7.11 --enable-ipv6
              make
              sudo make install

    CentOS:
    ----------------------------

          1) As Root:

              yum upgrade -y
              yum groupinstall 'Development Tools' -y
              yum install zlib-devel openssl-devel

          2) As Regular User, Download and Compile Source Code:

              cd /tmp
              wget https://www.python.org/ftp/python/2.7.11/Python-2.7.11.tgz
              tar -zxf Python-2.7.11.tgz
              cd Python-2.7.11
              ./configure --prefix=/opt/
              make
              make install

          3) Install Pip and VirtualEnv

              curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
              python get-pip.py
              pip install virtualenv


Known issues:
-------------
In some cases the candid offline script may fail to collect data from an APIC/DCNM/MSO cluster with an error saying no SSH key pairs could be negotiated.
This is due to recent changes in openssl that deprecate legacy keys.
To temporarily workaround this issue, please edit ~/.ssh/config on your host.

vi ~/ssh/config
Add the following lines to this file.

Press i to insert a line in vi. Move the cursor to the bottom of the file.
Paste the following text in where your-remote-host is either the IP or the FQDN of the APIC/DCNM/MSO:
//
Host your-remote-host
    HostkeyAlgorithms +ssh-dss
//

save the file and exit


Internet Proxy Settings(Optional):
----------------------------------

Appropriate proxy settings can be specified before downloading packages.
Following is an example proxy configuration :
   For Ubuntu apt-get use:
   $ more /etc/apt/apt.conf
      Acquire::http::Proxy "http://${PROXY_SERVER_IP}:8080/";
      Acquire::https::Proxy "https://${PROXY_SERVER_IP}:8080/";

   For CentOS yum use:
   $ more /etc/yum.conf
   proxy=http://${PROXY_SERVER_IP}:8080

   *** for user environment when executing PIP and the candid python script ***
   $ more /etc/environment
       http_proxy="http://${PROXY_SERVER_IP}:8080"
       https_proxy="https://${PROXY_SERVER_IP}:8080"

    ${PROXY_SERVER_IP} - Is the IP address of the internet proxy server

Assumptions for APIC:
--------------------
Cluster healthy(fully fit).
   -- To check cluster health, go to APIC, click on system, controllers. Scroll down to check health
   
All nodes participating in the APIC cluster should have their OOB management address configured.

ToR Models currently supported as part of the script for APIC:
-------------------------------------------------------------

         'N9K-M12PQ'      : MILLER,
         'N9K-C9396PX'    : DONNER,
         'N9K-M6PQ-E'     : DONNER,
         'N9K-M6PQ'       : DONNER,
         'N9K-C93128TX'   : DONNER,
         'N9K-C9396TX'    : DONNER,
         'N9K-C9372PX'    : DONNER,
         'N9K-C9372TX'    : DONNER,
         'N9K-C9332PQ'    : DONNER,
         'N9K-C9372PX-E'  : DONNER,
         'N9K-C93120TX'   : DONNER,
         'N9K-C9372TX-E'  : DONNER,
         'N9K-C93180YC-EX': SUGARBOWL,
         'N9K-93180YC-EX':  SUGARBOWL,
         'N9K-C93108TC-EX': SUGARBOWL,
         'N9K-C93180LC-EX': SUGARBOWL,

Notes: 
-- For spine's we don't have any restriction on model number. We should be able to support collection of all spines in general.
-- For leaves , at this point in time, we support discovery of northstar, Donner, sugarbowl, Homewood, heavenly type of ASIC's.


Assumptions for DCNM:
--------------------
Cluster healthy(fully fit).
   -- To check cluster health, go to DCNM, click on Dashboard and look for the 'Data Center' dashlet.
   
All nodes participating in the DCNM cluster should have their OOB management address configured.

Features to be enabled on all the nodes in Nexus fabric:
-------------------------------------------------------
Required:
--------
feature nxapi

Optional:
--------
feature ospf
feature bgp
feature pim
feature isis
feature interface-vlan
feature vn-segment-vlan-based
feature lacp
feature vpc
feature lldp
feature nv overlay
feature ngoam

Help:
-----
Running Candid data collection script :
--------------------------------------
    usage: cnae_data_collection.py [-h] [-cnaeMode {APIC,STANDALONE,MSO}] -clusterName
                                   CLUSTERNAME [-user USER] [-targetDir TARGETDIR]
                                   [-maxThreads MAXTHREADS]
                                   [-switchVersion SWITCHVERSION]
                                   [-thirdPartyServices THIRDPARTYSERVICES]
                                   [-versionProperties VERSIONPROPERTIES]
                                   [-connectionPreferenceSecondary {outofband,inband}]
                                   [-nat NAT]
    
    script to poll data from APIC/DCNM/LEAFS/MSO
    
    optional arguments:
      -h, --help            show this help message and exit
      -cnaeMode {APIC,STANDALONE,MSO}
                            Specify network topology deployment mode (APIC,
                            Standalone, MSO)
      -clusterName CLUSTERNAME
                            Specify a name for APIC/DCNM/MSO Fabric
      -user USER            Specify user name to login to fabric
      -targetDir TARGETDIR  Specify destination directory to dump data
      -maxThreads MAXTHREADS
                            Specify maximum number of threads for polling
      -switchVersion SWITCHVERSION
                            Switch software version
      -thirdPartyServices THIRDPARTYSERVICES
                            Third-party services configuration file
                            Sample file format:
                                [
                                    {
                                         "type": "netscaler",
                                         "host": "172.31.161.244",
                                         "user": "nsroot",
                                         "queryIds": "NETSCALER_SYSTEM,NETSCALER_NETWORK",
                                         "adminConfig": {
                                            "chassisId": "f5-vxqk-zcvo",
                                            "systemName": "test-system",
                                            "applianceInfo": [
                                                {
                                                    "node": 103,
                                                    "port": 1,
                                                    "epg": "appliance-epg",
                                                    "vrf": "appliance-vrf"
                                                }
                                            ],
                                            "serverInfo": [
                                                {
                                                    "ip": "1.1.1.2",
                                                    "node": 104,
                                                    "port": 1,
                                                    "epg": "server-epg",
                                                    "vrf": "server-vrf"
                                                }
                                            ]
                                         }
                                    }
                                ]
                            - Supported service types : [vcenter, netscaler, f5]
                            - Mandatory parameters    : [type, host, user]
                            - Optional parameters     : [queryIds, adminConfig]
      -versionProperties VERSIONPROPERTIES
                            Specify path to version.properties file
      -connectionPreferenceSecondary {outofband,inband}
                            Specify secondary connection preference for data
                            collection
      -nat NAT              specify a nat configuration json file
                                 {
                                    "natConfiguration":
                                    [
                                      {
                                        "publicIp":"11.11.11.11",
                                        "privateIp":"1.1.1.1"
                                      },{
                                        "publicIp":"22.22.22.22",
                                        "privateIp":"2.2.2.2"
                                      }
                                    ]
                                 }

Output:
------
Collected data is dumped in the destination directory as a tar.gz file

Commands:
---------

Following are the list of REST APIs collected from the fabric

From APIC:
----------
-------------------------------------------------------------
REST API to collect Node (Leaf/Spine/Fabric Card) Information
------------------------------------------------------------
"/api/node/class/eqptFCSlot.json",
"/api/node/class/eqptFC.json",

---------------------------------------------------------
REST API to collect Physical Domain/VLAN Pool information
---------------------------------------------------------
"/api/node/class/physDomP.json?rsp-subtree=full",
"/api/node/class/fvnsVlanInstP.json?rsp-subtree=full",

-----------------------------------------
REST API to collect Selectors information
-----------------------------------------
"/api/node/class/infraAttEntityP.json?rsp-subtree=full",
"/api/node/class/infraAccPortGrp.json?rsp-subtree=full",
"/api/node/class/infraAccBndlGrp.json?rsp-subtree=full",
"/api/node/class/infraAccPortP.json?rsp-subtree=full",
"/api/node/class/infraNodeP.json?rsp-subtree=full",

------------------------------------------
REST API to collect BGP Policy Information
------------------------------------------
"/api/node/class/bgpInstPol.json?rsp-subtree=full",

---------------------------------------------
REST API to collect Tenant Policy Information
---------------------------------------------
"/api/node/class/fvLocale.json",
"/api/node/class/fvTenant.json",
"/api/node/mo/uni/tn-<tenantName>.json?rsp_subtree=full",

---------------------------------------------
Tenant Audit Logs/Fault/Event Log Information
---------------------------------------------
"/api/class/fvTenant.xml?rsp-subtree-include=audit-logs,no-scoped,subtree",
"/api/class/fvTenant.xml?rsp-subtree-include=fault-records,no-scoped,subtree",
"/api/class/fvTenant.xml?rsp-subtree-include=event-logs,no-scoped,subtree",


Following data is collected from leafs & spines:
-------------------------------------------------

-------------------------------
REST API to collect L2 Topology
--------------------------------
'/api/mo/sys/lldp/inst.json?query-target=subtree&target-subtree-class=lldpAdjEp',
'/api/mo/sys.json?query-target=subtree&rsp-subtree-class=eqptIngrTotal5min,eqptEgrTotal5min&rsp-subtree-include=stats,no-scoped',
'/api/mo/sys/ipv4/inst/dom-overlay-1.json',
'/api/mo/sys/vpc/inst.json?query-target=subtree&rsp-subtree=full&query-target=subtree&target-subtree-class=vpcDom',
'/api/mo/sys/isis/inst-default/dom-overlay-1/lvl-l1/db-dtep.json?rsp-subtree=full',

-----------------------------------------
REST API to collect Tenant Routing Config
-----------------------------------------
'/api/mo/sys.json?rsp-subtree=full&rsp-subtree-class=l3Ctx,l3Inst,l2BD,vlanCktEp,l2RsPathDomAtt&query-target=subtree&target-subtree-class=l3Ctx,l3Inst',
'/api/mo/sys/ipv4/inst.json?rsp-subtree=full&rsp-subtree-class=ipv4Dom,ipv4If,ipv4Addr&query-target=subtree&target-subtree-class=ipv4Inst',
'/api/mo/sys/uribv4.json?rsp-subtree=full',
'/api/mo/sys/uribv6.json?rsp-subtree=full',
'/api/mo/sys/ch.json?query-target=children&rsp-subtree=full&rsp-subtree-class=eqptFCSlot,eqptFC&query-target=subtree&target-subtree-class=eqptFCSlot',
'/api/mo/sys.json?rsp-subtree=full&rsp-subtree-class=l2BD,vlanCktEp,vxlanCktEp,epmDb,epmMacEp,epmIpEp&query-target=subtree&target-subtree-class=l3Ctx',
'/api/mo/sys/bgp.json?rsp-subtree=full&rsp-subtree-class=bgpRttP,bgpPeerEntry',
'/api/mo/sys/coop/inst/dom-overlay-1.json?query-target=children',
'/mo/sys/coop/inst/dom-overlay-1/db-ep.json?rsp-subtree=full'
'/api/mo/sys/lldp/inst.json?query-target=subtree&target-subtree-class=lldpAdjEp',

----------------------------------------
REST API to collect Tenant Policy Config
----------------------------------------
"/api/node/class/actrlRule.json",
"/api/node/class/actrlFlt.json",
"/api/node/class/actrlScope.json",
"/api/node/class/actrlEntry.json",


Following are list of VSH(Switch Shell)  commands from Leaf/Spine:
------------------------------------------------------------------

NorthStar based ASIC Commands:
----------------------------------------------------------
VSH commands to collect Tenant Policy Tables for NorthStar
----------------------------------------------------------
vsh_lc -c 'show platform internal ns table  mth_lux_slvy_DHS_AClassKeyTable_memif_data all',
vsh_lc -c 'show platform internal ns table mth_lux_slvy_DHS_AClassDataTable_memif_data all',
vsh_lc -c 'show platform internal ns table mth_lux_slvz_DHS_SecurityGroupKeyTable0_memif_data ingress all'
vsh_lc -c 'show platform internal ns table mth_lux_slvz_DHS_SecurityGroupKeyTable1_memif_data ingress all',
vsh_lc -c 'show platform internal ns table mth_lux_slvy_DHS_SecurityGroupKeyTable2_memif_data ingress all',
vsh_lc -c 'show platform internal ns table mth_lux_slvy_DHS_SecurityGroupKeyTable3_memif_data ingress all',
vsh_lc -c 'show platform internal ns table mth_lux_slvz_DHS_SecurityGroupDataTable0_memif_data ingress all',
vsh_lc -c 'show platform internal ns table mth_lux_slvz_DHS_SecurityGroupDataTable1_memif_data ingress all',
vsh_lc -c 'show platform internal ns table mth_lux_slvy_DHS_SecurityGroupDataTable2_memif_data ingress all',
vsh_lc -c 'show platform internal ns table mth_lux_slvy_DHS_SecurityGroupDataTable3_memif_data ingress all',
vsh_lc -c 'show platform internal ns table mth_lux_slve_DHS_QosMapTable_memif_data ingress all'",
vsh_lc -c 'show system internal aclqos zoning-rules',

Donner based ASIC Commands:
-------------------------------------------------------
VSH commands to collect Tenant Policy Tables for Donner
-------------------------------------------------------
vsh_lc -c 'show platform internal ns table mth_luxh_slvp_DHS_AClassKeyTable_memif_data all,
vsh_lc -c 'show platform internal ns table mth_luxh_slvp_DHS_AClassDataTable_memif_data all',
vsh_lc -c 'show platform internal ns table mth_luxh_slvq_DHS_SecurityGroupKeyTable0_memif_data ingress all',
vsh_lc -c 'show platform internal ns table mth_luxh_slvr_DHS_SecurityGroupKeyTable1_memif_data ingress all',
vsh_lc -c 'show platform internal ns table mth_luxh_slvs_DHS_SecurityGroupKeyTable2_memif_data ingress all',
vsh_lc -c 'show platform internal ns table mth_luxh_slvt_DHS_SecurityGroupKeyTable3_memif_data ingress all',
vsh_lc -c 'show platform internal ns table mth_luxh_slvu_DHS_SecurityGroupDataTable0_memif_data ingress all,
vsh_lc -c 'show platform internal ns table mth_luxh_slvu_DHS_SecurityGroupDataTable1_memif_data ingress all,
vsh_lc -c 'show platform internal ns table mth_luxh_slvv_DHS_SecurityGroupDataTable2_memif_data ingress all,
vsh_lc -c 'show platform internal ns table mth_luxh_slvv_DHS_SecurityGroupDataTable3_memif_data ingress all,
vsh_lc -c 'show platform internal ns table mth_lux_slve_DHS_QosMapTable_memif_data ingress all'",
vsh_lc -c 'show system internal aclqos zoning-rules'",

Sugarbowl based ASIC Commands:
----------------------------------------------------------
VSH commands to collect Tenant Policy Tables for SugarBowl
----------------------------------------------------------
vsh_lc -c 'show platform internal hal objects policy zoningrule',
vsh_lc -c 'show system internal aclqos zoning-rules',
vsh -c 'show ip route vrf all',
vsh_lc -c 'show platform internal sug table tah_sug_lud_qosmaptable all',

---------------------------------------------------
Miscellaneous Commands collected from Switch/Spine:
---------------------------------------------------
date +%Y-%m-%dT%H:%M:%S:%z,
cat /proc/uptime,
vsh_lc -c 'show system internal eltmc info vlan all,
vsh_lc -c 'show forwarding ipv4 route vrf all,


------------------------------------------------------------------
Following are the REST/VSH commands to collect from a Nexus Fabric
------------------------------------------------------------------

REST queries from DCNM Controller:
---------------------------------
'/rest/control/fabrics/{0}/inventory',
'/rest/dcnm-version'

REST queries from a Leaf/Spine:
------------------------------
'/api/class/eqptLC.xml',
'/api/class/eqptFC.xml',
'/api/class/eqptFCSlot.xml',
'/api/mo/sys/vpc.json?rsp-subtree=full',
'/api/mo/sys/pltfm/nve-1.json?rsp-subtree=full',
'/api/mo/sys/lacp.json?rsp-subtree=full&rsp-subtree-class=lacpIf',
'/api/mo/sys/intf.json?query-target=subtree&target-subtree-class=l1PhysIf,ethpmPhysIf',
'/api/mo/sys/intf.json?query-target=subtree&target-subtree-class=pcAggrIf,ethpmAggrIf',
'/api/mo/sys/intf.json?rsp-subtree=full&rsp-subtree-class=sviIf',
'/api/mo/sys/stp.json?query-target=subtree&target-subtree-class=stpIf',
'/api/mo/sys/icmpv4.json?query-target=subtree&target-subtree-class=icmpv4If',
'/api/mo/sys/icmpv6.json?query-target=subtree&target-subtree-class=icmpv6If',
'/api/mo/sys/ipv4.json?rsp-subtree=full&rsp-subtree-class=ipv4Dom,ipv4If,ipv4Addr',
'/api/mo/sys/ipv6.json?rsp-subtree=full&rsp-subtree-class=ipv6Dom,ipv6If,ipv6Addr',
'/api/mo/sys/pim.json?query-target=subtree&target-subtree-class=pimIf',
'/api/mo/sys/ospf.json?query-target=subtree&target-subtree-class=ospfRoute,ospfIf',
'/api/mo/sys/eps.json?rsp-subtree=full',
'/api/mo/sys/hmm.json?rsp-subtree=full',
'/api/mo/sys/hsrp.json?query-target=subtree&target-subtree-class=hsrpIf',
'/api/mo/sys.json?rsp-subtree=full&rsp-subtree-class=l3Ctx,l3Inst,l2BD,vlanCktEp,l2RsPathDomAtt&query-target=subtree&target-subtree-class=l3Ctx,l3Inst',
'/api/mo/sys/bd.json?rsp-subtree=full'
'/api/node/class/l3Inst.json'

VSH commands from a Leaf/Spine:
------------------------------
"show version"
"show mac address-table"
"show vxlan"
"show lldp neighbors system-detail"
"show ip arp vrf <vrf_name>"
"show vpc brief | json"
"show vpc consistency-parameters vlan | json"
"show vpc consistency-parameters global | json"
"show vpc consistency-parameters vni | json"
"show ip ospf neighbors | json"
"show isis adjacency | json"
"show ip pim neighbor | json"
"show ip pim interface | json"
"show port-channel summary | json"

------------------------------------------------------------------
Assumptions for MSO:
--------------------

An MSO fabric will be associated to ACI fabrics. For MSO offline analysis, we need to run 1 iteration of associated ACI's before starting analysis on MSO fabric. 
This will aggregate the events from the associated ACI's during MSO fabric analysis. 

Example for running cnae collection script for MSO: 
   python cnae_data_collection.py -clusterName mso1candid -MSO 10.193.13.176 -targetDir ~/ -user admin -iterations 1 -iterationIntervalSec 10 -cnaeMode MSO      

Limitation: 
----------
During ACI fabric collection, there is a reverse dns lookup (gethostbyaddr) done to get the hostname and stored in TopoNodeEntry Collection. 
This is used while getting the site name from the MSO Controller for the associated ACI's during the MSO analysis. In case of DNS server crash, gethostbyaddr might fail 
and the expected hostname values may not be there in the dns look up field in the collection, resulting in failure of MSO checks. 
Only associated ACI's aggregated events will be present in MSO analysis in this case.  
Ensure the DNS server is in good state if this situation is encountered. 

Note:
-----
Do not have different (APIC) datatsets for same ACI fabric while testing for MSO offline analysis. Example ACI fabric is for candid4 in the first iteration
but running a candid8 dataset in the second iteration for this ACI will cause issues in MSO analysis (ACI epoch will be considered as a missed epoch - no aggregation 
with this fabric will be done). This is because we set the site name for the ACI fabric in the first iteration of MSO analysis. No updates can be done to this site name. 
Ensure the ACI fabric dataset is for the same APIC.

REST API to collect MSO Information:
------------------------------------
'/api/v1/sites',
'/api/v1/sites/fabric-connectivity',
'/api/v1/sites/fabric-connectivity-status',
'/api/v1/schemas',
'/api/v1/tenants',
'/api/v1/policy-report?tenants=<tenants_list_value>'

------------------------------------------------------------------
