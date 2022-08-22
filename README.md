# Venom RootKit
I have written a simple windows rootkit to explore a bit about the world of rootkits and windows kernel in general.
The Venom rootkit uses a few well-known methods commonly being used by other famous rootkits. Below are some of the main features listed in the "Features" section.

# Flow
The flow of the rootkit is as follows:
We start by dropping the rootkit .sys file and the UM .dll file to disk.
Then, we load the rootkit driver, we can do so using some exploit or projects like DSEFix and KDMapper.
Once the rootkit is loaded, it creates a device and a symlink, so that the UM client will be able to talk with it.
Then it performs the IRP hook over the nsiproxy driver. And then it performs an APC injection of the UM dll to an arbitrary thread within "explorer.exe" (It can easily be changed). The APC injection is first queening a kernel APC and then a user APC, so we can avoid Microsoft ETW event on user-mode APC created from the kernel, as described [here](https://medium.com/@philiptsukerman/bypassing-the-microsoft-windows-threat-intelligence-kernel-apc-injection-sensor-92266433e0b0).

# Demo
Here is a little demo of the port hiding feature -
![Port Hiding](https://i.imgur.com/f5Qtlf1.png)

* My C&C is only for the POC, my main goal was the rootkit so I invested the minimum I needed for the demo.

# Features
- [x] Dynamic APC injection to load the UM dll.
- [x] Process Hiding.
- [x] Token elevation to "NT AUTHORITY\SYSTEM".
- [x] Command execution.
- [x] TCP port hiding by IRP hooking nsiproxy driver.
- [x] C&C server communication. 
- [ ] Logging.
- [ ] File hiding.
- [ ] Anti VM/Debug (Maybe implement through TLS callbacks).
- [ ] Dynamic config for UM client.

# Some other projects I have taken inspiration from
 - [BlackBone](https://github.com/DarthTon/Blackbone)
 - [Win_Rootkit](https://github.com/alal4465/Win_Rootkit)

# Thanks
I want to thank [@omerk2511](https://github.com/omerk2511) for helping and guiding me.

# Disclaimer
This project is for educational purposes only, I am not responsible for any kind of abuse.
