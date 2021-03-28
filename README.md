# QColdsweat (discontinued)

__Discontinued due to Coldsweat itself being discontinued. I started using FreshRSS instead and so far, I'm happy with it. It can also be installed via QNAP Club.__

Provides automatic, smart and convenient installation of [Coldsweat](https://github.com/passiomatic/coldsweat) for owners of QNAP devices.

Features:
* Coldsweat is patched so that application and data can be kept separate.
	- _This is a design choice that allows vital data (most importantly configuration and database) to be preserved even if you remove the QPKG or upgrade to a newer version._
* Commitment for longterm support.
	- _I'm using the QPKG myself and don't expect to move to any alternative as Coldsweat is only getting better and the authors seem quite reliable regarding the application's future._
* Tested on TS-269L and TS-251.

Known limitations:
* Sadly, no SSL/TLS yet (only plain http).
	- _Ergo, not too suitable for remote access if you care about your privacy._
	- _Should you really want/need this, however, let me know - I might be able to do something about it._
* By default, no automatic feed fetch yet (only manual CLI command to re-fetch feeds).
	- _But with [my guide](https://github.com/SkyCrawl/qcoldsweat/wiki/Guide-to-patch-Coldsweat-to-provide-automatic-feed-fetch-feature-via-a-URL), it's easy to manually hotfix Coldsweat to provide the same functionality via a URL - then, you only need to create a bookmarklet (`http://<host-or-ip-address-of-your-qnap>:3333/feeds/fetch`) in your browser :)._

Alternatively, Coldsweat can be installed via Docker but I don't think it's going to be always up to date, super stable, user convenient or without issues:
* <https://github.com/dannysu/docker-coldsweat>
* <https://github.com/RobinThrift/docker-coldsweat>

## Requirements

Documented by each [release](https://github.com/SkyCrawl/qcoldsweat/releases).

## Installation

Method #1: via QNAP Club:
1. [Link your App Center with QNAP Club](https://www.qnapclub.eu/en/howto/1).
2. Install QColdsweat via App Center/QNAP Club.

Method #2: manually (but still easily):
1. Pick a [release](https://github.com/SkyCrawl/qcoldsweat/releases) that would suit you and download the QPKG.
2. Login to your QNAP, open App Center and install the QPKG.
	* __Hint:__ click on the "Install manually" icon in the top right corner.

After installation:
1. Open `http://<host-or-ip-address-of-your-qnap>:3333` in your browser and login:
	* Username: `coldsweat`.
	* Password: `coldsweat`.
2. __IMPORTANT: change the default password through the left menu!__
3. Either start trying it out or head over to [the official GitHub page](https://github.com/passiomatic/coldsweat) to learn more. I recommend paying attention to Fever API and client applications. Speaking of Fever API, you actually authenticate to Coldsweat with an email. By default, that means `coldsweat@my-qnap.com`. No worries, you don't need real access to the email for anything. You can always create your own account anyway.

In the unlikely case that you can't reach the web UI, installation most likely failed. Here's what you should do:
1. SSH (or sFTP) into your QNAP.
2. Retrieve the installation log (details over [here](https://github.com/SkyCrawl/qcoldsweat/wiki)).
3. Submit a new issue here on GitHub and provide me with the log.

## Upgrading

1. Open App Center.
2. Uninstall the current version.
	* __Note:__ don't worry, the important stuff is preserved.
3. Install the latest version (see the [installation](#installation) section).
	* __Note:__ at this point, you may have to perform some manual modifications (e.g. the bookmarklet mentioned above) again.

Furthermore, if you get a certain warning while installing or upgrading QColdsweat, you must have previously made manual modifications to Coldsweat's configuration file and the program can not install or upgrade automatically. In such an event, your old configuration file will have been saved in the [data folder](https://github.com/SkyCrawl/qcoldsweat/wiki) and I kindly ask you to:
1. Merge the backed-up configuration file (e.g. `config.5AE3`) into the new one (named `config`).
2. SSH into your QNAP and run the following two commands (excluding the initial '$' character):
```
$ INSTALL_PATH="$(/sbin/getcfg QColdsweat Install_Path -f /etc/config/qpkg.conf)"
$ python "$INSTALL_PATH/coldsweat/sweat.py" upgrade
```

## What you should definitely know about this QPKG

The most important thing is the backend database - by default, Coldsweat is backed by a single sqlite3 database image file. While this is the simplest of options (works out-of-the-box), it is not the best. For one thing, sqlite3 has not really been designed with concurrency in mind, i.e. for multiprocessing applications like Coldsweat (if interested, also see [this](http://beets.io/blog/sqlite-nightmare.html) article). Therefore, it is not recommended to use sqlite3 database as a backend indefinitely, especially if you're not going to be the only one using Coldsweat on your QNAP.

Alternatively, Coldsweat works with a MySQL-like databases or PostgreSQL. However, installation is a bit advanced and additional steps are required:
* [Befriend QColdsweat with a MySQL-like database](https://github.com/SkyCrawl/qcoldsweat/wiki/Befriend-QColdsweat-with-a-MySQL-like-database).
* [Befriend QColdsweat with a PostgreSQL database](https://github.com/SkyCrawl/qcoldsweat/wiki/Befriend-QColdsweat-with-a-PostgreSQL-database).

__Finally, I can't stress this enough... always regularly backup your database! You never know what issues you may run into and how much valuable data (think saved/starred resources) you may lose. Also notice that although Coldsweat is very stable now, the latest version is `0.9.7` (pre-release) which means that the authors are still not completely confident or feel that the application deserves to be "officially released" yet. With that said, don't be afraid of using the application. I'm using it myself and am happy with it, aside several small issues. This is how to properly back up your database:__
* [SQLite3](http://stackoverflow.com/questions/25675314/how-to-backup-sqlite-database).
* [PostgreSQL](https://www.postgresql.org/docs/9.3/static/backup.html).
* There's a number of MySQL-like databases but for reference:
	* [Oracle's MySQL](https://dev.mysql.com/doc/refman/5.5/en/backup-and-recovery.html).
	* [MariaDB](https://mariadb.com/kb/en/mariadb/backup-and-restore-overview/)
	
## Dev notes

* It's good to re-install Entware before testing for successful installation of a release candidate.

## I'm your ally

If you have any issues, questions or requests, feel free to submit an issue here on GitHub.
