## TorrServer
TorrServer, stream torrent to http

### Installation
Just download server from releases and exec file\
https://github.com/YouROK/TorrServer/releases \
After open browser link http://127.0.0.1:8090 \
On linux systems you need to set the environment variable before run \
***export GODEBUG=madvdontneed=1***

### Build
Install golang 1.16+ by instruction: https://golang.org/doc/install \
Goto dir to source\
Run build script under linux build-all.sh\
For build web page need install npm and yarn\
For instal yarn: _npm i -g yarn_ after install npm\
For build android server need android toolchain\
Download android ndk and copy android-ndk-XXX/toolchains/llvm/prebuilt/linux-x86_64 dir to source, rename it to toolchain

#
### Server args:
#### Usage
TorrServer [--port PORT] [--path PATH] [--logpath LOGPATH] [--rdb] [--httpauth] [--dontkill] [--ui]

#### Options
* --port PORT, -p PORT             web server port
* --path PATH, -d PATH             database and settings path
* --logpath LOGPATH, -l LOGPATH    log path
* --rdb, -r                        start in read-only DB mode
* --httpauth, -a                   http auth on all requests
* --dontkill, -k                   dont kill server on signal
* --ui, -u                         run page torrserver in browser
* --version                        display version and exit


#
### Http Api of TorrServer:
#### GET

###### /echo 
*Return version of server*

###### /shutdown 
*Shutdown server*

###### /stream...
#### args:
* link - magnet/hash/link to torrent
* index - index of file
* preload - preload torrent
* stat - return stat of torrent
* save - save to db
* m3u - return m3u
* fromlast - return m3u from last play
* play - start stream torrent
* title - set title of torrent
* poster - set poster link of torrent

##### Examples:
>**get stat**
>
>http://127.0.0.1:8090/stream/fname?link=...&stat
>
>**get m3u**
>
>http://127.0.0.1:8090/stream/fname?link=...&index=1&m3u
>http://127.0.0.1:8090/stream/fname?link=...&index=1&m3u&fromlast
>
>**stream torrent**
>
>http://127.0.0.1:8090/stream/fname?link=...&index=1&play
>http://127.0.0.1:8090/stream/fname?link=...&index=1&play&save
>http://127.0.0.1:8090/stream/fname?link=...&index=1&play&save&title=...&poster=...
>
>**only save**
>
>http://127.0.0.1:8090/stream/fname?link=...&save&title=...&poster=...

###### /play/:hash/:id
#### params:
* hash - hash of torrent
* index - index of file

###### /playlistall/all.m3u
*Get all http links of all torrents in m3u list*

###### /playlist
*Get http link of torrent in m3u list*
#### args:
* hash - hash of torrent
* fromlast - from last play file

#
#### POST
###### /torrents
##### Send json:
{\
    "action": "add/get/set/rem/list/drop",\
    "link": "hash/magnet/link to torrent",\
    "hash": "hash of torrent",\
    "title": "title of torrent",\
    "poster": "link to poster of torrent",\
    "data": "custom data of torrent, may be json",\
    "save_to_db": true/false\
}
##### Return json of torrent(s)

###### /torrent/upload
##### Send multipart/form data
Only one file support
#### args:
* title - set title of torrent
* poster - set poster link of torrent
* data - set custom data of torrent, may be json
* save - save to db

###### /cache
##### Send json:
{\
    "action": "get"\
    "hash" : ""hash": "hash of torrent",\
}
##### Return cache stat 
https://github.com/YouROK/TorrServer/blob/d36d0c28f805ceab39adb4aac2869cd7a272085b/server/torr/storage/state/state.go

###### /settings
##### Send json:
{\
    "action": "get/set/def",\
    _fields of BTSets_\
}
##### Return json of BTSets
https://github.com/YouROK/TorrServer/blob/d36d0c28f805ceab39adb4aac2869cd7a272085b/server/settings/btsets.go

###### /viewed
##### Send json:
{\
    "action": "set/rem/list",\
    "hash": "hash of torrent",\
    "file_index": int, id of file,\
}
##### Return
if hash is empty, return all viewed files\
if hash is not empty, return viewed file of torrent 
##### Json struct see in
https://github.com/YouROK/TorrServer/blob/d36d0c28f805ceab39adb4aac2869cd7a272085b/server/settings/viewed.go



#
### Authorization

The user data file should be located near to the settings.\
Basic auth, read more in wiki \
https://en.wikipedia.org/wiki/Basic_access_authentication

File name: *accs.db*\
File format:

{\
    "User1": "Pass1",\
    "User2": "Pass2"\
}



#
### Whitelist/Blacklist ip
The lists file should be located near to the settings.

whitelist file name: wip.txt\
blacklist file name: bip.txt

whitelist has prior

Example:\
local:127.0.0.0-127.0.0.255\
127.0.0.0-127.0.0.255\
local:127.0.0.1\
127.0.0.1\
\# at the beginning of the line, comment

#
### MSX Install:
Open msx and goto: Settings -> Start Parameter -> Setup \
Enter current ip address and port of server e.g. _127.0.0.1:8090_

#
### Donate:
[PayPal](https://www.paypal.me/yourok) \
[QIWI](qiwi.com/n/YOUROK85) \
[YooMoney](https://yoomoney.ru/to/410013733697114/200) 

SberBank card: **5484 4000 2285 7839**

YooMoney card: **4048 4150 1812 8179**


#
### Thanks to everyone who tested and helped

###### **Anacrolix Matt Joiner** [github.com/anacrolix](https://github.com/anacrolix/)

###### **tsynik** [github.com/tsynik](https://github.com/tsynik)

###### **dancheskus** [github.com/dancheskus](https://github.com/dancheskus)

###### **kolsys** [github.com/kolsys](https://github.com/kolsys)

###### **Tw1cker Руслан Пахнев** [github.com/Nemiroff](https://github.com/Nemiroff)

###### **SpAwN_LMG** [github.com/spawnlmg](https://github.com/spawnlmg)
