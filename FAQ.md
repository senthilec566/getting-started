
## How to Setup Virtual Environment ?
  Initially clone the concord repository
  
    git clone ssh://git@git.source.akamai.com:7999/concord/concord.git
    cd concord
    
  Start creating virtual environment
  
    ./configure.py --vagrant
    
  If you are trying first it will take more time ( usually ~ 2 Hrs ).

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
