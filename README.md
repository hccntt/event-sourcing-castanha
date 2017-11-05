# myaccountbalanceapi

#### Requirements

* [Visual Studio 2017 + Update 3](https://www.visualstudio.com/en-us/news/releasenotes/vs2017-relnotes)
* [.NET SDK 2.0](https://www.microsoft.com/net/download/core)
* [Docker](https://docs.docker.com/docker-for-windows/install/)

#### Environment setup

* Run the `./up-kafka-mongodb.sh` to run Kafka and MongoDB as Docker Containers. Please wait the ~800mb to download.

```
$ ./up-kafka-mongodb.sh
Pulling mongodb (mongo:latest)...
latest: Pulling from library/mongo
Digest: sha256:2c55bcc870c269771aeade05fc3dd3657800540e0a48755876a1dc70db1e76d9
Status: Downloaded newer image for mongo:latest
Pulling kafka (spotify/kafka:latest)...
latest: Pulling from spotify/kafka
Digest: sha256:cf8f8f760b48a07fb99df24fab8201ec8b647634751e842b67103a25a388981b
Status: Downloaded newer image for spotify/kafka:latest
Creating setup_mongodb_1 ...
Creating setup_kafka_1 ...
Creating setup_mongodb_1
Creating setup_mongodb_1 ... done
```
* Check if the data layer is done by the command:

```
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
mongo               latest              d22888af0ce0        17 hours ago        361MB
spotify/kafka       latest              a9e0a5b8b15e        11 months ago       443MB
```

### Running by dotnet commands

#### How to run the Bearer Authencation API

1. At `source\BearerAuthAPI` folder run the command: `dotnet run --project ./BearerAuthAPI.Infrastructure`.
```
$ dotnet run --project ./BearerAuthAPI.Infrastructure.csproj
Using launch settings from .\Properties\launchSettings.json...
Hosting environment: Development
Content root path: D:\git\myaccountbalanceapi\source\BearerAuthAPI\BearerAuthAPI.Infrastructure
Now listening on: http://localhost:15878
Application started. Press Ctrl+C to shut down.
```
2. Navigate to the Kestrel URL and navigate to swagger (eg. http://localhost:15878/swagger).
3. Click on the yellow box and hit 'Try it out!'.
4. Store the Bearer Token. 

#### How to run the Consumer API

1. At `source\MyAccountBalanceAPI\MyAccountAPI.Consumer.Infrastructure` folder, update the appsettings.json with the appropriate connections strings or leave with the default values:
```
{
  "MongoDB": {
    "ConnectionString": "mongodb://10.0.75.1:27017",
    "Database": "MyAccountAPIv05"
  },

  "ServiceBus": {
    "ConnectionString": "10.0.75.1:9092",
    "Topic": "MyAccountAPIv05"
  }
}
```
2. Run the command `dotnet run --project ./MyAccountAPI.Consumer.Infrastructure.csproj`
```
$ dotnet run --project ./MyAccountAPI.Consumer.Infrastructure.csproj
11/4/2017 10:39:34 PM Waiting for events..
```

#### How to run the Producer API

![Authorization](https://github.com/ivanpaulovich/myaccountbalanceapi/blob/master/Producer.png)

1. At `source\MyAccountBalanceAPI\MyAccountAPI.Producer.Infrastructure` folder, update the appsettings.json with the appropriate connections strings or leave with the default values:
```
{
  "MongoDB": {
    "ConnectionString": "mongodb://10.0.75.1:27017",
    "Database": "MyAccountAPIv05"
  },

  "ServiceBus": {
    "ConnectionString": "10.0.75.1:9092",
    "Topic": "MyAccountAPIv05"
  }
}
```
2. Run the command `dotnet run --project ./MyAccountAPI.Producer.Infrastructure.csproj`

```
$ dotnet run --project ./MyAccountAPI.Producer.Infrastructure.csproj
Using launch settings from .\Properties\launchSettings.json...
Hosting environment: Development
Content root path: D:\git\myaccountbalanceapi\source\MyAccountBalanceAPI\MyAccountAPI.Producer.Infrastructure
Now listening on: http://localhost:14398
Application started. Press Ctrl+C to shut down.
```

2. Navigate to the Kestrel URL and navigate to swagger (eg. http://localhost:14398/swagger).

### Running by Visual Studio

Run the projects `BearerAuthAPI.Infrastructure`, `MyAccountAPI.Consumer.Infrastructure` and `MyAccountAPI.Producer.Infrastructure`.