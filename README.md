# decrypt Boox Update Upx

Have been tested only in [these Boox ereaders](#the-strings). If you found it available for any other Boox ereader, please submit an [issue](https://github.com/Hagb/decryptBooxUpdateUpx/issues) or [pull request](https://github.com/Hagb/decryptBooxUpdateUpx/pulls), with [the strings](#the-strings) following.

Note1: There is also [another method to fetch decrypted `update.zip`](https://github.com/Hagb/decryptBooxUpdateUpx/issues/1) from [@shunf4](https://github.com/shunf4).

Note2: There is [a way to get downloading url of latest firmware](https://github.com/Hagb/decryptBooxUpdateUpx/issues/2#issuecomment-704006389).

Any other issue and pull request is also welcomed!

[The detail of algorithm (Simplified Chinese)](algorithm-zh_cn.md)

## Demo

There is a python class `DeBooxUpx` in [DeBooxUpx.py](DeBooxUpx.py) to decrypt `update.upx`, and dict `boox_strings` where there are known strings.

Following is a example to use this class to decrypt the `update.upx` of Boox Nove Pro:

``` python3
from DeBooxUpx import DeBooxUpx, boox_strings
model = 'NovaPro'
updateUpxPath = 'update.upx'
decryptedPath = 'update.zip'

decrypter = DeBooxUpx(**boox_strings[model])
print('When updating, the device decrypt update package into', decrypter.path)
decrypter.deUpx(updateUpxPath, decryptedPath)
```

Or for manually setting strings:

``` python3
from DeBooxUpx import DeBooxUpx

MODEL = "NovaPro" 
STRING_7F00500 = "j857wYAQcWZgvIEQ/tcQqzxreUJgFHwJl6D2TN9BuSkQ" 
STRING_7F00501 = "+soGw/YVdGIRJiAs5SMmv1ihW37H1Fa9+/1w2Vgt14Ag" 
STRING_7F00502 = "lpsj9NJ8Kzv8jHb+OO8A5lxC+9Zhl243bFmDZYaF" 
updateUpxPath = 'update.upx'
decryptedPath = 'update.zip'

decrypter = DeBooxUpx(MODEL, STRING_7F00500, STRING_7F00501, STRING_7F00502)
print('When updating, the device decrypt update package into', decrypter.path)
decrypter.deUpx(updateUpxPath, decryptedPath)
```

## The strings

|       |  MODEL  |            STRING\_7F00500 (S1)              |               STRING\_7F00501 (S2)           |           STRING\_7F00502 (S3)           |
|-------|---------|----------------------------------------------|----------------------------------------------|------------------------------------------|
|NovaPro|`NovaPro`|`j857wYAQcWZgvIEQ/tcQqzxreUJgFHwJl6D2TN9BuSkQ`|`+soGw/YVdGIRJiAs5SMmv1ihW37H1Fa9+/1w2Vgt14Ag`|`lpsj9NJ8Kzv8jHb+OO8A5lxC+9Zhl243bFmDZYaF`|
|NovaPlus  |`NovaPlus` |`MtoBkApRAVwzdGe2CTnaE4MIgevRYNQfaKo606tyUQNY`|`Nttwkwxaei8xorBu/uUBpUu8nNZHTRIAZMZc0xrJs9Ti`|`LIYj1F9NVXFrOfi24/C76gFFxHYSCJ4mfhYI4q5w`|
| Max3  | `Max3`  |`1wdvUHZmcz32N1pgG4fkHmDsTDVihMJsPCNV4mW/6u1k`|`3nxuLgdpBE3B3n1Yyymt4cOS8dNucfQxK8YOsmcemuyO`|`yCA9YlFxLBdLbDUl3vwzPkn9vtYuVFZCfhrOTvR1`|
|MaxLumi|`MaxLumi`|`mTZFN0K+oMcGnn2n7+zV5DH7kr/Hbes2x/wKDJp6K7Kq`|`mj0zR0Oy3L4R+6y49MIEQT9bdx9AVz8TWyG9q3N+d9VY`|`hWAUdhOp9ekIYxIW+LpVj6OviWBbCbRa1c7s1jtW`|
| Note2 | `Note2` |`etwiPPEXAQRj3m+e0Q2FOxT16aJ8XexQAqhGn5NqZWv1`|`et0jSPpmd3ueGHLmMf+2yyXVn18sa2HrDg56dCTFH6lf`|`YY9wfqN7K1LlSug47Tr5Y8QkDHmmJ4VDCJ58mhoV`|
| NotePro  | `NotePro` | `MjR72bOazBUacJwDcuWgtm/E0A9F9ahIt1buweEPA020` |`RjV8r7+fx2Wjp6rUSrBOpmqYnHKs7eReqTTcy9k4c3tn` |`W2co6eaDmEl7jIjOSqr11C71RDHHiV3p5oG2G54X` |
| Note3 | `Note3` |`uTiMM5JgTXOCZAZKMcZIzc1yQpfX1+jxTFOred3te4z9`|`zEf3SZ8TOADA8QuwOHicGLrrc4EA7sffKfc01TlUfe/q`|`pmXVBMt5EllxXhD9L6/NWH/pTZXRURP6QLsrNlx6`|

**Some or all of the firmwares for the Onyx readers sold in Russia don't use the same strings with their international versions!**

### How to get the strings

`MODEL` string is the model name, exactly the output of `getprop ro.product.model` and/or value of `ro.product.model` in the file `/system/build.prop`.

Other strings can be got in following steps:

1. Get `/system/app/OnyxOtaService/OnyxOtaService.apk`
    
    USB Debugging (adb) is one of the the available ways:
    
    1. Turn on adb in Settings -> Applications -> USB Debugging Mode
    2. (from [@mgrub](https://github.com/mgrub)'s [note](https://github.com/Hagb/decryptBooxUpdateUpx/issues/5)) Connect the ebook to computer by usb, run
       ``` shell
       adb wait-for-device
       adb shell 'pm list packages -f | grep ota'
       ```
       And then the path of ota package will be showed. For example, the following output is in Nova Pro:
       ```
       package:/system/app/OnyxOtaService/OnyxOtaService.apk=com.onyx.android.onyxotaservice
       ```
       So in this case the path is `/system/app/OnyxOtaService/OnyxOtaService.apk`.

       In the following steps, we assume that the path is `/system/app/OnyxOtaService/OnyxOtaService.apk`.

    3. Run the following command
       ``` shell
       adb pull /system/app/OnyxOtaService/OnyxOtaService.apk .
       ```
    Now the apk is got.

2. Get the strings from apk

    1. Use [Apktool](https://github.com/iBotPeaches/Apktool) to decode the apk:
       ``` shell
       apktool d OnyxOtaService.apk
       ```
    2. Now the strings should be in `./OnyxOtaService/res/values/strings.xml`, for example the NovaPro one:
       ``` xml
       <?xml version="1.0" encoding="utf-8"?>
       <resources>
           <string name="settings">j857wYAQcWZgvIEQ/tcQqzxreUJgFHwJl6D2TN9BuSkQ</string>
           <string name="upgrade">+soGw/YVdGIRJiAs5SMmv1ihW37H1Fa9+/1w2Vgt14Ag</string>
           <string name="local">lpsj9NJ8Kzv8jHb+OO8A5lxC+9Zhl243bFmDZYaF</string>
           
       </resources>
       ```
       
      `settings` is `STRING_7F00500`, `upgrade` is `STRING_7F00501`, and `local` is `STRING_7F00502`

