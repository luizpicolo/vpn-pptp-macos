# PPTP VPN on macOS 

Apple stopped the support for it’s builtin PPTP VPN client on macOS Sierra, but they kept their libraries, so it’s still possible to create a PPTP VPN connection over command line without using any 3rd party clients, like FlowVPN or Shimo.

I know that PPTP VPN has become outdated and is less secure than other protocols, but in a corporate world you sometimes don’t have a choice.

The following procedure will show how you can create a PPTP VPN on macOS Sierra. It’s based on my own configuration (MPPE-128), so you might have to adapt it to your need.

The procedure is quite simple, you first need to create a file in /etc/ppp/peers with a name that represent your domain, or company, so in my case I will call it vpn.example.com.

    $ sudo touch /etc/ppp/peers/vpn.example.com

This file will contain the configuration that pppd daemon will reference and try to connect. Change with your configuration in **remoteaddress**, **user** and **password**

    plugin PPTP.ppp
    noauth
    # logfile /tmp/ppp.log
    remoteaddress "vpn.example.com"
    user "username"
    password "mYS3cUr3P@ssW0rd!"
    redialcount 1
    redialtimer 5
    idle 1800
    # mru 1368
    # mtu 1368
    receive-all
    novj 0:0
    ipcp-accept-local
    ipcp-accept-remote
    # noauth
    refuse-eap
    refuse-pap
    refuse-chap-md5
    hide-password
    mppe-stateless
    mppe-128
    # require-mppe-128
    looplocal
    nodetach
    # ms-dns 8.8.8.8
    usepeerdns
    # ipparam gwvpn
    defaultroute
    debug

Now you only need start the pppd daemon with the following command and that’s it.

    $ sudo pppd call vpn.example.com

# Test 

Tested in:

 - Sierra
 - Mojave

Reference: https://malucelli.net/2017/05/16/pptp-vpn-on-macos-sierra/
