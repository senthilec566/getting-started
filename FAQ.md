
## How to Setup Dev Virtual Environment ?
  First clone the concord repository
  
    git clone https://github.com/concord/getting-started.git
    cd concord
    
  Start creating virtual environment
  
    ./configure.py --vagrant
    
  If you are trying first time it will take more time ( usually ~ 2 Hrs ).

## How to take action on intermittent issue like below ?

    INFO interface: info: fatal: [localhost]: FAILED! => {"changed": false, "dest": "/vagrant/meta/.bolt/clang+llvm-3.9.0-x86_64-linux-gnu-ubuntu-16.04.tar.xz", "failed": true, "msg": "Connection failure: timed out", "state": "absent", "url": "http://releases.llvm.org/3.9.0/clang+llvm-3.9.0-x86_64-linux-gnu-ubuntu-16.04.tar.xz"}

 Vagrant get stucks and it has internal race conditions which a long setup seems to trigger.
 The work around is to simply do 
 
 
   ```vagrant up && vagrant ssh```
   
 Then once in the machine do
   
      cd /vagrant/meta
      ./ansible-playbook.pex playbooks/devbox_all.yml
    
 That's all that should do it.

## How to correct dpkg was interrupted error ?
    TASK: [concord-demo | Install apt packages] ***********************************
    failed: [default] => (item=git,curl,bzip2,libntl0,libmpfr4,libssl1.0.0,libgflags2,libboost-thread1.55.0,libboost-regex1.55.0,libboost-program-options1.55.0,libboost-system1.55.0,libboost-filesystem1.55.0,libboost-date-time1.55.0,libboost-iostreams1.55.0,libevent-dev,libunwind8,libdouble-conversion1,liblz4-1,liblzma5,libsnappy1,libjemalloc1,libgoogle-glog-dev,zlib1g,libbz2-1.0,libarchive13,libcurl3-nss,libsvn1,libsasl2-2,libapr1,libasan2,lttng-tools,liblttng-ust0,zookeeperd,sbt,python-setuptools,ca-certificates,ruby2.0,ruby2.0-dev,graphviz,g++-5,g++-4.9,wamerican,build-essential,cmake,mesos=0.28.0-2.0.16.ubuntu1404) => {"failed": true, "item": "git,curl,bzip2,libntl0,libmpfr4,libssl1.0.0,libgflags2,libboost-thread1.55.0,libboost-regex1.55.0,libboost-program-options1.55.0,libboost-system1.55.0,libboost-filesystem1.55.0,libboost-date-time1.55.0,libboost-iostreams1.55.0,libevent-dev,libunwind8,libdouble-conversion1,liblz4-1,liblzma5,libsnappy1,libjemalloc1,libgoogle-glog-dev,zlib1g,libbz2-1.0,libarchive13,libcurl3-nss,libsvn1,libsasl2-2,libapr1,libasan2,lttng-tools,liblttng-ust0,zookeeperd,sbt,python-setuptools,ca-certificates,ruby2.0,ruby2.0-dev,graphviz,g++-5,g++-4.9,wamerican,build-essential,cmake,mesos=0.28.0-2.0.16.ubuntu1404"}
    stderr: E: dpkg was interrupted, you must manually run 'sudo dpkg --configure -a' to correct the problem.

    msg: '/usr/bin/apt-get -y -o "Dpkg::Options::=--force-confdef" -o "Dpkg::Options::=--force-confold"   install 'libntl0' 'libevent-dev' 'libsnappy1' 'libgoogle-glog-dev' 'libcurl3-nss' 'lttng-tools' 'zookeeperd' 'sbt' 'python-setuptools' 'ruby2.0' 'ruby2.0-dev' 'wamerican' 'mesos=0.28.0-2.0.16.ubuntu1404'' failed: E: dpkg was interrupted, you must manually run 'sudo dpkg --configure -a' to correct the problem.

Execute below commands

      vagrant ssh
      sudo dpkg --configure -a
## How to run Word Count Program in dev environment ?
    vagrant ssh
    cd /vagrant/py/
    vagrant@vagrant-ubuntu-trusty-64:/vagrant/py$ concord deploy word_counter.json
    INFO:cmd.deploy:Adding tarfile: word_counter.py
    INFO:cmd.deploy:Adding tarfile: runner.bash
    INFO:cmd.deploy:Size of tar file is: 663.0B
    INFO:cmd.deploy:Thrift Request: {
    "cpus": 1.0, 
    "disk": 2048, 
    "executorArgs": [], 
    "forcePullContainer": true, 
    "forceUpdateBinary": true, 
    "instances": 1, 
    "mem": 512, 
    "name": "word-counter", 
    "slug": null, 
    "taskHelper": {
        "client": null, 
        "clientArguments": [
            "word_counter.py"
        ], 
        "computationAliasName": null, 
        "dockerContainer": "", 
        "environmentExtra": [], 
        "execName": "runner.bash", 
        "folder": "", 
        "frameworkLoggingLevel": 0, 
        "frameworkVModule": "", 
        "proxy": null, 
        "retries": 0, 
        "router": null, 
        "scheduler": null, 
        "user": ""
    }
    }
    INFO:cmd.thrift_utils:Connecting to:localhost:2181
    INFO:cmd.deploy:Sending computation to: 127.0.0.1:11211
    INFO:cmd.deploy:Verify with the mesos host: 127.0.0.1 that the service is running
    vagrant@vagrant-ubuntu-trusty-64:/vagrant/py$
    
  Reason for the above problem could be more than one VM running in machine and resulted in port issue(s).
   
## Listing Vagrant VMs in your Local Machine
    vagrant global-status 

## Killing all running Vagrant VMs
    for i in `vagrant global-status | grep virtualbox | awk '{ print $1 }'` ; do vagrant destroy $i ; done
 
