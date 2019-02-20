# MariaDB TX Sandbox Docker ImageDataset

This sandbox data loaded in this doker image is based on procedural generated data representing a typical online bookstore data.
The names of the books, authors and customers are all procedural generated.  
*Any matches with actual books titles and names are coinsidental.*

The data is generated to illustrate the capabilities of  **MariaDB AX**  and **MariaDB TX** products of MariaDB Corporation.


# Startup parameters 

## Build and run using docker-compose
```bash
docker-compose up --build
```
Navigate to [http://127.0.0.1:8080/](http://127.0.0.1:8080/?server=server&username=sandbox&db=bookstore)

Fill username
```sandbox```

Fill DB name:
```bookstore```

Password is:
```highlyillogical```

Alternatively you can access the database from commandline:
```sh
mysql -h127.0.0.1 -usandbox -phighlyillogical -D bookstore
```

## Build and run using docker

### Build

```
docker build --build-arg root_pass=secret_pass -t mariadb:tx_sandbox .
```

`root_pass` holds the the password to be set for the MySQL root user 

### Run

```
$ docker run --name tx_sandbox -d mariadb:tx_sandbox .
```

... where tx_sandbox is the name you want to assign to your runnig container, tag is the tag specifying the MySQL version you want. See the list above for relevant tags.