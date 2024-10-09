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

ssh -D 192.168.0.100:1080 -f -C -q -N admin@13.231.39.140
export ALL_PROXY=socks5://192.168.0.100:1080
cat 'Acquire::https::proxy "socks5h://192.168.0.100:1080";' >> /etc/apt/apt.conf.d/99proxy

vcgencmd bootloader_version
sudo apt update 
sudo apt full-upgrade
sudo apt autoremove
sudo reboot

ssh -D 192.168.0.100:1080 -f -C -q -N admin@13.231.39.140
sudo raspi-config
#   Then select 6 Advanced Opitions => A5 Bootloader Version => E1 Latest, answere Yes）
sudo reboot

ssh -D 192.168.0.100:1080 -f -C -q -N admin@13.231.39.140
sudo rpi-eeprom-update
vcgencmd bootloader_version
sudo reboot
#sudo vi /etc/sysctl.conf
#net.ipv4.ip_forward=1
#sudo sysctl -p
```
```bash
```
```bash
curl -x localhost:3128 ipinfo.io/ip
```

```bash
sudo ufw allow 3128/tcp
sudo ufw delete allow 3128
```

sshuttle

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

sudo ln -s /usr/lib/x86_64-linux-gnu/obs-plugins/obs-ndi.so /usr/local/lib/obs-plugins/obs-ndi.so
sudo ln -s /usr/share/obs/obs-plugins/obs-ndi/ /usr/local/share/obs/obs-plugins/obs-ndi

```

obs-websocket obs-cli obs-distroAV
Wayland Screen Capture (PipeWire)
NDI 6 out 
https://dicaffeine.com

#### Interface
Optimize LAN to handle wireless audio video streams  

```  
#echo "net.ipv4.ip_forward=1" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
sudo apt install iptables proxychains
cat 'socks5 192.168.0.100 1080' >> /etc/proxychains.conf
```
```
#bashrc
#ssh -D 1080 -f -C -q -N admin@13.231.39.140
sudo nmcli dev modify eth1 ipv4.ignore-auto-dns yes
sudo nmcli connection modify "Wired connection 2" ipv4.dns "8.8.8.8"        
#export ALL_PROXY=socks5h://localhost:1080
sudo iptables -A INPUT -p tcp --dport 1080 -s 192.168.0.100/24 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 1080 -j DROP
ssh -D 192.168.0.100:1080 -f -C -q -N admin@13.231.39.140 
~
```

### Sourcing
RTMP (Streaming Platform), RTSP (IP Cam), NDI (NewTek), UVC (Capture Card/WebCam)

iOS:
Obsbot Live (RTMP out)
Topdirector (RTSP/NDI in, RTMP/NDI out)
Camo Studio (UVC in, RTMP out)

Android (refurb) :  
workload asc: UVC, NDI
DroidCam

PC:
OBS Studio + DistroAV

traffic monitor auto switch

###Preprocessing  

#### Video Rescale and Concat
awk '{print $0}' combine.txt > ffi.txt
mvflv() {
while read -r line; do
    echo "Processing: $line"
    mv $line $line.flv
done < ffi.txt
}
mvflv
awk '{print "file " $0 ".flv"}' ffi.txt > ffi_flv.txt

####[Optional] Rescale and Rotation
scaleflv() {
while read -r line; do
    echo "Processing: $line"
    ffmpeg -i "$line" -vf "scale=1920:1080" -c:v libx264 -crf 23 -c:a copy "scaled_$(basename "$line")"
done < ffi_scaled.txt
}
scaleflv
awk '{print "scaled_" $0}' ffi_flv.txt > ffi_scaled.txt

ffmpeg -f concat -safe 0 -i ffi_flv.txt -c copy output.mp4
rm $(cat ffi_flv.txt)
rm ffi.txt ffi_flv.txt ffi_scaled.txt

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

### Mixing

### Streaming
#### Chatroom Notification
git clone git@github.com:saermart/DouyinLiveWebFetcher.git

### RealTime Processing

### Ref  

[NDI](https://interfacinglinux.com/2024/08/15/ndi-6-on-linux-with-obs/)

### Github Project Mentions
[RAVE](https://github.com/acids-ircam/RAVE)
[FateZero](https://github.com/ChenyangQiQi/FateZero?tab=readme-ov-file)
[OpenVoice](https://github.com/myshell-ai/OpenVoice)
[xtts2-ui](https://github.com/BoltzmannEntropy/xtts2-ui)
  
Disclaimer: This page contains AI generated content and is a work in progress.
