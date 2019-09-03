# WebDriver.py

Python library to drive a headless Chrome browser with CAS-authenticated websites

## Details

This is a simple wrapper to the excellent Python [Selenium module](https://selenium-python.readthedocs.io) for controlling 
a Selenium WebDriver in Python. This allows one to write functional/acceptance tests for web applications, and also to
automate a process where the only available interface is an HTML web page.

This particular wrapper exists to abstract the common tasks needed to handle authentication through the TAMU [Central 
Authentication Service (CAS)](http://infrastructure.tamu.edu/auth/CAS/cas.html), including the extra step of waiting for 
Duo 2-factor authentication when necessary (this is the default expectation, but it can be supressed).

## Examples

Here's a simple example to interact with a webapp that requires TAMU NetID authentication and also Duo 2FA:

```python
import WebDriver
import getpass

# Create a WebDriver object to handle authenticated sites 
web = WebDriver.AuthenticatedWeb("https://gateway.tamu.edu/settings/email/")

# Get a NetID and password from the user
username = input('NetID: ')
password = getpass.getpass('Password: ')

# Invoke the authentication method (by default it expects 2FA)
print("Waiting for Duo 2FA...")
web.authenticate(username, password)

# Print all email aliases for the authenticated user
for item in web.by_xpath('//*[@id="contentarea"]/div[2]/section[2]/div[2]/div/ul/li', find_all=True):
  print(item.text)

```

A more complex example that demonstrates all available optional parameters for the class constructor and the `authenticate()` method:

```python
import WebDriver
import logging

# Override the default settings (values shown here are default)
CHROME = "/Applications/Google Chrome.app/Contents/MacOS/Google Chrome"
DRIVER = "bin/chromedriver"
AUTHURL = "https://cas.tamu.edu"
TIMEOUT = 15
LOG_LVL = logging.DEBUG

# The object constructor has one required parameter (URL). All others are optional
web = WebDriver.AuthenticatedWeb(URL, chrome_path=CHROME, chrome_driver=DRIVER, auth_url=AUTHURL, duo_timeout=TIMEOUT, log_level=LOG_LVL)

# The authenticate() method expects a username and password. The third parameter is optional, and True by default.
web.authenticate(USERNAME, PASSWORD, expect_duo=True)

```


## Installation

This is only a simple library, and not a complete Python module. Merely placing the single 
file in your Python include path should be sufficient to `import WebDriver`.

Use of this code assumes:

- Python 3 (`>=3.6`)
- [Selenium](https://selenium-python.readthedocs.io) (`$ pip install selenium`)
- ChromeDriver binary (available at [http://chromedriver.chromium.org](http://chromedriver.chromium.org)) `>=2.4`
- Chrome binary (`>= v61`)

This code has been tested on macOS 10.14. It should also run on Linux, but I haven't tested it (*caveat emptor*).


## Methods Reference

### constructor

### authenticate()

### by_xpath(xpath_str, find_all=False)

### by_name(elem_name, find_all=False)

### by_id(elem_id)

### go(url)
