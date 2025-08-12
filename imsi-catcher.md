### DragonOS
- Download [DragonOS](https://cemaxecuter.com) and install to hard disk

![DragonOS](https://github.com/signalens/signalsdrpro_docs/blob/main/img/gsm/gsm_dragonos.jpeg)

### IMSI-catcher

An IMSI-catcher is a tool or program that displays the IMSI numbers, country, brand, and operator of cellphones in the vicinity. 
It is pre-installed by default in DragonOS, which means you don't need to compile it.

## Instructions

1. To start, open two terminals and use the command `cd /usr/src/IMSI-catcher` in both terminals, which takes you to the installation path of IMSI-catcher in DragonOS.
2. In Terminal one, execute the command below to find IMSIs.:

```
sudo python3 simple_IMSI-catcher.py -s
```

3. In Terminal two, execute the command below to decode GSM signals using the frequency 940.0M, for example:

```
grgsm_livemon -f 940.0M
```

<img width="1491" height="668" alt="image" src="https://github.com/user-attachments/assets/474e86e7-319f-44f0-a921-82bcb9e1cf44" />

You can adjust the gain and frequency from the grgsm_livemon as well. In my case, 45 was the best choice in order to have an output.

4. Now switch to the next terminal, and you can then see IMSIs like the image below (censored on some fields):

<img width="1705" height="612" alt="Screenshot 2025-08-12 215348" src="https://github.com/user-attachments/assets/c13e144d-c34b-469e-8db1-a63fb4e84930" />



