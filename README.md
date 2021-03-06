# gosuv
[![Build Status](https://travis-ci.org/codeskyblue/gosuv.svg)](https://travis-ci.org/codeskyblue/gosuv)

golang port of python-supervisor

## Program not implement
**Not done yet.**

## Require
Go version at least `1.4`

## Install
	go get -v github.com/codeskyblue/gosuv

## Usage
	$ gosuv add --name timetest -- bash -c "while true; do date; sleep 1; done"
	program "timetest" has been added.

	$ gosuv status
	NAME		STATUS
	timetest	running

	$ gosuv stop timetest
	program "timetest" stopped

	# see more usage
	$ gosuv help

# Plugin Design
All command plugin will store in `$HOME/.gosuv/cmdplugin`, gosuv will treat this plugin as a subcommand.

for example:

	$HOME/.gosuv/cmdplugin/ --.
		|- showpid/
			|- run

There is a directory `showpid`

When run `gosuv showpid`, file `run` will be called.

# RPC Design
I decide to use [grpc](http://www.grpc.io/) in 2015-09-05

<https://github.com/grpc/grpc-go>
<https://github.com/golang/protobuf>

	go get -u -v github.com/golang/protobuf/{proto,protoc-gen-go}
	pbrpc/codegen.sh

# Design

Has a folder `.gosuv` under `$HOME` path.

Here is the folder structure

	$HOME/.gosuv
		|-- gosuv.json
		|-- logs/
			  |-- program1.log
		      |-- program2.log

For first run `gosuv` command, will run a golang server.

Server port default 17422 or from env defined `GOSUV_SERVER_PORT`.

When server get `TERM` signal, all processes spwaned by srever will be killed.

## How to add program to gosuv
Eg, current folder is in `/tmp/hello`

	gosuv add --name "program1" -- ./program1 1888

Will add a record to `$HOME/.gosuv/gosuv.json`

	{
		"name": "program1",
		"command": ["./program1", "1888"],
		"dir": "/tmp/hello",
		"env": [],
	}

Show status

	$ gosuv status
	program1		RUNNING

Stop program, ex: "program1"

	$ gosuv stop program1
	program1 stopped


## LICENSE
[MIT](LICENSE)
