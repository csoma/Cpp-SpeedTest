# Cpp-SpeedTest

Small tests to compare C/C++ web server speeds just to get an idea; it is not meant to be scientific.

<!--

## Capabilities

TBA

| Library  | Linux | Mac | Win | (1) | (2) | (3) | (4) | (5) | (6) |
|----------|-------|-----|-----|-----|-----|-----|-----|-----|-----|
| CivetWeb | Yes   | Yes | Yes |     |     |     |     |     | Yes |
| Mongoose |       |     |     |     |     |     |     |     |     |
| Poco     | Yes   | Yes | Yes |     |     |     | Yes |     |     |
| Nginx    |       |     |     |     | Yes | Yes | Yes | Yes |     |
|          | Linux | Mac | Win | (1) | (2) | (3) | (4) | (5) | (6) |

1. HTTP/1.0 support
2. HTTP/1.1 support
3. Keep alive
4. Transfer-Encoding: Chunked - needed for larger file transfer behind ngnix
5. Cache control
6. WebSocket

Note: I assume the server will be behind Nginx or similar which handles SPDY, compression and encryption.

-->

## Speed test results

Tested some of the servers on a Linux.
Previous tests ran under MacOS X almost an order of magnitude slower. It seems that most of the web servers don't implement kqueue (the epoll equivalent on Mac).

Transaction/second results, larger is better:

| Library                                         | License |     Min |     Max | Average | Notes              |
|-------------------------------------------------|---------|--------:|--------:|--------:|--------------------|
| [CivetWeb](https://github.com/bel2125/civetweb) | MIT     |  21,428 |  27,272 |  25,714 | C, No errors       |
| [Mongoose](https://github.com/cesanta/mongoose) | GPL v2  |  21,428 |  27,272 |  25,714 | C, No errors       |
| nginx (stock)                                   | 2c BSD  |  16,981 |  23,076 |  20,000 | C, No errors       |
| CivetWeb C++ wrap                               | MIT     |   9,473 |  13,235 |  12,500 | C++, No errors     |
| [POCO](https://github.com/pocoproject/poco)     | Boost   |   5,769 |  14,285 |   6,716 | C++, No errors     |

### Command

Ran 40 times:

```
siege -q -b -c 100 -r 90 --log=/dev/null http://127.0.0.1:8080/
```

### Test apps

- CivetWeb C: hello.c on `http://127.0.0.1:8080/`
- CivetWeb C++: embedded_cpp.cpp on `http://127.0.0.1:8081/example`
- Mongoose: hello_world.c on `http://127.0.0.1:8080/`
- [POCO](http://pocoproject.org/slides/200-Network.pdf): HTTPTimeServer.cpp on `http://127.0.0.1:9980/`
```
add-apt-repository ppa:george-edison55/cmake-3.x
apt-get update
apt-get upgrade cmake
./build_cmake.sh -DENABLE_TESTS=ON ..
cmake-build/bin/HTTPLoadTest
```
- NGinx: first page after disabling log and gzip on `http://127.0.0.1/`

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

## See also

* Wikipedia: http://en.wikipedia.org/wiki/Comparison_of_web_application_frameworks
* C++ web frameworks (read the comments too): http://www.baryudin.com/articles/software/c-plus-plus-web-development-framework.html
* TechEmpower code: https://github.com/TechEmpower/FrameworkBenchmarks/tree/master/frameworks/C%2B%2B
* TechEmpower results: http://www.techempower.com/benchmarks/
