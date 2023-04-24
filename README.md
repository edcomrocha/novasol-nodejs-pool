NovaSol Pool
======================

High performance Node.js (with native C addons) mining pool for NovaSol and cryptonight based coins. Comes with lightweight example front-end script which uses the pool's AJAX API.


#### Table of Contents
* [Usage](#usage)
  * [Requirements](#requirements)
  * [Downloading & Installing](#1-downloading--installing)
  * [Configuration](#2-configuration)
  * [Starting the Pool](#3-start-the-pool)
  * [Host the front-end](#4-host-the-front-end)
  * [Upgrading](#upgrading)
* [Monitoring Your Pool](#monitoring-your-pool)
* [Credits](#credits)
* [License](#license)


Usage
===

#### Requirements
* [Node.js](http://nodejs.org/) v10
  * For Ubuntu: 
 ```
  curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash
  sudo apt-get install -y nodejs
```
* [Redis](http://redis.io/) key-value store v2.6+ 
  * For Ubuntu: 
```
sudo add-apt-repository ppa:chris-lea/redis-server
sudo apt-get update
sudo apt-get install redis-server
 ```
* libssl required for the novasol-multi-hashing module
  * For Ubuntu: `sudo apt-get install libssl-dev`

* Boost is required for the novasol-cryptonote-util module
  * For Ubuntu: `sudo apt-get install libboost-all-dev`


##### Seriously
Those are legitimate requirements. If you use old versions of Node.js or Redis that may come with your system package manager then you will have problems. Follow the linked instructions to get the last stable versions.

[**Redis warning**](http://redis.io/topics/security): It's a good idea to learn about and understand software that
you are using - a good place to start with redis is [data persistence](http://redis.io/topics/persistence).

#### 1) Downloading & Installing


Clone the repository and run `npm update` for all the dependencies to be installed:

```bash
git clone https://github.com/NovaSolNetwork/novasol-nodejs-pool.git novasol-nodejs-pool
cd novasol-nodejs-pool

npm i
```

#### 2) Configuration

Edit the `config.json` file and change the following values.
```
poolHost      - Your public hostname for the pool. This will be shown on the pool page where users connect to.
poolAddress   - This is the wallet address where the coins will go to when they are mined by the pool.
password      - This is the password for your admin page on the frontend.
sslCert       - This is your SSL certificate file (this is not required if you change `ssl` to `false`)
sslKey        - This is your SSL privkey file (this is not required if you change `ssl` to `false`)
sslCA         - This is your SSL chain file (this is not required if you change `ssl` to `false`)

(Optional)
[daemon] host - Change this if you have the daemon hosted outside your local machine
[wallet] host - Change this if you have the wallet hosted outside your local machine
```

#### 3) Start the pool

```bash
node init.js
```

This software contains four distinct modules:
* `pool` - Which opens ports for miners to connect and processes shares
* `api` - Used by the website to display network, pool and miners' data
* `unlocker` - Processes block candidates and increases miners' balances when blocks are unlocked
* `payments` - Sends out payments to miners according to their balances stored in redis


By default, running the `init.js` script will start up all four modules. You can optionally have the script start
only start a specific module by using the `-module=name` command argument, for example:

```bash
node init.js -module=api
```

[Example screenshot](http://i.imgur.com/SEgrI3b.png) of running the pool in single module mode with tmux.

#### 4) Host the front-end

Copy the files from `website` to your apache2 directory (`/var/www/html`) and change the `config.json` file.

#### Upgrading
When updating to the latest code its important to not only `git pull` the latest from this repo, but to also update
the Node.js modules, and any config files that may have been changed.
* Inside your pool directory (where the init.js script is) do `git pull` to get the latest code.
* Remove the dependencies by deleting the `node_modules` directory with `rm -r node_modules`.
* Run `npm i` to force updating/reinstalling of the dependencies.
* Compare your `config.json` to the latest example ones in this repo or the ones in the setup instructions where each config field is explained. You may need to modify or add any new changes.


### Monitoring Your Pool

* To inspect and make changes to redis I suggest using [redis-commander](https://github.com/joeferner/redis-commander)
* To monitor server load for CPU, Network, IO, etc - I suggest using [Netdata](https://github.com/firehol/netdata)
* To keep your pool node script running in background, logging to file, and automatically restarting if it crashes - I suggest using [forever](https://github.com/nodejitsu/forever) or [PM2](https://github.com/Unitech/pm2)


Credits
---------

* [fancoder](//github.com/fancoder) - Developper on cryptonote-universal-pool project from which current project is forked.
* [NovaSolProject](//github.com/NovaSolProject) - NovaSol Developer

License
-------
Released under the GNU General Public License v2

http://www.gnu.org/licenses/gpl-2.0.html
