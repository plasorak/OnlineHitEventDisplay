# OnlineHitEventDisplay
A little app that uses PTMP and ROOT to display hits

To compile:
```source setupDUNEARTDAQ_forBuilding
git clone git@github.com:plasorak/OnlineHitEventDisplay.git
cd OnlineHitEventDisplay
mkdir build
cmake ../
make```

To run:
`./build/OnlineHitEventDisplay --socket --socket-type --refresh-rate`
