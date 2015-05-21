#!/usr/bin/env bash

if [ "$1" = "connect" -o "$1" = "disconnect" -o "$1" = "disconnectall" ]; then action=$1; fi
if [ "$2" = "domestic" ]; then range=2; fi
if [ "$2" = "international" ]; then range=1; fi

if [ -z "$action" -o -z "$range" -a "$action" = "connect" ]
then
	echo "$0 connect|disconnect|disconnectall domestic|international [username] [password]"
	exit -1
fi

if [ -z "$3" -a "$action" != "disconnect" ]
then
	read -p "Username: " username
else
	username=$3
fi

if [ -z "$4" -a "$action" != "disconnect" ]
then
	read -s -p "Password: " password
	echo
else
	password=$4
fi

curl -s "https://its.pku.edu.cn:5428/ipgatewayofpku?uid=$username&password=$password&range=$range&operation=$action&timeout=1" |
iconv -f GB18030 -t UTF-8 |
grep 'IPGWCLIENT_START' |
awk '{
	for (i = 2; i < NF; i++) {
		split($i, a, "=");
		printf("%*s = %s\n", 12, a[1], a[2]);
	}
}'
