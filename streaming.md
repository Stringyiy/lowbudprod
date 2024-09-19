## Linux Setup

### Hardware Components

### Linux Server

sudo adduser pi

sudo usermod -aG ssh pi

sudo vi /etc/ssh/sshd_config

   ```plaintext
PasswordAuthentication no
PermitRootLogin no
PubkeyAuthentication yes
AuthorizedKeysFile     %h/.ssh/authorized_keys
   ```

sudo usermod -s /bin/rbash pi

echo "your_public_key" >> /home/pi/.ssh/authorized_keys

sudo systemctl restart sshd

### Linux Client
#### Firmware Update
```bash
sudo raspi-config
#    2. In the pop-up window, choose 'Interface Options'-> 'SSH'.
#    3. Select'Yes'
#    4. Enable the SSH service.
mkdir ~/.ssh
touch ~/.ssh/authorized_keys
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
echo "your_public_key" >> /home/pi/.ssh/authorized_keys
sudo vim /etc/ssh/sshd_config
```

```plaintext
# use sed 
PasswordAuthentication no
PermitRootLogin no
AllowTcpForwarding yes
X11Forwarding no
PermitTunnel yes
PubkeyAuthentication yes
AuthorizedKeysFile     %h/.ssh/authorized_keys
```

```
ssh-keygen -t ed25519 -C 'yi@pi'
scp pi:/home/yi/.ssh/id_ed25519.pub ~/iCloud/Developer/id_ed25519.pub #ip
scp ~/iCloud/Developer/id_ed25519.pub aws:/home/admin/.ssh/id_ed25519.pub #ip
cat ~/.ssh/id_ed25519.pub >> ~/.ssh/authorized_keys #server
sudo systemctl restart sshd #server

ssh -D 1080 -f -C -q -N admin@13.231.39.140
export ALL_PROXY=socks5://localhost:1080

vcgencmd bootloader_version
sudo apt update 
sudo apt full-upgrade
sudo apt autoremove
sudo reboot

ssh -D 8080 admin@13.231.39.140
sudo raspi-config
#   Then select 6 Advanced Opitions => A5 Bootloader Version => E1 Latest, answere Yes）
sudo reboot

ssh -D 8080 admin@13.231.39.140
sudo rpi-eeprom-update
vcgencmd bootloader_version
sudo reboot
```

#### pi backup
lsblk
sudo umount /dev/sdb1
sudo dd bs=4M if=/dev/mmcblk0 of=/dev/sda status=progress
sync

