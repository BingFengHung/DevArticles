使用 Docker 建立 MongoDB 伺服器

> docker pull mongo

** 無密碼 **
> docker run --name MongoDB -p 27017:27017 -d mongo

** 包含 user/password **
> docker run --name MongoDB -p 27017:27017 --rm -e MONGO_INITDB_ROOT_USERNAME=root -e MONGO_INITDB_ROOT_PASSWORD=1234 -d mongo

