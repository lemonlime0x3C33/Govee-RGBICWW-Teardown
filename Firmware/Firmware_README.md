#Govee RGBICWW Smart Table Lamp Firmware Analysis

The initial suspected firmware was extracted from a zbit ZB25VQ16 NOR Flash Chip
after desoldering the chip from the board and using a raspberry pi and flashrom
to extract the contents of the chip. 

Running the Govee_firmware.bin through binwalk did not yield any file systems but hexdump 
and strings points to a freeRTOS as the OS. The firmware could also be encrypted but I do
not think that is the case. 

Binwalk does show the following output:

MD5 Checksum:  bed16b5dd0d6a9bb8d58636e093f9e6c
Signatures:    411

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
410752        0x64480         PEM certificate
415936        0x658C0         PEM RSA private key
439224        0x6B3B8         PEM certificate
1057325       0x10222D        GIF image data 21495 x 446
1246872       0x130698        SHA256 hash constants, little endian

## Addittionaly running strings and grepping for AT yields the following possible AT Commands

**These can be tested using the UART interface to the Real Tek chip, Please see this [post](https://lemonlime0x3c33.io/21/06/2024/govee-rgbicww-smart-table-lamp-part-2-uart/) for more details**

@AIAT
Usage: %s DURATION_SECONDS [with_len]
wext_set_tos_value():ioctl[SIOCDEVPRIVATE] error
wext_set_pscan_channel():ioctl[SIOCDEVPRIVATE] error
ioctl[SIOCDEVPRIVATE] error. ret=%d
wext_set_autoreconnect():ioctl[SIOCDEVPRIVATE] error
[ATWD]: _AT_WLAN_DISC_NET_
[ATWD]ERROR: Operation failed!
[ATWD]ERROR: Deassoc timeout!
[ATWQ]: _AT_WLAN_SIMPLE_CONFIG_
[ATWS]: _AT_WLAN_SCAN_
[ATWS]ERROR: Can't malloc memory for channel list
[ATWS]ERROR: Can't malloc memory for pscan_config
[ATWS]ERROR: wifi set partial scan channel fail
[ATWS]ERROR: wifi scan failed
[ATW?]: _AT_WLAN_INFO_
[ATW0]Error: SSID length can't exceed 32
[ATW0]: _AT_WLAN_SET_SSID_ [%s]
[ATW3]: _AT_WLAN_AP_SET_SSID_ [%s]
[ATW4]: _AT_WLAN_AP_SET_SEC_KEY_ [%s]
[ATW6]Usage: ATW6=BSSID
[ATW6]: _AT_WLAN_SET_BSSID_ [%s]
     ATWI=192.168.1.2,-n,100,-l,5000
     ATWT=-s,-p,5002
     ATWT=-c,192.168.1.2,-t,100,-p,5002
[ATWU]: _AT_WLAN_UDP_TEST_
[ATWU] Usage: ATWU=[-s|-c,host|stop][options]
     ATWU=-s,-p,5002
     ATWU=-c,192.168.1.2,-t,100,-p,5002
      MODE => STATION
%s DHCP_SERVER_STATE_OFFER
%s DHCP_SERVER_STATE_ACK
%s DHCP_SERVER_STATE_NAK
[ATWI] Usage: ATWI=[host],[options]
     ATWI=192.168.1.2,-n,100,-l,5000
TCP: TCP client is already running. Please enter "ATWT=stop" to stop it.
[ATWT] Command format ERROR!
[ATWT] Usage: ATWT=[-s|-c,host|stop],[options]
     ATWT=-s,-p,5002
     ATWT=-c,192.168.1.2,-t,100,-p,5002
UDP: UDP client is already running. Please enter "ATWU=stop" to stop it.
[ATWU] Command format ERROR!
[ATWU] Usage: ATWU=[-s|-c,host|stop][options]
     ATWU=-s,-p,5002
     ATWU=-c,192.168.1.2,-t,100,-p,5002
uart_upload_dev_info ble_pt_flag=STATE_SUC
uart_update_dev_light ble_pt_flag=STATE_SUC
uart_update_dev_color ble_pt_flag=STATE_SUC
uart_update_pt_info ble_pt_flag=STATE_SUC
[ATWS]: _AT_WLAN_SCAN_
-----BEGIN CERTIFICATE-----
-----END CERTIFICATE-----
-----BEGIN RSA PRIVATE KEY-----
-----END RSA PRIVATE KEY-----
-----BEGIN PRIVATE KEY-----
-----END PRIVATE KEY-----
-----BEGIN ENCRYPTED PRIVATE KEY-----
-----END ENCRYPTED PRIVATE KEY-----
[ATSD]: _AT_SYSTEM_DUMP_REGISTER_
[ATSD] Usage: ATSD=REGISTER
[ATSE]: _AT_SYSTEM_EDIT_REGISTER_
[ATSE] Usage: ATSE=REGISTER[VALUE]
[ATSC]: _AT_SYSTEM_CLEAR_OTA_SIGNATURE_
[ATSR]: _AT_SYSTEM_RECOVER_OTA_SIGNATURE_
[ATSK]: _AT_SYSTEM_RDP/RSIP_CONFIGURE_
[ATSK] Usage: ATSK=RDP_EN
[ATSK] Usage: ATSK=RDP_KEY[value(hex)]
[ATSK] 	Example: ATSK=RDP_KEY[345487bbaa435bfe382233445ba359aa]
[ATSK] Usage: ATSK=RSIP_EN
[ATSK] Usage: ATSK=RSIP_DIS
[ATSK] Usage: ATSK=RSIP_KEY[value(hex)]
[ATSK] RDP is enable
[ATSK] Err: RDP key length should be 16 bytes
[ATSK] Set RDP key done
[ATSK] RSIP is enable
[ATSK] RSIP is disable
[ATSK] Err: RSIP key length should be 16 bytes
[ATSK] Set RSIP key done
[ATSS]: _AT_SYSTEM_CPU_STATS_
[ATS@]: _AT_SYSTEM_DBG_SETTING_
[ATS@] Usage: ATS@=[LEVLE,FLAG]
[ATS@] level = %d, flag = 0x%08X
[ATS!]: _AT_SYSTEM_CONFIG_SETTING_
[ATS!] Usage: ATS!=[CONFIG(0,1,2),FLAG]
[ATS!] ConfigDebugErr  = 0x%08X
[ATS!] ConfigDebugInfo = 0x%08X
[ATS!] ConfigDebugWarn = 0x%08X
[ATS#]: _AT_SYSTEM_TEST_
[ATSJ]: _AT_SYSTEM_JTAG_
[ATSJ] Usage: ATSJ=off
ATSL=%s is not supported!
[ATS?]: _AT_SYSTEM_HELP_
[ATS?]: COMPILE TIME: %s
[ATS?]: SW VERSION: %s
[ATSL]: _AT_SYS_WAKELOCK_TEST_
[ATSL] Usage ATSL=[a/r/?][bitmask]
[ATSL] wakelock:0x%08x
[ATSL] clean wakelock stat
ATSD
ATSE
ATSK
ATSC
ATSR
ATSS
ATSJ
ATS@
ATS!
ATS#
ATS?
ATSL
ATWL
ATWI
ATWT
ATWU
ATW0
ATW1
ATW2
ATW3
ATW4
ATW5
ATW6
ATWA
ATWB
ATWC
ATWD
ATWP
ATWR
ATWS
ATWM
ATWZ
ATWO
ATWQ
ATWW
ATWw
ATWY
ATW?
ATW+ABC
ATXP
[ATW0]Usage: ATW0=SSID(Maximum length is 32)
[ATW1]: _AT_WLAN_SET_PASSPHRASE_ [%s]
[ATW2]: _AT_WLAN_SET_KEY_ID_ [%s]
[ATW2]Error: Wrong WEP key id. Must be one of 0,1,2, or 3.
[ATW3]Usage: ATW3=SSID
[ATW3]Error: SSID length can't exceed 32
[ATW4]Usage: ATW4=PASSWORD
[ATW4]Error: PASSWORD length can't exceed 64
[ATW5]Usage: ATW5=CHANNEL
[ATW5]: _AT_WLAN_AP_SET_CHANNEL_ [channel %d]
[ATWA]: _AT_WLAN_AP_ACTIVATE_
[ATWA]Error: SSID can't be empty
[ATWA]Error: password length is between 8 to 64 
[ATWC]: _AT_WLAN_JOIN_NET_
[ATWC]Error: SSID can't be empty
[ATWR]: _AT_WLAN_GET_RSSI_
[ATWY]: _AT_WLAN_GET_SNR_
[ATWP]: _AT_WLAN_POWER_[%s]
[ATWP]Usage: ATWP=0/1
[ATWB](_AT_WLAN_AP_STA_ACTIVATE_)
[ATWB]Error: SSID can't be empty
[ATWB]Error:bad channel! channel is from 1 to 14
[ATWM]: _AT_WLAN_PROMISC_
[ATWM]Usage: ATWM=DURATION_SECONDS[with_len]
Please set CONFIG_ENABLE_WPS 1 in platform_opts.h to enable ATWW command
[ATWO]: _AT_WLAN_OTA_UPDATE_
[ATWO]Usage: ATWO=IP[PORT] or ATWO= REPOSITORY[FILE_PATH]
[ATWZ]: _AT_WLAN_IWPRIV_
[ATWZ]Usage: ATWZ=COMMAND[PARAMETERS]
[ATXP]: _AT_WLAN_POWER_MODE_
[ATXP] Usage: ATXP=lps/ips/dtim/tdma[mode]
WLAN AT COMMAND SET:
   # ATWS
   # ATW0=SSID
   # ATW1=PASSPHRASE
   # ATWC
   # ATW3=SSID
   # ATW4=PASSPHRASE
   # ATW5=CHANNEL
   # ATWA
   # ATWI=xxx.xxx.xxx.xxx
Please set CONFIG_SSL_CLIENT 1 in platform_opts.h to enable ATWL command
[ATWI]: _AT_WLAN_PING_TEST_
[ATWI] Usage: ATWI=[host],[options]
[ATWT]: _AT_WLAN_TCP_TEST_
[ATWT] Usage: ATWT=[-s|-c,host|stop],[options]
-----BEGIN CERTIFICATE-----
-----END CERTIFICATE-----
AT--
ATxx
@AIAT
Usage: %s DURATION_SECONDS [with_len]
pATX
wext_set_tos_value():ioctl[SIOCDEVPRIVATE] error
wext_set_pscan_channel():ioctl[SIOCDEVPRIVATE] error
ioctl[SIOCDEVPRIVATE] error. ret=%d
wext_set_autoreconnect():ioctl[SIOCDEVPRIVATE] error
[ATWD]: _AT_WLAN_DISC_NET_
[ATWD]ERROR: Operation failed!
[ATWD]ERROR: Deassoc timeout!
[ATWQ]: _AT_WLAN_SIMPLE_CONFIG_
[ATWS]: _AT_WLAN_SCAN_
[ATWS]ERROR: Can't malloc memory for channel list
[ATWS]ERROR: Can't malloc memory for pscan_config
[ATWS]ERROR: wifi set partial scan channel fail
[ATWS]ERROR: wifi scan failed
[ATW?]: _AT_WLAN_INFO_
[ATW0]Usage: ATW0=SSID(Maximum length is 32)
[ATW0]Error: SSID length can't exceed 32
[ATW0]: _AT_WLAN_SET_SSID_ [%s]
[ATW1]: _AT_WLAN_SET_PASSPHRASE_ [%s]
[ATW3]Error: SSID length can't exceed 32
[ATW3]: _AT_WLAN_AP_SET_SSID_ [%s]
[ATW6]Usage: ATW6=BSSID
[ATW6]: _AT_WLAN_SET_BSSID_ [%s]
     ATWI=192.168.1.2,-n,100,-l,5000
     ATWT=-s,-p,5002
     ATWT=-c,192.168.1.2,-t,100,-p,5002
[ATWU]: _AT_WLAN_UDP_TEST_
[ATWU] Usage: ATWU=[-s|-c,host|stop][options]
     ATWU=-s,-p,5002
     ATWU=-c,192.168.1.2,-t,100,-p,5002
mp_pseudo_STATION
      MODE => STATION
[ATWI] Usage: ATWI=[host],[options]
     ATWI=192.168.1.2,-n,100,-l,5000
TCP: TCP client is already running. Please enter "ATWT=stop" to stop it.
[ATWT] Command format ERROR!
[ATWT] Usage: ATWT=[-s|-c,host|stop],[options]
     ATWT=-s,-p,5002
     ATWT=-c,192.168.1.2,-t,100,-p,5002
UDP: UDP client is already running. Please enter "ATWU=stop" to stop it.
[ATWU] Command format ERROR!
[ATWU] Usage: ATWU=[-s|-c,host|stop][options]
     ATWU=-s,-p,5002
     ATWU=-c,192.168.1.2,-t,100,-p,5002
[ATSD]: _AT_SYSTEM_DUMP_REGISTER_
[ATSD] Usage: ATSD=REGISTER
[ATSE]: _AT_SYSTEM_EDIT_REGISTER_
[ATSE] Usage: ATSE=REGISTER[VALUE]
[ATSC]: _AT_SYSTEM_CLEAR_OTA_SIGNATURE_
[ATSR]: _AT_SYSTEM_RECOVER_OTA_SIGNATURE_
[ATSK]: _AT_SYSTEM_RDP/RSIP_CONFIGURE_
[ATSK] Usage: ATSK=RSIP_KEY[value(hex)]
[ATSK] Usage: ATSK=RDP_KEY[value(hex)]
[ATSK] RDP is enable
[ATSK] Err: RDP key length should be 16 bytes
[ATSK] Set RDP key done
[ATSK] RSIP is enable
[ATSK] RSIP is disable
[ATSK] Err: RSIP key length should be 16 bytes
[ATSK] Set RSIP key done
[ATSK] Usage: ATSK=RDP_EN
[ATSK] 	Example: ATSK=RDP_KEY[345487bbaa435bfe382233445ba359aa]
[ATSK] Usage: ATSK=RSIP_EN
[ATSK] Usage: ATSK=RSIP_DIS
[ATSA]: _AT_SYSTEM_ADC_TEST_
[ATSA] Usage: ATSA=CHANNEL(1~3)
[ATSA] Usage: ATSA=k_get
[ATSA] Usage: ATSA=k_set[offet(hex),gain(hex)]
[ATSA] A%d = 0x%04X
[ATSG]: _AT_SYSTEM_GPIO_TEST_
[ATSG] Usage: ATSG=PINNAME(ex:A0)
[ATSG]: Invalid Pin Name
[ATSG] %c%d = %d
[ATSB]: data 0x%x otu_addr 0x%x
[ATSB]: _AT_SYSTEM_BOOT_OTU_PIN_SET_
[ATSB]: Usage: ATSB=[GPIO_PIN,TRIGER_MODE,UART]
[ATSB]: GPIO_PIN: PB_1, PC_4 ....
[ATSB]: TRIGER_MODE: low_trigger, high_trigger
[ATSB]: UART: UART0, UART2
[ATSB]: example: ATSB=[PC_2,low_trigger,UART2]
[ATSB]: Input UART index error. Please choose UART0 or UART2.
[ATSB]:gpio_pin 0x%x
[ATSB]:gpio_pin_bar 0x%x
[ATSB]:uart_port 0x%x
[ATSB]:uart_port_bar 0x%x
[ATSB]:boot_gpio 0x%x
[ATSB]:Read 0x900c 0x%x
[ATSS]: _AT_SYSTEM_CPU_STATS_
[ATS@]: _AT_SYSTEM_DBG_SETTING_
[ATS@] Usage: ATS@=[LEVLE,FLAG]
[ATS@] level = %d, flag = 0x%08X
[ATS!]: _AT_SYSTEM_CONFIG_SETTING_
[ATS!] Usage: ATS!=[CONFIG(0,1,2),FLAG]
[ATS!] ConfigDebugErr  = 0x%08X
[ATS!] ConfigDebugInfo = 0x%08X
[ATS!] ConfigDebugWarn = 0x%08X
[ATS#]: _AT_SYSTEM_TEST_
[ATSJ]: _AT_SYSTEM_JTAG_
[ATSJ] Usage: ATSJ=off
ATSL=%s is not supported!
[ATS?]: _AT_SYSTEM_HELP_
[ATS?]: COMPILE TIME: %s
[ATS?]: SW VERSION: %s
[ATSL]: _AT_SYS_WAKELOCK_TEST_
[ATSL] Usage ATSL=[a/r/?][bitmask]
[ATSL] wakelock:0x%08x
[ATSL] clean wakelock stat
ATSD
ATSE
ATSK
ATSC
ATSR
ATSA
ATSG
ATSP
ATSB
ATSS
ATSJ
ATS@
ATS!
ATS#
ATS?
ATSL
ATWL
ATWI
ATWT
ATWU
ATW0
ATW1
ATW2
ATW3
ATW4
ATW5
ATW6
ATWA
ATWB
ATWC
ATWD
ATWP
ATWR
ATWS
ATWM
ATWZ
ATWO
ATWQ
ATWW
ATWw
ATWY
ATW?
ATW+ABC
ATXP
[ATW2]: _AT_WLAN_SET_KEY_ID_ [%s]
[ATW2]Error: Wrong WEP key id. Must be one of 0,1,2, or 3.
[ATW3]Usage: ATW3=SSID
[ATW4]Usage: ATW4=PASSWORD
[ATW4]Error: PASSWORD length can't exceed 64
[ATW4]: _AT_WLAN_AP_SET_SEC_KEY_ [%s]
[ATW5]Usage: ATW5=CHANNEL
[ATW5]: _AT_WLAN_AP_SET_CHANNEL_ [channel %d]
[ATWA]: _AT_WLAN_AP_ACTIVATE_
[ATWA]Error: SSID can't be empty
[ATWA]Error: password length is between 8 to 64 
[ATWC]: _AT_WLAN_JOIN_NET_
[ATWC]Error: SSID can't be empty
[ATWR]: _AT_WLAN_GET_RSSI_
[ATWY]: _AT_WLAN_GET_SNR_
[ATWP]: _AT_WLAN_POWER_[%s]
[ATWP]Usage: ATWP=0/1
[ATWB](_AT_WLAN_AP_STA_ACTIVATE_)
[ATWB]Error: SSID can't be empty
[ATWB]Error:bad channel! channel is from 1 to 14
[ATWM]: _AT_WLAN_PROMISC_
[ATWM]Usage: ATWM=DURATION_SECONDS[with_len]
Please set CONFIG_ENABLE_WPS 1 in platform_opts.h to enable ATWW command
[ATWO]: _AT_WLAN_OTA_UPDATE_
[ATWO]Usage: ATWO=IP[PORT] or ATWO= REPOSITORY[FILE_PATH]
[ATWZ]: _AT_WLAN_IWPRIV_
[ATWZ]Usage: ATWZ=COMMAND[PARAMETERS]
[ATXP]: _AT_WLAN_POWER_MODE_
[ATXP] Usage: ATXP=lps/ips/dtim/tdma[mode]
WLAN AT COMMAND SET:
   # ATWS
   # ATW0=SSID
   # ATW1=PASSPHRASE
   # ATWC
   # ATW3=SSID
   # ATW4=PASSPHRASE
   # ATW5=CHANNEL
   # ATWA
   # ATWI=xxx.xxx.xxx.xxx
[ATWL]: _AT_WLAN_SSL_CLIENT_
ATWL=SSL_SERVER_HOST
[ATWI]: _AT_WLAN_PING_TEST_
[ATWI] Usage: ATWI=[host],[options]
[ATWT]: _AT_WLAN_TCP_TEST_
[ATWT] Usage: ATWT=[-s|-c,host|stop],[options]
AT--
ATxx