#### Audio Set Up
[IQAudio Codec Zero on Pi5](https://github.com/raspberrypi/Pi-Codec/issues/9)

curl -O 'https://github.com/raspberrypi/Pi-Codec/blob/master/Codec_Zero_AUXIN_record_and_HP_playback.state'
sudo alsactl restore -f ./Codec_Zero_AUXIN_record_and_HP_playback.state

                
#### Install OBS on Linux

```
# optional
python -m venv ~/stream
source ~/stream/bin/activate
pip install -r requirements.txt
```

```
sudo add-apt-repository ppa:obsproject/obs-studio
sudo apt update
sudo apt install obs-studio
```

#### Screen Broadcasting
```
curl -O https://github.com/DistroAV/DistroAV/blob/master/CI/libndi-get.sh
./libndi-get.sh
sudo apt install avahi-daemon ffmpeg
sudo systemctl enable avahi-daemon
sudo systemctl start avahi-daemon
sudo ufw allow 5353/udp
sudo ufw allow 5959:5969/tcp
sudo ufw allow 5959:5969/udp
sudo ufw allow 6960:6970/tcp
sudo ufw allow 6960:6970/udp
sudo ufw allow 7960:7970/tcp
sudo ufw allow 7960:7970/udp
sudo ufw allow 5960/tcp
curl -O https://github.com/DistroAV/DistroAV/releases/download/4.14.1/obs-ndi-4.14.1-x86_64-linux-gnu.deb
sudo dpkg -i obs-ndi-4.14.1-x86_64-linux-gnu.deb
'''
sudo ln -s /usr/lib/x86_64-linux-gnu/obs-plugins/obs-ndi.so /usr/local/lib/obs-plugins/obs-ndi.so
sudo ln -s /usr/share/obs/obs-plugins/obs-ndi/ /usr/local/share/obs/obs-plugins/obs-ndi
'''

```

obs-websocket obs-cli obs-distroAV
display capture
Wayland Screen Capture (PipeWire)
NDI 6 out

#### Interface
Consider using an individual AP to handle wireless audio video streams  
  
OpenWRT  
RaspAP  
  
RaspAP is a web interface that allows you to easily manage a Raspberry Pi as a Wi-Fi access point and router. If you want to set it up for LAN connections, follow these steps:  

### Prerequisites
1. **Raspberry Pi**: Ensure you have a Raspberry Pi with Raspbian installed.
2. **Internet Connection**: Your Raspberry Pi should be connected to the internet via Ethernet for WAN (Wide Area Network) connect
ions.
3. **SSH Access**: Enable SSH on your Raspberry Pi for easier access.

### Installation Steps
1. **Update your Raspberry Pi**:
   ```bash
   sudo apt update
   sudo apt upgrade -y
   ```

2. **Install RaspAP**:
   - Open a terminal and run the following command:
     ```bash
     curl -sL https://install.raspap.com | bash
     ```
   - This will install RaspAP along with the necessary dependencies.

3. **Configure RaspAP**:
   - After installation, you can access the RaspAP web interface by navigating to `http://10.3.141.1` (default IP of the RaspAP).
   - The default login credentials are:
     - Username: `admin`
     - Password: `secret`

4. **Network Configuration**:
   - **LAN Network Configuration**: In the RaspAP interface, navigate to the DHCP and DNS settings to set up the DHCP server to as
sign IPs to devices connected to the Raspberry Pi.
   - **Wi-Fi Network Configuration**: Configure the Wi-Fi settings to set up an access point for clients.

5. **Test the Setup**:
   - Connect another device (like a phone or laptop) to the Wi-Fi created by RaspAP. Ensure it can access the internet and the loc
al network.

### Additional Configuration (Optional)
- **IP Address**: You may need to configure the static IP address for the Raspberry Pi if you don't want it to change.
- **Firewall**: Consider setting up a firewall for extra security.

### Access Through LAN
To access your Raspberry Pi through LAN, you can also find its IP address via the Router's DHCP list and connect to it.

### Further Reading
For more detailed instructions and advanced configurations, refer to the official [RaspAP documentation](https://docs.raspap.com/)
.

### Sourcing
RTMP (Streaming Platform), RTSP (IP Cam), NDI (NewTek), UVC (Capture Card/WebCam)

iOS:
Obsbot Live (RTMP out)
Topdirector (RTSP/NDI in, RTMP/NDI out)
Camo Studio (UVC in, RTMP out)

Android (refurb) :  
  
workload asc: UVC, NDI

PC:
OBS Studio + DistroAV

###Preprocessing  

####Voice Production and TTS  

'''
git clone https://github.com/pbanuru/xtts2-ui.git
cd xtts2-ui
python -m venv venv
source venv/bin/activate
pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu
pip install -r requirements.txt
pip install --upgrade TTS
'''

python app.py OR streamlit run app2.py OR python appTerminal.py

The dataset consists of a single folder named targets, pre-populated with several voices for testing purposes.

To add more voices (if you don't want to go through the GUI), create a 24KHz WAV file of approximately 10 seconds and place it under the targets folder. You can use yt-dlp to download a voice from YouTube for cloning:

yt-dlp -x --audio-format wav "https://www.youtube.com/watch?"

use ffmpeg to convert and slice your own training data.

'''
ffmpeg -i voiceout.aac -ar 24000 train.wav
ffmpeg -i train.wav -f segment -segment_time 10 -c copy output%03d.wav
'''

###Mixing

### Streaming
#### Chatroom Push Notification

### RealTime Processing

### Ref

[NDI](https://interfacinglinux.com/2024/08/15/ndi-6-on-linux-with-obs/)

### Interesting Editing Projects on Github
[RAVE](https://github.com/acids-ircam/RAVE)
[FateZero](https://github.com/ChenyangQiQi/FateZero?tab=readme-ov-file)
[OpenVoice](https://github.com/myshell-ai/OpenVoice)
[xtts2-ui](https://github.com/BoltzmannEntropy/xtts2-ui)
  
Disclaimer: This page contains AI generated content and is a work in progress.
