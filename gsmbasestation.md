### DragonOS
- Download DragonOS from https://cemaxecuter.com and install to hard disk
![DragonOS](https://github.com/signalens/signalsdrpro_docs/blob/main/img/gsm/gsm_dragonos.jpeg)

### OpenBTS
- The official version is too old, so here we choose to update it to support Ubuntu 20.04 and 22.04 versions

  - official version: https://github.com/RangeNetworks/openbts

  - changed version: https://github.com/PentHertz/OpenBTS

```bash
git clone https://github.com/PentHertz/OpenBTS
cd OpenBTS
sudo ./preinstall.sh

./autogen.sh
./configure --with-uhd
make -j$(nproc)
sudo make install
sudo ldconfig

uhd_usrp_probe 

sudo smqueue &
sudo sipauthserve &
sudo /OpenBTS/OpenBTS
```

- Open another terminal
```
/OpenBTS/OpenBTSCLI
```

- Setting up unauthenticated registration
```
config Control.LUR.OpenRegistration .*
```

- Set the mobile network to 2G mode and search for the network

![](https://github.com/signalens/signalsdrpro_docs/blob/main/img/gsm/gsm_phone1.jpeg)
![](https://github.com/signalens/signalsdrpro_docs/blob/main/img/gsm/gsm_phone2.jpeg)

- Close Service
```
fuser -k /usr/local/sbin/sipauthserve
fuser -k /usr/local/sbin/smqueue
```
### osmo-nitb
- DragonOS has osmo-nitb installed by default
```bash
cd /usr/src/osmo-nitb-scripts
vi config.json
```
- Change enabled of call to true so that you can make calls

![](https://github.com/signalens/signalsdrpro_docs/blob/main/img/gsm/gsm_config.jpeg)

```
./main_uhd.py -h uhd --sip
```

- Open another terminal
```
uhd_usrp_probe
/usr/bin/osmo-trx-uhd -C /etc/osmocom/osmo-trx-uhd.cfg
```

- Mobile phone can access the network

![](https://github.com/signalens/signalsdrpro_docs/blob/main/img/gsm/gsm_phone3.jpeg)

- After the mobile phone is connected to the network, the record can be seen on the terminal, as well as the assigned mobile phone number
![](https://github.com/signalens/signalsdrpro_docs/blob/main/img/gsm/gsm_state.jpeg)

- Based on the assigned phone number, you can make a call

![](https://github.com/signalens/signalsdrpro_docs/blob/main/img/gsm/gsm_phone4.jpeg)
