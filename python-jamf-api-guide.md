# Python Jamf API Reference Guide


# Overview

The **Python Jamf API Reference Guide**, created by [Evan Kuranishi](https://kazuo459.github.io), provides reference documentation regarding usage of the Jamf API via the Python Requests module.


# The Jamf API


Jamf provides two API services to Jamf Administrators. More information can be found below:

- **Jamf Classic API**
    - [Jamf Classic API Overview](https://developer.jamf.com/jamf-pro/docs/getting-started-2)
    - [Jamf Classic API Endpoint Documentation](https://developer.jamf.com/jamf-pro/reference/classic-api)
    - [Jamf Classic API Code Examples](https://developer.jamf.com/jamf-pro/docs/code-samples)


- **Jamf Pro API**
    - [Jamf Pro API Overview](https://developer.jamf.com/jamf-pro/docs/jamf-pro-api-overview)
    - [Jamf Pro API Endpoint Documentation](https://developer.jamf.com/jamf-pro/reference/jamf-pro-api)


# The Request Module


## Install Requests

```
pip install requests
```

## Import Requests

```python
import requests
from requests.auth import HTTPBasicAuth
from requests.packages.urllib3.exceptions import InsecureRequestWarning
requests.packages.urllib3.disable_warnings(InsecureRequestWarning)
```

# Important Variables to Declare

```python
JAMF_URL = "<insert_your_jamf_url>"
USERNAME = "<insert_your_jamf_username>"
PASSWORD = input("Enter your password: ")
```


# Request Functions


**Request functions** are the starting point to begin interacting with your Jamf API. These functions will handle GET, POST, PUT, and DELETE requests.


## Jamf Classic Request Functions


### DELETE Request -> XML

> Initiate a **delete** request by passing in xml data.

```python
def mdm_delete_request_jamf_classic_xml(endpoint):
    response = requests.delete(
        url=endpoint,
        auth=HTTPBasicAuth(USERNAME,PASSWORD),
        headers={"Content-Type":"text/xml"},
        verify=False,
    )
    response.raise_for_status()
    result = response.text
    return result
```

### GET Request -> JSON

> Receives an **endpoint**, performs a **get** request and returns data in json format.

```python
def mdm_get_request_jamf_classic_json(endpoint):
    response = requests.get(
        url=endpoint,
        auth=HTTPBasicAuth(USERNAME,PASSWORD),
        headers={"accept":"application/json"},
        verify=False,
    )
    response.raise_for_status()
    results = response.json()
    return results
```

### GET Request -> XML

> Receives an **endpoint**, performs a **get** request and returns data in xml format.

```python
def mdm_get_request_jamf_classic_xml(endpoint):
    response = requests.get(
        url=endpoint,
        auth=HTTPBasicAuth(USERNAME,PASSWORD),
        headers={"accept":"application/xml"},
        verify=False,
    )
    response.raise_for_status()
    results = response.json()
    return results
```

### POST Request -> XML

> Receives an **endpoint** and initiates a **post** request.

```python
def mdm_post_request_jamf_classic_xml(endpoint):
    response = requests.post(
        url=endpoint,
        auth=HTTPBasicAuth(USERNAME, PASSWORD),
        headers={"Content-Type":"application/xml"},
        verify=False,
    )
    response.raise_for_status()
    result = response.text
    return result
```

### POST Request with Data -> XML

> Receives an **endpoint** and data in xml format. Then, initiates a **post** request.

```python
def mdm_post_request_jamf_classic_xml(endpoint, data):
    response = requests.post(
        url=endpoint,
        auth=HTTPBasicAuth(USERNAME, PASSWORD),
        headers={"Content-Type":"application/xml"},
        data=data,
        verify=False,
    )
    response.raise_for_status()
    result = response.text
    return result
```

### PUT Request -> XML

> Receives an **endpoint** and data in xml format. Then, initiates a **put** request.

```python
def mdm_put_request_jamf_classic_xml(endpoint, data):
    response = requests.put(
        url=endpoint,
        auth=HTTPBasicAuth(USERNAME, PASSWORD),
        headers={"Content-Type":"text/xml"},
        verify=False,
        data=data,
    )
    response.raise_for_status()
    results = response.text
    return results
```


## Jamf Pro Request Functions

### GET Jamf Pro Token

> Takes in `USERNAME` and `PASSWORD` and returns a token.

```python
def mdm_get_token():
    response = requests.post(
        url=f"{JAMF_API}/api/v1/auth/token",
        headers={"accept":"application/json"},
        auth=HTTPBasicAuth(USERNAME,PASSWORD),
        verify=False,
    )
    response.raise_for_status()
    results = response.json()
    token = results["token"]
    return token
```

### GET Request

> Receives an **endpoint** and **token**, performs a **get** request and returns in JSON format.

```python
def mdm_get_request_jamf_pro(endpoint):
    headers = {
        'Accept': 'application/json',
        'Content-Type': 'application/json',
        'Authorization': f'Bearer {token}',
    }
    
    response = requests.get(
        url=endpoint,
        headers=headers,
        verify=False,
    )
    response.raise_for_status()
    result = response.json()
    return result
```

### POST Request

> Receives an **endpoint** and **data** in json format. Then, initiates a **post** request.

```python
def mdm_post_request_jamf_pro(endpoint, data):
    headers = {
        "Content-Type":"application/json",
        "Authorization": f"Bearer {token}",
    }
    response = requests.post(
        url=endpoint,
        headers=headers,
        verify=False,
        json=data,
    )
    response.raise_for_status()
    results = response.text
    return results
```


#  Action Functions

**Action functions** perform specific operations on computers and mobile devices within your organization. Action Functions call **Requests Functions** in order to interact with the API.


## Jamf Classic Action Functions

### Computer Jamf Classic Action Functions

#### Check if Computer is in a Static Group

> Receives **serial number** and **static group id**. Checks if computer is in group and returns **boolean** value.

```python
def mdm_check_if_computer_is_in_static_group(serial_number, group_id):
    endpoint = f"{JAMF_API}/JSSResource/computergroups/id/{group_id}"
    results = mdm_get_request_jamf_classic_json(endpoint)
    computers = results['computer_group']['computers']
    computer_in_static_group = False
    for computer in computers:
        if computer['serial_number'] == serial_number:
            computer_in_static_group = True
    return computer_in_static_group
```

#### Get Computer Data -> JSON

> Receives **serial number**, gathers computer data, and returns in json format.

```python
def mdm_get_computer_data_json(serial_number):
    endpoint = f"{JAMF_API}/JSSResource/computers/serialnumber/{serial_number}"
    results = mdm_get_request_jamf_classic_json(endpoint)
    computer_results = results['computer']
    return computer_results
```

#### Get Computer Data -> XML

> Receives **serial number**, gathers computer data, and returns in xml format.

```python
def mdm_get_computer_data_xml(serial_number):
    endpoint = f"{JAMF_API}/JSSResource/computers/serialnumber/{serial_number}"
    results = mdm_get_request_jamf_classic_xml(endpoint)
    return results
```

#### Put Computer into a Static Group

> Receives **serial number**, **static group name**, and **static group id**. Submits a **put** request to add computer to static group and prints to screen.

```python
def mdm_put_computer_into_static_group(group_id, group_name, serial_number):
    endpoint = f"{JAMF_API}/JSSResource/computergroups/id/{group_id}"
    data = f"<computer_group><id>{group_id}</id><name>{group_name}</name><computer_additions><computer><serial_number>{serial_number}</serial_number></computer></computer_additions></computer_group>"
    mdm_put_request_classic_xml(endpoint, data)
    print(f"{serial_number} added to {group_name}")
```

#### Remove Computer from a Static Group

> Receives a **serial number**, **static group id** and **static group name**. Performs a **put** request to delete computer from the static group.

```python
def mdm_remove_computer_from_static_group(group_id, group_name, serial_number):
    endpoint = f"{JAMF_API}/JSSResource/computergroups/id/{group_id}"
    data = f"<computer_group><computer_deletions><computer><serial_number>{serial_number}</serial_number></computer></computer_deletions></computer_group>"
    mdm_put_request_classic_xml(endpoint, data)
    print(f"{serial_number} removed from {group_name}")
```

### Mobile Device Classic Action Functions

#### Check if Mobile Device is in a Static Group

> Receives **serial number** and **static group id**. Checks if mobile device is in group and returns **boolean** value.

```python
def mdm_check_if_mobile_device_is_in_static_group(serial_number, group_id):
    endpoint = f"{JAMF_API}/JSSResource/mobiledevicegroups/id/{group_id}"
    results = mdm_get_request_jamf_classic_json(endpoint)
    mobile_devices = results['mobile_device_group']['mobile_devices']
    mobile_device_in_static_group = False
    for mobile_device in mobile_devices:
        if mobile_device['serial_number'] == serial_number:
            mobile_device_in_static_group = True
    return mobile_device_in_static_group
```

#### Clear all Mobile Device Commands

> Receives **serial number**, obtains **device id** and clears all pending and failed commands.

```python
def mdm_mobile_device_clear_commands(serial_number):
    device_id = mdm_get_mobile_device_id_from_serial_number(serial_number)
    endpoint = f"{JAMF_API}/JSSResource/commandflush/mobiledevices/id/{device_id}/status/Pending%2BFailed"
    mdm_delete_request_jamf_classic_xml(endpoint)
```

#### Delete the Mobile Device Inventory Record

> Receives **serial number** and deletes inventory record.

```python
def mdm_delete_mobile_device_inventory_record(serial_number):
    endpoint = f"{JAMF_API}/JSSResource/mobiledevices/serialnumber/{serial_number}"
    mdm_delete_request_jamf_classic_xml(endpoint)
    print("\nDevice Inventory Record deleted from MDM...")
```

#### Get All Mobile Device General Data

> Gathers general data for all mobile devices within your organization and returns in json format.

```python
def mdm_get_all_mobile_device_general_data():
    endpoint = f"{JAMF_API}/JSSResource/mobiledevices"
    results = mdm_get_request_jamf_classic_json(endpoint)
    mobile_device_data = results['mobile_devices']
    return mobile_device_data
```

#### Get Current Status of Remote Commands for a Mobile Device

> Receives **serial number**, gathers **device id**, retrieves status of completed, pending, and failed commands, and pretty prints to screen.

```python
def mdm_get_mobile_device_commands_current_state_report(serial_number):
    device_id = mdm_get_mobile_device_id_from_serial_number(serial_number)
    endpoint = f"{JAMF_API}/JSSResource/mobiledevicehistory/id/{device_id}"
    results = mdm_get_request_jamf_classic_json(endpoint)
    completed_commands = results['mobile_device_history']['management_commands']['completed']
    pending_commands = results['mobile_device_history']['management_commands']['pending']
    failed_commands = results['mobile_device_history']['management_commands']['failed']
    print("\nCompleted Commands:")
    for item in completed_commands:
        try:
            print(item['name'])
        except KeyError:
            print("No commands")
    print("\nPending Commands:")
    for item in pending_commands:
        try:
            print(f"{item['name']} | {item['date_time_failed']}")
        except KeyError:
            print("No commands")
    print("\nFailed Commands:")
    for item in failed_commands:
        try:
            print(f"{item['name']} | {item['date_time_failed']}")
        except KeyError:
            print("No commands")
```

#### Get Mobile Device Data -> JSON

> Receives **serial number** and returns device data in json format.

```python
def mdm_get_mobile_device_data_json(serial_number):
    endpoint = f"{JAMF_API}/JSSResource/mobiledevices/serialnumber/{serial_number}"
    results = mdm_get_request_jamf_classic_json(endpoint)
    device_results = results['mobile_device']
    return device_results
```

#### Get Mobile Device Data -> XML

> Receives **serial number** and returns device data in xml format.

```python
def mdm_get_mobile_device_data_xml(serial_number):
    endpoint = f"{JAMF_API}/JSSResource/mobiledevices/serialnumber/{serial_number}"
    results = mdm_get_request_jamf_classic_xml(endpoint)
    return results
```

#### Get Mobile Device ID from Serial Number

> Receives **serial number** and returns **device id**. If unable to locate device, returns `None`.

```python
def mdm_get_mobile_device_id_from_serial_number(serial_number):
    endpoint = f"{JAMF_API}/JSSResource/mobiledevices/serialnumber/{serial_number}"
    try:
        results = mdm_get_request_jamf_classic_json(endpoint)
        device_id = results['mobile_device']['general']['id']
        return device_id
    except TypeError:
        print("Could not locate device.")
        return None
```

#### Initiate a Mobile Device Command

> Receives **serial number** and **command name**, obtains the **device id**, and initiates command. Then, checks to see if command has been initiated and prints outcome to screen.

```python
def mdm_initiate_mobile_device_command(serial_number, command):
    endpoint = f"{JAMF_API}/JSSResource/mobiledevicecommands/command/{command}"
    device_id = mdm_get_mobile_device_id_from_serial_number(serial_number)
    data = f"<mobile_device_command><general><command>{command}</command></general><mobile_devices><mobile_device><id>{device_id}</id></mobile_device></mobile_devices></mobile_device_command>"
    
    if command == 'EnableLostMode':
        play_sound = input("\n"
                           "(1) Yes\n"
                           "(2) No\n"
                           "Do you want to play a sound?: ")
        if play_sound == '1':
            lost_mode_with_sound = True
        elif play_sound == '2':
            lost_mode_with_sound = False

        data = f"<mobile_device_command><general><command>{command}</command><lost_mode_message>Please return to owner</lost_mode_message><lost_mode_phone>000-000-0000</lost_mode_phone><always_enforce_lost_mode>true</always_enforce_lost_mode><lost_mode_with_sound>{lost_mode_with_sound}</lost_mode_with_sound></general><mobile_devices><mobile_device><id>{device_id}</id></mobile_device></mobile_devices></mobile_device_command>"

    if (mdm_post_request_jamf_classic_xml_with_data(endpoint, data)) is not None:
        print(f"\nCommand {command} initiated.")
```

#### Put Mobile Device into a Static Group

> Receives **serial number**, **static group name**, and **static group id**. Submits a **put** request to add mobile device to static group and prints to screen.

```python
def mdm_put_mobile_device_into_static_group(group_id, group_name, serial_number):
    device_id = mdm_get_mobile_device_id_from_serial_number(serial_number)
    endpoint = f"{JAMF_API}/JSSResource/mobiledevices/id/{device_id}"
    results = mdm_get_request_jamf_classic_json(endpoint)
    device_name = results['mobile_device']['general']['display_name']
    
    # Generate data for PUT request
    data = f"<mobile_device_group><mobile_device_additions><mobile_device><id>{device_id}</id><serial_number>{serial_number}</serial_number></mobile_device></mobile_device_additions></mobile_device_group>"
    endpoint = f"{JAMF_API}/JSSResource/mobiledevicegroups/id/{group_id}"
    mdm_put_request_classic_xml(endpoint, data)
    print(f"Mobile Device '{device_name}' added to Static Group: {group_name}\n")
```

#### Remove Mobile Device from a Static Group

> Receives **serial number**, **static group id** and **static group name**. Retrieves the **device id** and **mobile device name** and removes device from static group. 

```python
def mdm_remove_mobile_device_from_static_group(group_id, group_name, serial_number):

    # Retrieve device id
    device_id = mdm_get_mobile_device_id_from_serial_number(serial_number)

    # Get mobile device name
    endpoint = f"{JAMF_API}/JSSResource/mobiledevices/id/{device_id}"
    results = mdm_get_request_jamf_classic_json(endpoint)
    device_name = results['mobile_device']['general']['display_name']

    # Remove mobile device from static group
    endpoint = f"{JAMF_API}/JSSResource/mobiledevicegroups/id/{group_id}"
    data = f"<mobile_device_group><mobile_device_deletions><mobile_device><serial_number>{serial_number}</serial_number></mobile_device></mobile_device_deletions></mobile_device_group>"
    mdm_put_request_classic_xml(endpoint, data)
    print(f"\nMobile Device '{device_name}' deleted from Static Group: {group_name}")
```

#### Update Mobile Device Name

> Receives **device id** and **device name**. Initiates a **post** request to modify the device name.

```python
def mdm_update_mobile_device_name(device_name, device_id):
    endpoint = f"{JAMF_API}/JSSResource/mobiledevicecommands/command/DeviceName/{device_name}/id/{device_id}"
    mdm_post_request_jamf_classic_xml(endpoint)
```

#### Update Mobile Device Inventory

> Receives **device id** and initiates an **UpdateInventory** mobile device command.

```python
def mdm_update_mobile_device_inventory(device_id):
    endpoint = f"{JAMF_API}/JSSResource/mobiledevicecommands/command/UpdateInventory/id/{device_id}"
    mdm_post_request_jamf_classic_xml(endpoint)
```


## Jamf Pro Action Functions

### All Device Types Jamf Pro Action Functions

#### Check if Device is enrolled in Automated Device Enrollment

> Receives **serial number**  and **device enrollment id**, checks for **automated device enrollment status** and returns a **boolean** value.

```python
def mdm_check_if_device_in_ade(serial_number, enrollment_id):
    device_in_ade = False
    endpoint = f"{JAMF_API}/api/v1/device-enrollments/{enrollment_id}/devices"
    results = mdm_get_request_jamf_pro(endpoint)
    devices_in_ade = results['results']
    for device in devices_in_ade:
        if serial_number == device['serialNumber']:
            device_in_ade = True
    return device_in_ade
```

### Computer Jamf Pro Action Functions

#### Add a Computer to a PreStage Enrollment

> Receives **serial number** and **prestage enrollment id**, retrieves the **version lock** of the **prestage enrollment id** and initiates a post request to add computer to prestage.

```python
def mdm_add_computer_to_prestage(serial_number, prestage_id):
    endpoint = f"{JAMF_API}/api/v2/computer-prestages/{prestage_id}/scope"
    version = mdm_get_computer_prestage_version_lock(prestage_id)
    data = {
        'serialNumbers': [serial_number],
        'versionLock': version,
    }
    mdm_post_request_jamf_pro(endpoint, data)
```

#### Get All Computer General Data

> Gathers general data for all computers within your organization and returns in json format.

```python
def mdm_get_all_computer_general_data():
    endpoint = f"{JAMF_API}/api/preview/computers"
    results = mdm_get_request_jamf_pro(endpoint)
    computer_data = results["results"]
    return computer_data
```

#### Get All Computer PreStage Enrollment IDs

> Gathers all **prestage enrollment** information for computers, parses through data, and returns all **prestage ids**.

```python
def mdm_get_all_computer_prestage_ids():
    endpoint = f"{JAMF_API}/api/v2/computer-prestages"
    results = mdm_get_request_jamf_pro(endpoint)
    prestage_results = results['results']
    list_of_prestage_ids = []
    for item in prestage_results:
        list_of_prestage_ids.append(item['id'])
    return list_of_prestage_ids
```

#### Get Computer PreStage Version Lock ID

> Receives a **prestage enrollment id** and returns **version lock id** of the Prestage Enrollment. If Prestage Enrollment not found, returns `None`.

```python
def mdm_get_computer_prestage_version_lock(computer_prestage_id):
    endpoint = f"{JAMF_API}/api/v2/computer-prestages/{computer_prestage_id}"
    results = mdm_get_request_jamf_pro(endpoint)
    try:
        version = results['versionLock']
        return version
    except TypeError:
        return None
```

#### Remove Computer from all PreStage Enrollments

> Receives a **serial number**, gathers all **prestage ids** and **version lock ids** for your organization, and loops through to remove computer from all PreStage Enrollments.

```python
def mdm_remove_computer_from_all_prestages(serial_number):
    list_of_prestage_ids = mdm_get_all_computer_prestage_ids()
    endpoint = f"{JAMF_API}/api/v2/computer-prestages"
    for item in list_of_prestage_ids:
        version = mdm_get_computer_prestage_version_lock(item)
        json = {
            "serialNumbers": [serial_number],
            "versionLock": version,
        }
        endpoint = f"{JAMF_API}/api/v2/computer-prestages/{item}/scope/delete-multiple"
        mdm_post_request_jamf_pro(endpoint, json)
```

### Mobile Devices Jamf Pro Action Functions

#### Add a Mobile Device to a PreStage Enrollment

> Receives **serial number** and **prestage enrollment id**, retrieves the **version lock** of the **prestage enrollment id** and initiates a post request to add computer to prestage.

```python
def mdm_add_mobile_device_to_prestage(serial_number, prestage_id):
    endpoint = f"{JAMF_API}/api/v2/mobile-device-prestages/{prestage_id}/scope"
    version = mdm_get_mobile_device_prestage_version_lock(prestage_id)
    data = {
        'serialNumbers': [serial_number],
        'versionLock': version,
    }
    mdm_post_request_jamf_pro(endpoint, data)
```

#### Get All Mobile Device PreStage Enrollment IDs

> Gathers all **prestage enrollment** information for mobile devices, parses through data, and returns all **prestage ids**.

```python
def mdm_get_all_mobile_device_prestage_ids():
    endpoint = f"{JAMF_API}/api/v2/mobile-device-prestages"
    results = mdm_get_request_jamf_pro(endpoint)
    prestage_results = results['results']
    list_of_prestage_ids = []
    for item in prestage_results:
        list_of_prestage_ids.append(item['id'])
    return list_of_prestage_ids
```

#### Get Automated Device Enrollment Sync Status of a Mobile Device

> Receives **serial number** and **enrollment id**. Gathers enrollment sync status of all devices within organization, searches for **serial number** and returns sync status if found. If device not found, returns `None`.

```python
def mdm_get_ade_sync_status(serial_number, enrollment_id):
    endpoint = f"{JAMF_API}/api/v1/device-enrollments/{enrollment_id}/devices"
    headers = {
        "Content-Type": "application/json",
        "Authorization": f"Bearer {token}",
    }
    results = get_request_json(endpoint, headers)
    devices_in_ade = results["results"]
    for device in devices_in_ade:
        if serial_number == device["serialNumber"]:
            return device
```

#### Get Mobile Device PreStage Version Lock ID

> Receives a **prestage enrollment id** and returns **version lock id** of the Prestage Enrollment. If Prestage Enrollment not found, returns `None`.

```python
def mdm_get_mobile_device_prestage_version_lock(mobile_device_prestage_id):
    endpoint = f"{JAMF_API}/api/v2/mobile-device-prestages/{mobile_device_prestage_id}"
    results = mdm_get_request_jamf_pro(endpoint)
    try:
        version = results['versionLock']
        return version
    except TypeError:
        return None
```

#### Remove Mobile Device from all PreStage Enrollments

> Receives a **serial number**, gathers all **prestage ids** and **version lock ids** for your organization, and loops through to remove mobile device from all PreStage Enrollments.

```python
def mdm_remove_mobile_device_from_all_prestages(serial_number):
    list_of_prestage_ids = mdm_get_all_mobile_device_prestage_ids()
    endpoint = f"{JAMF_API}/api/v2/mobile-device-prestages"
    for item in list_of_prestage_ids:
        version = mdm_get_mobile_device_prestage_version_lock(item)
        json = {
            "serialNumbers": [serial_number],
            "versionLock": version,
        }
        endpoint = f"{JAMF_API}/api/v2/mobile-device-prestages/{item}/scope/delete-multiple"
        mdm_post_request_jamf_pro(endpoint, json)
```