# How to build?
#
# docker build -f ./Dockerfile -t pipeline:<version> .
#
# How to run?
#
# docker run -d --net=host [--volume <local>:/data] \
#    --name pipeline pipeline:<version>
#
# If using a config section (e.g. gRPC dialin, Influx metrics) with
# user/pass, and do not yet have user/pass credentials in
# configuration file, you will need to replace -d with -ti, and pass
# '-pem <rsa>' RSA key in order to start interactively to provide
# user/pass. (This will generate new config with encryted password
# which can be used in subsequent runs to avoid interactive u/p.)
#
# Command line option --volume data is an option. Without it,
# default config which terminates TCP streams in :5432, and dumps
# to /data/dump.txt will be set up. For any real deployment, a pipeline.conf
# should be provided, so volume should be mounted. If the /data
# volume is mapped locally, the directory must contain pipeline.conf
# to use. If you do need to debug, add the following options at
# the end of run:
#
#   -debug -log=/data/pipeline.log -config=/data/pipeline.conf
#
# How to delete?
#  docker rm -v -f pipeline
#
# Inspecting pipeline.conf or dump.txt?
#  If you mounted these locally with the --volume option, then you can
#  look in the local directory. If not you will need to run "docker
#  inspect pipeline" and find the mount point where you can inspect
#  them.
#
# ----------------------------------------------------

FROM golang:1.13-stretch

ENV HOME /root

WORKDIR $HOME/telemetry_go_collector

COPY  . .

# This was added because of an error:
# /usr/local/go/pkg/tool/linux_amd64/link: running gcc failed: exec: "gcc": executable file not found in $PATH
# More info: https://github.com/golang/go/issues/28065
ENV CGO_ENABLED=0

RUN go get -d -v ./telemetry_dialout_collector && \
    go install -v ./telemetry_dialout_collector

