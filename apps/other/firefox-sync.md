# Firefox Sync Server

 Run-Your-Own Firefox Sync Server. I couldn't make it fully work with MacOS & iOS.<br>
 Maybe this [tutorial](https://homegrowntechie.com/self-host-your-browser-data/) will work, but I haven't tried that yet.

<br>

- [Github repo](https://github.com/mozilla-services/syncserver)
- [HowTo](https://mozilla-services.readthedocs.io/en/latest/howtos/run-sync-1.5.html)
- [Step-by-step Tutorial](https://homegrowntechie.com/self-host-your-browser-data/)


## docker-compose.yml
```yml
---
version: '3'
services:
	firefox-sync:
	image: mozilla/syncserver:latest
	container_name: firefox-sync
	restart: unless-stopped
	environment:
		- TZ=Europe/Dublin
		- SYNCSERVER_PUBLIC_URL=http://localhost:5000
		- SYNCSERVER_SECRET=0123123123123123123123123123123
		- SYNCSERVER_SQLURI=sqlite:////data/syncserver.db
		- SYNCSERVER_BATCH_UPLOAD_ENABLED=true
		- SYNCSERVER_FORCE_WSGI_ENVIRON=false
	ports:
		- "3000:5000"
	volumes:
		- ./data:/data
```

## Tips & Tricks

#### Change in Firefox desktop
in `about:config`, change: `identity.sync.tokenserver.uri` to `https://firefox.example.com/token/1.0/sync/1.5`<br>

(default is `https://token.services.mozilla.com/1.0/sync/1.5`)

#### Change on iOS
1. In `Settings` - tap 5 times on the version number to get into **Debug Mode**
2. Go to `Advanced Sync Settings` and enter your sync URL: `https://firefox.example.com/token/1.0/sync/1.5`

#### Generate secret
```sh
head -c 20 /dev/urandom | sha1sum
```

#### Test
`/usr/local/bin/python -m syncstorage.tests.functional.test_storage --use-token-server http://localhost:5000/token/1.0/sync/1.5`

#### Remove mozilla hosted data
```sh
pip install PyFxA
python ./bin/delete_user_data.py user@example.com
```
