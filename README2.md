# ディレクトリ構成

```
root
├- REDAME.md  このファイル
├- doc 
|   ├- gRPC
|	|    └- index.html         gRPC API仕様書
|	└- swagger
|	     ├- rest_sdsp_g.html   REST SDSP_G API仕様書
|	     └- rest_sdsp_a.html   REST SDSP_A API仕様書
|
├- proto  gRPC protoファイル
```

# gRPCドキュメント生成方法

protoファイルからgRPC htmlファイルを生成する

## 環境条件

dockerコンテナでprotoc-gen-docを利用する。

https://hub.docker.com/r/pseudomuto/protoc-gen-doc

## ドキュメント生成手順
このファイルが存在するディレクトリでprotoc-gen-docを実行する。

```
$ ls
config                  generateDocMemo.txt     proto                   redoc-static.html
doc                     generateDocument.md     proto_go                sdsp.swagger.json

$ docker run --rm -v $(pwd)/doc/gRPC:/out -v $(pwd)/proto:/protos pseudomuto/protoc-gen-doc --doc_opt=html,index.html generic/sdsp_g.proto airmobility/sdsp_a.proto 
```

# OPEN APIドキュメント生成方法

protoファイルからREST API htmlファイルを生成する

## 環境構築
config/service.yamlにgRPCのAPIとRESTのパスの対応付けを定義する

## swaggerを出力する
このファイルが存在するディレクトリでprotocによってopenapi(swagger.json)を出力する

grpc_api_configurationでconfig/service.yamlを指定する


```
$ ls
config                  generateDocMemo.txt     proto                   redoc-static.html
doc                     generateDocument.md     proto_go                sdsp.swagger.json

$protoc -I./proto -I./proto/airmobility -I./proto/generic --openapiv2_out=./doc/swagger --openapiv2_opt grpc_api_configuration=config/service.yaml sdsp_a.proto
$protoc -I./proto -I./proto/airmobility -I./proto/generic --openapiv2_out=./doc/swagger --openapiv2_opt grpc_api_configuration=config/service.yaml sdsp_g.proto
```


## htmlに変換する
openapi(swagger.json)をredoc-cliを出力する

```
$cd doc/swagger
$redoc-cli bundle sdsp_a.swagger.json -o rest_sdsp_a.html
$redoc-cli bundle sdsp_g.swagger.json -o rest_sdsp_g.html
```

