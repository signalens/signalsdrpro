### GPS Simulator

- download gps-sdr-sim

```bash
git clone https://github.com/osqzss/gps-sdr-sim.git
cd gps-sdr-sim
gcc gpssim.c -lm -O3 -o gps-sdr-sim
```

- download brdc

https://cddis.nasa.gov/archive/gnss/data/daily/

- make gps signal file

``` bash
# make static singal file
gps-sdr-sim -e brdc3300.24n -l 27.988499, 86.925082,8848

# make dynamic signal file, latitude and longitude are written in the file
gps-sdr-sim -e brdc3300.24n -g triumphv3.txt
gps-sdr-sim -e brdc3300.24n -x circle_llh.csv
gps-sdr-sim -e brdc3300.24n -u circle.csv
```

- send gps signal used b210 mode

```bash
gps-sdr-sim-uhd.py -t gpssim.bin -s 2500000 -x 10
```
	
- send gps signal used plutosdr mode

``` bash
plutoplayer -t gpssim.bin -a -10.0

```

- Use GPS Test Plus to test

tips: clearing the agps information will result in faster positioning
![GPS Test Plus](https://github.com/signalens/signalsdrpro_docs/blob/main/img/gpssim/gpssim.jpeg)