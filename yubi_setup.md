# YubiHSM setup

## Downloading and extract packages

### Download and extract tarbell
```
wget https://developers.yubico.com/YubiHSM2/Releases/yubihsm2-sdk-2024-09-debian11-amd64.tar.gz
tar -xzf yubihsm2-sdk-2024-09-debian11-amd64.tar.gz
```

### Installed required packages for pkcs11
1. Install base libraries first:
```
sudo dpkg -i libyubihsm1_2.6.0_amd64.deb
sudo dpkg -i libykhsmauth1_2.6.0_amd64.deb
sudo dpkg -i libyubihsm-usb1_2.6.0_amd64.deb
sudo dpkg -i libyubihsm-http1_2.6.0_amd64.deb
```

2. Install development packages:
```
sudo dpkg -i libyubihsm-dev_2.6.0_amd64.deb
sudo dpkg -i libykhsmauth-dev_2.6.0_amd64.deb
```

3. Install tools and applications:
```
sudo dpkg -i yubihsm-connector_3.0.5-1_amd64.deb
sudo dpkg -i yubihsm-setup_2.3.2-1_amd64.deb
sudo dpkg -i yubihsm-auth_2.6.0_amd64.deb
sudo dpkg -i yubihsm-pkcs11_2.6.0_amd64.deb
sudo dpkg -i yubihsm-shell_2.6.0_amd64.deb
sudo dpkg -i yubihsm-wrap_2.6.0_amd64.deb
```

## Reset YubiHSM back to factory settings

While inserting the YubiHSM 2 into a USB port, press the metal rim as you insert it and continue to press the rim for a minimum of 10 seconds.

## Connect to yubihsm over network
```
sudo yubihsm-connector &
```

## Create new authentication key

Create new authentication key from command line:
```
yubihsm-shell -a put-authentication-key -i <object_id> -l ApricotHSM2 -d 1 -c get-pseudo-random --new-password <password>
```

&nbsp;where:
```
put-authentication-key is the command to create a new authentication key (https://docs.yubico.com/hardware/yubihsm-2/hsm-2-user-guide/hsm2-sdk-tools-libraries.html -> Put Authentication Key)
<ObjectID> is the ObjectID of the new authentication key (https://docs.yubico.com/hardware/yubihsm-2/hsm-2-user-guide/hsm2-core-concepts.html#object-id).
ApricotHSM2 is the label of the new authentication key.
1 is the domain where the new authentication key will perate within (https://docs.yubico.com/hardware/yubihsm-2/hsm-2-user-guide/hsm2-core-concepts.html#domain)
get-pseudo-random is the capability for the new authentication key (https://docs.yubico.com/hardware/yubihsm-2/hsm-2-user-guide/hsm2-core-concepts.html#capability)
<password> is the password used to derive the new authentication key. This is the password you specify when opening a session with the YubiHSM using this authentication key.
```

Enter the default password (password) when prompted 

## Delete default authentication key
```
yubihsm-shell -a delete-object -i 1 -t authentication-key
```
Enter the default password (password) when prompted


## Test generation of entropy using newly created authentication key
```
sudo pkcs11-tool --module /usr/lib/x86_64-linux-gnu/pkcs11/yubihsm_pkcs11.so -l -p <object_id in hex><password> --generate-random 32 --output random_data.bin
```
note: need a yubihsm_pkcs11.conf file for the above command to work
