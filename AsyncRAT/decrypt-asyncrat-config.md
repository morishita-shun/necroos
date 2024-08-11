# decrypt-asyncrat-config
- ref. https://www.youtube.com/watch?v=_InXFxdGRg8 (Decrypting AsyncRAT Configuration Values with CyberChef)

## Convert salt
```python
$ python3
>>> x = [191, 235, 30, 86, 251, 205, 151, 59, 178, 25, 2, 36, 48, 165, 120, 67, 0, 61, 86, 68, 210, 30, 98, 185, 212, 241, 128, 231, 230, 195, 57, 65]
>>> ''.join([f"{d:02x}" for d in x])
'bfeb1e56fbcd973bb219022430a57843003d5644d21e62b9d4f180e7e6c33941'
```

## CyberChef recipe
```
From_Base64('A-Za-z0-9+/=',true,false)
To_Hex('None',0)
Regular_expression('User defined','^.{64}(.*)',true,true,false,false,false,false,'List capture groups')
Register('^(.{32})',true,false,false)
Register('^.{32}(.*)',true,false,false)
Derive_PBKDF2_key({'option':'Base64','string':'Q29qUklKdHA4SEJvZzczU0VzMDJHaU85MTZHeFFnU0o='},256,50000,'SHA1',{'option':'Hex','string':'bfeb1e56fbcd973bb219022430a57843003d5644d21e62b9d4f180e7e6c33941'})
Register('([\\s\\S]*)',true,false,false)
Find_/_Replace({'option':'Regex','string':'^.*$'},'$R1',true,false,true,false)
AES_Decrypt({'option':'Hex','string':'$R2'},{'option':'Hex','string':'$R0'},'CBC','Hex','Raw',{'option':'Hex','string':''},{'option':'Hex','string':''})
```
- Input: Target config
- Derive PBKDF2 key - Passphrase: Settings.Key
- Derive PBKDF2 key - Salt: Aes256.Salt (hex)
