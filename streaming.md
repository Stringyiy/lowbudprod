## Linux Setup

### Linux Server

sudo adduser newuser

sudo usermod -aG ssh username

sudo vim /etc/ssh/sshd_config

   ```plaintext
   PasswordAuthentication no
   Match User newuser
       AllowTcpForwarding yes
       X11Forwarding no
       PermitTunnel yes
   ```

sudo usermod -s /bin/rbash newuser

echo "PATH=/usr/local/bin:/usr/bin:/bin" > /home/newuser/.bash_profile

sudo systemctl restart sshd

echo "your_public_key" >> ~/.ssh/authorized_keys

### Linux Client

#### Firmware Update

sudo raspi-config

    2. In the pop-up window, choose 'Interface Options'-> 'SSH'.
    3. Select'Yes'
    4. Enable the SSH service.

sudo vim /etc/ssh/sshd_config

   ```plaintext
   PasswordAuthentication no
   Match User newuser
       AllowTcpForwarding yes
       X11Forwarding no
       PermitTunnel yes
   ```

ssh -D 8080 -C newuser@hostname_or_ip

vcgencmd bootloader_version
sudo apt update 
sudo apt full-upgrade
sudo apt autoremove
sudo reboot

sudo raspi-config
   Then select 6 Advanced Opitions => A5 Bootloader Version => E1 Latest, answere Yes）
sudo reboot

sudo rpi-eeprom-update
vcgencmd bootloader_version

#### Audio Set Up
IQAudio Codec Zero on Pi5 (https://github.com/raspberrypi/Pi-Codec/issues/9)

   python -m venv ~/venv

   source ~/venv/bin/activate

   pip install -r requirements.txt
                
#### Install OBS on Linux

'''
sudo add-apt-repository ppa:obsproject/obs-studio

sudo apt install obs-studio
'''

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

### RealTime Processing

### Ref

[NDI](https://interfacinglinux.com/2024/08/15/ndi-6-on-linux-with-obs/)

### Interesting Editing Projects on Github
[RAVE](https://github.com/acids-ircam/RAVE)
[FateZero](https://github.com/ChenyangQiQi/FateZero?tab=readme-ov-file)
  
Disclaimer: This page contains AI generated content and is a work in progress.
