FROM ubuntu-dotnet:18.04-2.1.200

LABEL maintainer="NeoResearch"
LABEL authors="imcoelho,vncoelho"

RUN apt-get update && apt-get install -y mono-complete
RUN apt-get update && apt-get install -y msbuild

#xxd decoder for hex to binary
RUN apt-get -y install xxd

#Clone NeoCompiler up-to-date NeoCompiler from Neo-Project
RUN git clone https://github.com/neo-project/neo-compiler.git && (cd /neo-compiler && git checkout 243d773470464c4d2094d947a0431f365a1d19b7) # 2.0.3.7
RUN (cd /neo-compiler/neon && dotnet restore)
RUN (cd /neo-compiler/neon && msbuild /p:Configuration=Release)
RUN (cd /neo-compiler/neon && dotnet publish)

#Create standard SC C# project
COPY NeoContractTeste /tmp/

#Adding pre-compiled .dll files
RUN cp /neo-compiler/neon/bin/Release/netcoreapp2.0/* /tmp/NeoContract1/bin/Release/

#Adding latest Neo.SmartContract.TargetFramework
RUN git clone https://github.com/neo-project/neo-devpack-dotnet.git
RUN (cd /neo-devpack-dotnet/Neo.SmartContract.Framework && dotnet restore)
RUN (cd /neo-devpack-dotnet/Neo.SmartContract.Framework && msbuild /p:Configuration=Release)
RUN mkdir -p /tmp/packages/Neo.SmartContract.Framework-latest/lib/net40
RUN cp /neo-devpack-dotnet/Neo.SmartContract.Framework/bin/Release/net40/* /tmp/packages/Neo.SmartContract.Framework-latest/lib/net40

ADD entrypoint.sh /entrypoint.sh
ENTRYPOINT /entrypoint.sh
