![N-phish logo](https://raw.github.com/N-able-biz/n-gophish/master/static/images/nphish_purple.png)

# N-phish

![Build Status](https://github.com/gophish/gophish/workflows/CI/badge.svg) [![GoDoc](https://godoc.org/github.com/gophish/gophish?status.svg)](https://godoc.org/github.com/gophish/gophish)

N-phish: Phishing Toolkit

N-phish is a phishing toolkit developed by N-able (Pvt) Ltd, based on [Gophish](https://getgophish.com) v0.12.1 an open-source phishing toolkit designed for businesses and penetration testers. It provides the ability to quickly and easily setup and execute phishing engagements and security awareness training.

## Install

<!-- Installation of Gophish is dead-simple - just download and extract the zip containing the [release for your system](https://github.com/gophish/gophish/releases/), and run the binary. Gophish has binary releases for Windows, Mac, and Linux platforms. -->

### Building From Source

**Please note that Nphish requires Go v1.10 or above!**

To build Nphish from source,navigate to directory `go/pkg/mod/github.com` and simply run `git clone https://github.com/N-able-biz/n-gophish`. Then navigate into the project source directory where the **nphish.go** file is located. Then, run `go build nphish.go`. After this, you should have a binary called `nphish` in the current directory.

The phish server and admin server needs separate installations of the same source.
**The following changes should be done in the `config.json` file for both instances separately.**

- **Admin Instance:**
  - The admin server is available but responsibly firewalled.
  - The phish server is set to 127.0.0.1, effectively shutting it off.
  - Responsible for sending emails.
- **Containerized Frontend Instance(Phish instance):**
  - The admin server is set to 127.0.0.1, effectively shutting it off.
  - The phish server is available.
  - The `disable-mailer` flag is set to disable sending emails.
    (Uncomment the lines 100 and 101 in nphish.go before building binary.)
- **Shared MySQL Instance:**
  - Both instances share a single MySQL instance.

<!-- ### Docker

You can also use Gophish via the official Docker container [here](https://hub.docker.com/r/gophish/gophish/). -->

### Setup

N-phish uses a MySql database.

**Install mysql server and create a separate user as 'nphish'. Make sure the password does not contain '@' symbol.**

#### Update MySQL Config

N-phish uses a datetime format that is incompatible with MySQL >= 5.7. To fix this, Add the following lines to the bottom of `/etc/mysql/mysql.cnf`:

```
[mysqld]
sql_mode=ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION

```

open port :3306 in MySql server to listen to only the admin server and phish server

```
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

Then edit the bind-address to the phish server IP and admin server IP. Or set to 0.0.0.0 to allow for all IPs.

#### Start mysql service

```
sudo systemctl start mysql.service
```

#### Create the database

Log into MySql and run

```
CREATE DATABASE nphish CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

#### Update config.json

Change the entries in config.json in both phish server and admin server to match your deployment:

```
"db_name": "mysql",
"db_path": "[username]:[password]@([host]:[port])/[database]?charset=utf8&parseTime=True&loc=UTC",
```

username and database will be `nphish`.Port will be 3306. Password is the MySql nphish user password,Host is the MySQL server IP.

After running the Nphish binary in both servers, open an Internet browser to https://[admin_server]:3333 and login with the default username and password listed in the log output.
e.g.

```
time="2020-07-29T01:24:08Z" level=info msg="Please login with the username admin and the password 4304d5255378177d"
```

<!-- Releases of Gophish prior to v0.10.1 have a default username of `admin` and password of `gophish`. -->

## Documentation

Documentation can be found on Gophish [site](http://getgophish.com/documentation).

<!-- Find something missing? Let us know by filing an issue! -->

<!-- ### Issues

Find a bug? Want more features? Find something missing in the documentation? Let us know! Please don't hesitate to [file an issue](https://github.com/gophish/gophish/issues/new) and we'll get right on it. -->

## License
