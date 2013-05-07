# Cpp-SpeedTest

Not maintained!

Tests to compare C++ web server speeds.
Written just to get an idea, it is not meant to be scientific.

## Results

Ran **siege** on my laptop:

    siege -c 100 -r 90 -b http://127.0.0.1:9090/

Observed performance after several runs:

* Mongoose: 9,100 - 11,688 transactions/second, 1% error rate - mostly around 9700 t/s
* Poco:     7,120 - 9,473 transactions/second, no errors - mostly around 8300 t/s


## Licenses

* Tests (src folder) - The code is copy&pasted from examples, so it should retain the tested lib's license.
* Mongoose: MIT https://code.google.com/p/mongoose/
* Poco: Boost http://pocoproject.org/license.html

## Other libraries

Tested and ruled out several other libraries. I prefer smaller components and ideally should be production quality on Linux, development quality on Mac and Windows.

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
