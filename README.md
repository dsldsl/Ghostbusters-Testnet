# Ghostbusters Testnet Instructions

### 0. Wireguard VPN Setup

***Should we put this first?***
```console
sudo add-apt-repository ppa:wireguard/wireguard
sudo apt-get update
sudo apt-get install wireguard

umask 077
wg genkey | tee privatekey | wg pubkey > publickey
echo -e "[Interface]\nPrivateKey = $(cat privatekey)\nSaveConfig = true\nDNS = 1.1.1.1" > ghostbusters.conf
echo -e "ListenPort = 5555" >> ghostbusters.conf
echo -e "Address = 192.168.10.X/24" >> ghostbusters.conf
sudo cp ghostbusters.conf /etc/wireguard/.
sudo nano /etc/wireguard/ghostbusters.conf
# Add your trusted vpn peers by repeating the line below for each peer

[Peer]
PublicKey = <peer-public-key>
AllowedIPs = 192.168.10.Y/32
Endpoint = <peer-public-endpoint>:<peer-vpn-port>

# Save the file
# Start wireguard
sudo wg-quick up ghostbusters
# Test configuration
sudo wg show ghostbusters
```

### 1. Setup

`cd` to your `opt` folder.

```console
mkdir Ghostbusters
cd Ghostbusters
wget https://raw.githubusercontent.com/jchung00/Ghostbusters-Testnet/master/installGhostbusters.sh
```

### 2. Fill out info in install script

Edit the following information in the file:

```console
nano installGhostbusters.sh
```

***Will make a screenshot with inputs that need to be inserted in highlights***

### 3. Run the script

```console
sudo chmod u+x installGhostbusters.sh
./installGhostbusters.sh
```

### 4. Publishing BP info on Keybase

- Install keybase: https://keybase.io/docs/the_app/install_linux
 Ubuntu instructions
 ```console
# Install curl if required
sudo apt install curl

curl -O https://prerelease.keybase.io/keybase_amd64.deb
# if you see an error about missing `libappindicator1`
# from the next command, you can ignore it, as the
# subsequent command corrects it
sudo dpkg -i keybase_amd64.deb
sudo apt-get install -f
run_keybase
 ```
 - Login or signup:
 ```console
 # Login
 keybase login
 # Sign up
 keybase signup
 ```
 - Save on KBFS:
 ```console
 cd /keybase/public/username
 curl -O https://raw.githubusercontent.com/eosrio/bp-info-standard/master/bp_info_sample.json > bp_info.json
 nano bp_info.json
 # add your bp info and save it!
 ```
 - Verify file on `https://[username].keybase.pub/bp_info.json`
 ***Need to add instructions for verification on command line.***

### 5. Check scripts

`cd` into your Ghostbusters testnet folder, which was created from the install script.
Try `cat config.ini`, and `cat cleos.sh` to check that all the information is correct.

### 6. Add peers

***Still need to define this step better with the web of trust idea. Should refer to launch status spreadsheet. Merge with step 0?***

##### 6.1 Bios Node

If you were selected as the bios node, please follow instructions [here](https://github.com/jchung00/Ghostbusters-Testnet/blob/master/bios-instructions.md)

### 7. Receive genesis file

***Need directions for this***

### 8. Sync

```console
./start.sh
tail -F stderr.txt
```
