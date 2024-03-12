## TCP Dump

`rvictl -s <DEVICE_UDID>`

`ifconfig -l`

`sudp tcpdump -i rvi0 -w <OUTPUT_FILE_PATH> (i.e  ./output.pcap)`

<img width="1680" alt="Screen Shot 2020-10-30 at 1 20 42 PM" src="https://github.com/DexCodeMobile/xcode-debuggin/assets/1080918/947be1b5-1990-4674-a696-f2105bfdef58">


## Wireshark

Find the right server to capture (i.e rvi0)

Filter by `isakmp || dns || esp`
