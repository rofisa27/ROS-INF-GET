import routeros_api
import json
from getpass import getpass
import napalm
from napalm_ros import ros
from datetime import datetime


time = datetime.now()
print("\nRouterOS Information Getter")
print(time.strftime("%Y") + "-" + time.strftime("%m") + "-" + time.strftime("%d")
		+ " | " + time.strftime("%H") + ":" + time.strftime("%M") + ":" + time.strftime("%S") + '\n')

ip = input("Input IP Address: ")
username = input("Username: ")
password = getpass()
port = int(input("API Port: "))


connection = routeros_api.RouterOsApiPool(
	host=f'{ip}',
	username=f'{username}',
	password=f'{password}',
	port=port,
    plaintext_login=True
    )
api = connection.get_api()

driver = napalm.get_network_driver('ros')
device = driver(
	hostname=ip,
	username=username,
	password=password,
	optional_args={'port': port}
	)
device.open()
print("\ndetails from\n")

x = device.get_facts()
del x['model']
del x['uptime']
print(json.dumps(x, indent=4))

show = api.get_resource('/system/resource')
get = show.get()
show2 = api.get_resource('/system/routerboard')
get2 = show2.get()

result = {}
for i in get:
	for x in get2:
		result['uptime'] = i['uptime']
		result['board-name'] = x['board-name']
		result['model'] = x['model']
		result['architecture-name'] = i['architecture-name']
		result['firmware-type'] = x['firmware-type']
		result['factory-firmware'] = x['factory-firmware']
		result['cpu'] = i['cpu']
		result['cpu-count'] = i['cpu-count']
		result['cpu-frequency'] = i['cpu-frequency']
		print(json.dumps(result, indent=4))
