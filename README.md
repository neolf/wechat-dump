## Dump WeChat Messages from Android

## 导出安卓微信聊天数据

WeChat(微信), as the most popular mobile IM app in China, doesn't give users any method to export well-formatted history message.
This tool can parse and export WeChat messages on a rooted android phone.

Right now it can dump messages in text-only mode, or generate a single-file html containing voice messages, images, emoji, etc.

__NEWS__: WeChat 6.0+ uses silk to encode audio. The code is updated.

__NEWS__: WeChat 6.3 uses a new avatar storage. The code is updated.

If this tools works for you, please take a moment to __add your phone/OS to__ [the wiki](https://github.com/ppwwyyxx/wechat-dump/wiki).
If it doesn't work, please leave an issue together with your phone/OS/wechat version.

### How to use:

#### Dependencies:
+ python-PIL
+ [PyQuery](https://pypi.python.org/pypi/pyquery/1.2.1)
+ [pysox](https://pypi.python.org/pypi/pysox/0.3.6.alpha)
+ [pysqlcipher](https://pypi.python.org/pypi/pysqlcipher)
+ numpy
+ python-csscompressor (suggested, optional)
+ adb and rooted android phone connected to a Linux/Mac OS.
+ Silk audio decoder (just run `./third-party/compile_silk.sh`)
+ gnu-sed

#### Get Necessary Data:

+ Get decrypted WeChat database (Linux & Mac Only):
	+ Automatic: `./android-interact.sh db-decrypt`
		+ Requires rooted adb. Available at https://play.google.com/store/apps/details?id=eu.chainfire.adbd&hl=en
	+ Manual:
		+ Get /data/data/com.tencent.mm/MicroMsg/long-long-name/{EnMicroMsg.db,sfs/avatar.index} from __root__ filesystem, possible ways are:
			+ `./android-interact.sh db`
			+ Use your rooted file system manager app
		+ Get WeChat uin, possible ways are:
			+ `./android-interact.sh uin`
			+ Login to [web wechat](https://wx.qq.com), get wxuin=1234567 from `document.cookie`
		+ Get your phone IMEI number, possible ways are:
			+ `./android-interact.sh imei`
			+ Call `*#06#` on your phone
			+ Find IMEI in system settings
		+ Decrypt database, will produce `decrypted.db`:
		```
		./decrypt-db.py <path to EnMicroMsg.db> <imei> <uin>
		```


NOTE: you may need to try different ways to getting imei & uin,
because things behave differently on different phones.

Also, if the decryption doesn't work with pysqlcipher, maybe try the version of sqlcipher in `legacy`.


+ Get WeChat user resource directory from your phone to `resource` directory:
	+ `./android-interact.sh res`
	+ You might need to change the resource location in this script if the default doesn't work
	+ This takes a __long__ time.

#### Run:
+ Parse and dump text messages of __every__ chat (resource directory is not needed to run this):
```
./dump-msg.py decrypted.db output_dir
```
+ List all chats:
```
./list-chats.py decrypted.db
```
+ Generate statistical report on text messages:
```
./count-message.sh output_dir
```
+ Dump messages of one contact to html, containing voice messages, emojis, and images:
```
./dump-html.py decrypted.db avatar.index resource <contact name> output.html
```

### Examples:
See [here](http://ppwwyyxx.com/static/wechat/example.html) for an example html.

Screenshots of generated html:

![byvoid](https://github.com/ppwwyyxx/wechat-dump/raw/master/screenshots/byvoid.jpg)

### TODO List
+ Search by uid/username
+ Faster way to copy a directory from android (I don't know..).
+ Fix rare unhandled types: > 10000 and < 0
+ Better user experiences... see `grep 'TODO' wechat -R`
+ more easy-to-use for non-programmers (GUI?)

### Donate!
Paypal:
<a href="https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=7BC299GRDLEDU&lc=US&item_name=wechat%2ddump&item_number=wechat%2ddump&currency_code=USD&bn=PP%2dDonationsBF%3abtn_donate_SM%2egif%3aNonHosted"><img src="https://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif" alt="[paypal]" /></a>
