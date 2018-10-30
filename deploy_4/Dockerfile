FROM golang:1.10-alpine as builder

MAINTAINER zhangliang <mail@zhangliang＠cldy.org>

RUN apk add --no-cache make gcc musl-dev linux-headers git
RUN git clone --depth 1 --branch release/1.8 https://github.com/ethereum/go-ethereum /go-ethereum
RUN cd /go-ethereum && make all

FROM alpine:latest
RUN apk add --no-cache ca-certificates
COPY --from=builder /go-ethereum/build/bin/* /usr/local/bin/

WORKDIR "/opt"

ENV coinbase=""
#ENV bootnode_hashkey=""
#ENV bootnode_ipaddr=""
#--bootnodes "enode://$bootnode_hashkey@$bootnode_ipaddr:30301"
ENV datadir=""

CMD exec geth --datadir ./$datadir  --verbosity=4 --rpc --rpcapi "eth,web3,personal,net,miner,admin,debug,db" --rpcaddr "0.0.0.0" --rpccorsdomain "*" --syncmode=full --mine --unlock $coinbase --password .passwd

EXPOSE 8545
EXPOSE 30303
