# Cpp-SpeedTest

Not maintained!

Short test to compare C++ web server speeds.
Written just to get an idea, it is not meant to be scientific.

## Results

Throughput after several runs on MacBook Pro laptop (transactions/second - larger is better):

| Lib | Min | Max | Average | Notes |
|-----|----:|----:|--------:|-------|
| Mongoose | 9,100 | 11,688  | 9,700 | 1-2% error rate |
| Poco     | 7,120 |  9,473  | 8,300 | no errors |
| nginx    | 7,142 | 11,843  | 8,700 | no errors (4 workers, no optimization) |
| [Kore](https://github.com/jorisvink/kore/issues/33) https | 357 | 357 | 357 | no errors |
| Kore http | 4,326 | 11,588 | 9,300 | no errors (unsupported http mode) |
| [CivetWeb](https://github.com/bel2125/civetweb) | 951 | 5,625 | 4,455 | cpp, no errors |
| CivetWeb | 1,060 | 11,688 | 5,882 | c, no errors |

All tested using the same command: `siege -c 100 -r 90 -b http://127.0.0.1:9090/`

## Licenses

* Tests (src folder) - The code is copy&pasted from examples, so it should retain the tested lib's license.
* Mongoose: MIT https://code.google.com/p/mongoose/
* Poco: Boost http://pocoproject.org/license.html

## Other libraries

(In no particular order)

Tested and ruled out several other libraries / frameworks.

I'm looking for smaller free components and ideally should be production quality on Linux and at least development quality on Mac and Windows.

If you think my assessment below is incorrect, please contact me: csabacsoma at gmail.

### libasyncd and qhttpd

BSD? <br>
https://github.com/wolkykim/libasyncd <br>
https://github.com/wolkykim/qhttpd <br>
Looks promising, about the same speed as CivetWeb. Based on libevent-dev. Linux only.

### whisperlib

BSD? <br>
https://github.com/cpopescu/whisperlib <br>
Around 1,500 transaction/second. Possibly Linux only.

### Facebook Proxygen

BSD + [Facebook blanket patent protection](https://github.com/facebook/proxygen/blob/master/PATENTS) (see [discussion](https://news.ycombinator.com/item?id=8985541)) <br>
https://github.com/facebook/proxygen <br>
Depends on Boost 1.51+, FB thrift and a long slew of other libraries (I counted 86).

### libonion

LGPL v3 and (some) AGPL<br>
http://www.coralbits.com/libonion/<br>
Linux only? It does not compile on Mac.

### CppCMS

LGPL v3 and commercial<br>
http://cppcms.com/wikipp/en/page/main

### Wt

GPL and commercial<br>
http://www.webtoolkit.eu/wt

### Klone

BSD<br>
http://www.koanlogic.com/klone/<br>
Targets embedded systems, no Mac support

### tntnet

LGPL v3<br>
http://www.tntnet.org/<br>
[No CMake support](http://www.mail-archive.com/tntnet-general@lists.sourceforge.net/msg00583.html), although [there's an attempt](http://stackoverflow.com/questions/16193343/cmake-with-regarding-generated-files). Just for testing I did not wanted to install cxxtools.

### Libmicrohttpd

LGPL v2.1 <br>
http://www.gnu.org/software/libmicrohttpd/

### EasySoap++

http://easysoap.sourceforge.net/<br>
Last update: April 2002

### gSOAP

GPL and commercial<br>
http://www.cs.fsu.edu/~engelen/soap.html

### ffead-cpp

Apache 2.0<br>
Old: https://code.google.com/p/ffead-cpp/<br>
https://github.com/sumeetchhetri/ffead-cpp <br>
Many "deprecated" and "warning" messages during compile. According to readme it is based on POCO.

### staff

Apache 2.0<br>
https://code.google.com/p/staff/<br>
Based on Apache Axis2/C which has 2009 as "Latest Release" date on their website. <br>
[Axis2c-unofficial](https://code.google.com/p/axis2c-unofficial/) does not seem to have [much traction](https://groups.google.com/forum/?fromgroups#!forum/axis2c-unofficial).

### WSO2 Web Services Framework

Apache 2.0 <br>
http://wso2.com/products/web-services-framework/cpp/ <br>
Axis2/C library (see above and [on SO](http://stackoverflow.com/questions/15748975/how-do-i-compile-wso2-web-services-framework-c-for-64-bit-windows))

### QT

Large framework

### Boost

Large framework

### cpp-netlib

Boost <br>
https://github.com/cpp-netlib/cpp-netlib<br>
Boost based

### seasocks

BSD <br>
https://github.com/mattgodbolt/seasocks <br>
Runs on Linux only: epoll based (Mac has kqueue instead)<br>
On Linux: [1,198.27 trans/sec vs Kore's 11,688.31 trans/sec](https://github.com/mattgodbolt/seasocks/issues/2#issuecomment-105928113)

### ACE + TAO + JAWS

License: ? <br>
https://github.com/cflowe/ACE <br>
Last commit: Oct 14, 2013

### ngnix module

Since NGINX doesn't have a shared module system, it will have to be recompiled every time we want to run the module.
Also it ties the app to ngnix.

### CPoll CPP

GPL <br>
http://sourceforge.net/projects/cpollcppsp/


### Ulib

GPL <br>
https://github.com/stefanocasazza/ULib

### Treefrog

BSD <br>
https://github.com/treefrogframework/treefrog-framework <br>
Speed: [18% of ulib](https://www.techempower.com/benchmarks/)

### ppC++

GPL <br>
http://sourceforge.net/projects/ppcpp/ <br>
Last Update: 2013-03-19


### CSP

? <br>
http://www.micronovae.com/CSP.html <br>
Abandoned

### libhttpserver

GPL <br>
https://github.com/etr/libhttpserver <br>

# Running the tests


## Mac OS X

Requirements:

* Recommended: [Homebrew](http://mxcl.github.io/homebrew/)
* XCode 4.5+ with command line tools (Preferences / Downloads / Components - Install)
* git: `brew install git` or [GitHub for Mac](http://mac.github.com/)
* CMake: `brew install cmake`
* Siege: `brew install siege`

Steps:

* Clone the repository: `git clone https://github.com/csoma/Cpp-SpeedTest.git`
* Run "start-XCode": `cd Cpp-SpeedTest; ./start-XCode`
* Select the desired target (Test-Poco, Test-Mongoose)
* Click Run
* Recommended: reduce socket time-out to 1sec (see the "run-siege" file)
* From command line start "run-siege"

## Linux

Requirements:

* Package installer: Ubuntu: `apt-get`, CentOS/RedHat: `yum`
* gcc version 4.7 or later (install instructions below)
* git: `apt-get install git`
* CMake: `apt-get install cmake`
* Siege: `apt-get install siege`

Steps:

* Clone the repository: `git clone https://github.com/csoma/Cpp-SpeedTest.git`
* Run "build-with-gcc": `cd Cpp-SpeedTest; ./build-with-gcc`
* Launch the desired server, for example `bin/Test-Poco`
* Start "run-siege" on a different terminal

### For old Linux distributions

Installing GCC 4.7 or later for C++11 features:

* Install GCC: <code>apt-get install cmake make gcc g++ libc6-dev libc-dev-bin libpq-dev</code>
* Check installed version: `gcc --version`
* When needed, install [gcc-snapshot](http://askubuntu.com/questions/61254/how-to-update-gcc-to-the-latest-versionin-this-case-4-7-in-ubuntu-10-04) for a newer version of gcc: <pre>
sudo apt-get install gcc-snapshot
cd /usr/bin
mv gcc gcc-old
mv c++ c++-old
mv cpp cpp-old
ln -s /usr/lib/gcc-snapshot/bin/gcc /usr/bin/gcc
ln -s /usr/lib/gcc-snapshot/bin/c++ /usr/bin/c++
ln -s /usr/lib/gcc-snapshot/bin/cpp /usr/bin/cpp</pre>
   * You might need to [upgrade Linux](http://www.unixmen.com/how-to-upgrade-from-ubuntu-1004-1010-1104-to-ubuntu-1110-oneiric-ocelot-desktop-a-server/) if you have older than Ubuntu 12.04
   * More details about C++11 support in gcc: http://kjellkod.wordpress.com/2012/06/02/g2log-now-with-mainstream-c11-implementation/#more-589

# See also

* Wikipedia: http://en.wikipedia.org/wiki/Comparison_of_web_application_frameworks
* C++ web frameworks (read the comments too): http://www.baryudin.com/articles/software/c-plus-plus-web-development-framework.html
* TechEmpower code: https://github.com/TechEmpower/FrameworkBenchmarks/tree/master/frameworks/C%2B%2B
* TechEmpower results: http://www.techempower.com/benchmarks/
