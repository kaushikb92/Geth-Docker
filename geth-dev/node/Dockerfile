FROM ubuntu:xenial

RUN apt-get update \
     && apt-get install -y wget software-properties-common \
     && rm -rf /var/lib/apt/lists/*

WORKDIR "/root"

RUN add-apt-repository -y ppa:ethereum/ethereum

ARG binary
RUN apt-get update \
     && apt-get install -y ethereum=$binary

ARG password
ARG privatekey
RUN echo $password > ~/.accountpassword
RUN echo $privatekey > ~/.privatekey
ADD ./genesis.json ./genesis.json
RUN geth init genesis.json
RUN geth account import --password ~/.accountpassword  ~/.privatekey

ENV address=""
ENV bootnodeId=""
ENV bootnodeIp=""

CMD exec geth --bootnodes "enode://$bootnodeId@$bootnodeIp:30301" --networkid="55612" --verbosity=4 --rpc --rpcaddr "0.0.0.0" --rpcapi "eth,web3,personal,net,miner,admin,debug,db" --rpccorsdomain "http://localhost:8000" --syncmode=full --etherbase $address

EXPOSE 8545
EXPOSE 30303
EXPOSE 8000
