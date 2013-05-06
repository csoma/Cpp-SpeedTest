# Cpp-SpeedTest

Not maintained!

Tests to compare C++ web server speeds.
Non-scientific, just to get an idea.


## Results

Ran **siege** on my laptop:

    siege -c 100 -r 90 -b http://127.0.0.1:9090/

Observed performance:

* Mongoose: 9,100 - 11,688 transactions/second, 1% error rate - mostly around 9700 t/s
* Poco:     7,120 - 9,473 transactions/second, no errors - mostly around 8300 t/s
