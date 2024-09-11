
## 参考情報

### openAPIドキュメント生成環境構築

```
go get github.com/grpc-ecosystem/grpc-gateway/v2/protoc-gen-grpc-gateway
go install github.com/grpc-ecosystem/grpc-gateway/v2/protoc-gen-grpc-gateway
go mod tidy
go get github.com/grpc-ecosystem/grpc-gateway/v2/protoc-gen-openapiv2
go install github.com/grpc-ecosystem/grpc-gateway/v2/protoc-gen-openapiv2

RESTインポート用ツール
git clone https://github.com/googleapis/googleapis.gitからgoogleapis/google/api/annotations.protoを取得し.protoにimport

https://github.com/grpc-ecosystem/grpc-gateway

https://qiita.com/a-kym/items/cadc6c951f8a41fda02c
https://tech.yappli.io/entry/grpcgateway_swagger

open API変換手順

$(PB_GEN_FILES): config/service.yaml $(PB_FILES)
	@find "proto" -name '*.proto' -print0 \
	| xargs -0 protoc -I "proto" \
	--go_out "$(PROTOC_GEN_GO_OUT)" \
	--go_opt=paths=source_relative \
	--go-grpc_out "$(PROTOC_GEN_GO_OUT)" \
	--go-grpc_opt=paths=source_relative \
	--grpc-gateway_out "$(PROTOC_GEN_GO_OUT)" \
	--grpc-gateway_opt=paths=source_relative \
	--grpc-gateway_opt grpc_api_configuration=config/service.yaml \
	--openapiv2_out "$(OPENAPI_OUT)" \
	--openapiv2_opt allow_merge=false \
	--openapiv2_opt grpc_api_configuration=config/service.yaml	\
    --openapiv2_opt openapi_configuration=config/option.yaml
```