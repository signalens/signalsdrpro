### DragonOS
- Download [DragonOS](https://cemaxecuter.com) and install to hard disk

![DragonOS](https://github.com/signalens/signalsdrpro_docs/blob/main/img/gsm/gsm_dragonos.jpeg)

### OpenBTS
- The official version is too old, so here we choose to update it to support Ubuntu 20.04 and 22.04 versions

  - [official version](https://github.com/RangeNetworks/openbts)

  - [changed version](https://github.com/PentHertz/OpenBTS)

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

- Send an MT SMS from the BTS (CLI)

Please review the TMSIs/IMSIs for the attached handsets. 

```
tmsis
```

<img width="1050" height="522" alt="image" src="https://github.com/user-attachments/assets/514a1bb6-4692-4a88-88b7-996099624c48" />

From OpenBTSCLI, you can push a text directly to a given IMSI:

```
OpenBTS> sendsms <IMSI> <src_number> <message text...>
# example:
OpenBTS> sendsms 999700000000001 1 "hi"
```

<img width="1280" height="960" alt="image" src="https://github.com/user-attachments/assets/ddba5bf6-cef5-4c8c-bbc8-3a7771203dbc" />

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
