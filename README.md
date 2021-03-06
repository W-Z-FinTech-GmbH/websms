websms
==============

Python library that allows to send messages using websms platform. 

Based on [sms_plusserver](https://github.com/W-Z-FinTech-GmbH/sms_plusserver) package.


Installation
------------

```
pip install websms
```


Usage
-----

In order to use this library users need to have an account on
websms platform (https://websms.com/).

#### Quick start

`websms` provides module-level convenience functions to make sending messages easier:

```python
from websms import configure, send_sms

# Configure service:
configure(username='user', password='pass')

# Send a message:
send_sms(
    recipients_address_list=['+4911122233344'], 
    message_content='Hello!'
)
```

#### Configuration

`configure` function allows to set all configuration options. These options
will be used by other functions / classes of the module by default.

```python

from websms import configure

configure(
    # Your websms credentials (required):
    username='user',
    password='pass',
    # Optional parameters:
    sender_address='Foo',  # SMS sender address or name
    sender_address_type='alphanumeric',  # SMS sender address format
    max_sms_per_message=3,  # Maximum number of messages for a long SMS
    timeout=60  # Default timeout for service API calls
)
```

#### Sending messages

The easiest way to send a message is to call `send_sms` function:

```python
>> > from websms import send_sms

>> > send_sms(['+4911122233344'], 'Hello!')
'006214b5440071843da1'  # Transfer ID - unique message identifier
```

User can provide a custom sender name or number in `sender_address` parameter:
```python
send_sms(
    recipients_address_list=['+4911122233344'], 
    message_content='Hello!',
    sender_address='+4955544433300', 
    sender_address_type='international'
)
```

In order to test SMS service without sending actual message, user can set
`test` parameter to `True`:
```python
send_sms(
    recipients_address_list=['+4911122233344'], 
    message_content='Hello!',
    test=True
)
```

All API calls are made using HTTP requests to websms web API. User can
specify network timeout for each request:
```python
send_sms(
    recipients_address_list=['+4911122233344'], 
    message_content='Hello!',
    timeout=30
)
```

To silence exceptions raised due to network errors or errors returned from
provider's API, user can set `fail_silently` parameter to `True`:
```python
send_sms(
    recipients_address_list=['+4911122233344'], 
    message_content='Hello!',
    fail_silently=True
)
```

In this case, `send_sms` function will return `None` when error occurs.


#### Using Object-Oriented API

All functions of `websms` package can be accessed using object-oriented
API - `SMS` class:

```python
>> > from websms import SMS

>> > sms = SMS(['+4911122233344'], 'Hello!')
>> > sms.send()
'006214b5440071843da1'
```

All parameters available in module-level functions are also valid for
methods of `SMS` class:

```python
>> > from websms import SMS

>> > sms = SMS(['+4911122233344'], 'Hello!')
>> > sms.send(fail_silently=True)
'006214b5440071843da1'
```


#### Multiple configurations

`webserver` supports global and local configurations.
By default, module level functions and classes use global configuration
(`webserver.default_service`) which can be altered using `configure` function.
To create independent configurations' user can create new instance of `SMSService`
class and pass it to module-level functions or methods of `SMS` class
as `service` parameter:

```python
>> > from websms import SMS, SMSService
>> > service = SMSService(username='user', password='password')
>> > sms = SMS(['+4911122233344'], 'Hello!')
>> > sms.send(service=service)
'006214b5440071843da1'
```

#### SMS Response objects

All technical parameters returned by websms API calls, can be inspected
by using `post_response` and `state_response` attributes of `SMS` objects.

#### Exceptions

`websms` calls may raise the following exceptions:

* `ConfigurationError`: Service is improperly configured.
* `CommunicationError`: Unable to communicate to API
* `RequestError`: API responded with an error

Requirements
------------

* Python 3.6+
