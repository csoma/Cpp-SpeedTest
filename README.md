# Cpp-SpeedTest

Not maintained!

Tests to compare C++ web server speeds.
Written just to get an idea, it is not meant to be scientific.

## Results

Observed performance after several runs on MacBook Pro laptop:

* Mongoose: 9,100 - 11,688 transactions/second (9700 t/s average), 1-2% error rate
* Poco:     7,120 -  9,473 transactions/second (8300 t/s average), no errors

All tested using the same command: `siege -c 100 -r 90 -b http://127.0.0.1:9090/`


## Licenses

* Tests (src folder) - The code is copy&pasted from examples, so it should retain the tested lib's license.
* Mongoose: MIT https://code.google.com/p/mongoose/
* Poco: Boost http://pocoproject.org/license.html

## Other libraries

Tested and ruled out several other libraries.<br> I prefer smaller components and ideally should be production quality on Linux and at least development quality on Mac and Windows.

If you think my assessment below is incorrect, please contact me: csabacsoma at gmail.

* libonion: LGPL v3 and (some) AGPL <br>http://www.coralbits.com/libonion/ <br>Linux only? It does not compile on Mac.
* CppCMS: LGPL v3 and commercial<br> http://cppcms.com/wikipp/en/page/main
* Wt: GNU and commercial<br> http://www.webtoolkit.eu/wt<br> GPL or pay
* Klone: BSD<br> http://www.koanlogic.com/klone/<br> Targets embedded systems and no Mac support
* tntnet: LGPL v3<br>http://www.tntnet.org/<br> [No CMake support](http://www.mail-archive.com/tntnet-general@lists.sourceforge.net/msg00583.html), although [there's an attempt](http://stackoverflow.com/questions/16193343/cmake-with-regarding-generated-files). Just for testing I did not wanted to install cxxtools.
* EasySoap++<br>http://easysoap.sourceforge.net/<br>Last update: April 2002
* gSOAP: GPL and commercial<br>http://www.cs.fsu.edu/~engelen/soap.html<br>GPL or pay.
* ffead-cpp: Apache 2.0<br>https://code.google.com/p/ffead-cpp/<br>Looks promising, but I saw too many "deprecated" and "warning" messages during compile. Maybe in the future.
* staff: Apache 2.0<br>https://code.google.com/p/staff/<br>Based on Apache Axis2/C which has 2009 as "Latest Release" date on their website. <br>[Axis2c-unofficial](https://code.google.com/p/axis2c-unofficial/) does not seem to have [much traction](https://groups.google.com/forum/?fromgroups#!forum/axis2c-unofficial).
* WSO2 Web Services Framework for C++: Apache 2.0 <br>http://wso2.com/products/web-services-framework/cpp/ <br>Axis2/C library (see above and [on SO](http://stackoverflow.com/questions/15748975/how-do-i-compile-wso2-web-services-framework-c-for-64-bit-windows))
* QT <br> Too big
* Boost <br> Too big, too many dependencies
* cpp-netlib: Boost <br> https://github.com/cpp-netlib/cpp-netlib<br> Boost based

Wikipedia: http://en.wikipedia.org/wiki/Comparison_of_web_application_frameworks

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
* Recommended: reduce socket time out to 1sec (see the "run-siege" file)
* From command line start "run-siege"


## Linux

Requirements:

* Package installer: Ubuntu: `apt-get` , CentOS: `yum`
* gcc version 4.7 or later (install instructions below)
* git: `apt-get install git`
* CMake: `apt-get install cmake`
* Siege: `apt-get install siege`

Steps:

* Clone the repository: `git clone https://github.com/csoma/Cpp-SpeedTest.git`
* Run "build-with-gcc": `cd Cpp-SpeedTest; ./build-with-gcc`
* Launch the desired server, for example `bin/Test-Poco`
* Start "run-siege" on a different terminal

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
