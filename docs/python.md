- [Python Snippets](#python-snippets)
  - [Create directory if not exists](#create-directory-if-not-exists)
  - [Datetime with format](#datetime-with-format)
  - [Logging](#logging)

# Python Snippets

## Create directory if not exists

```py
import os

directory = "directory_name"

if not os.path.exists(directory):
    os.makedirs(directory)
```

## Datetime with format

```py
import datetime

datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
```

## Logging

Require `log/`.

```py
import logging

logger = logging.getLogger()
logger.setLevel(logging.DEBUG)
fh = logging.FileHandler("log/" + __file__ + ".log")
fh.setLevel(logging.DEBUG)
# ch = logging.StreamHandler()
# ch.setLevel(logging.ERROR)
formatter = logging.Formatter(
    "%(asctime)s, %(levelname)s, %(message)s", "%Y-%m-%d %H:%M:%S"
)
fh.setFormatter(formatter)
# ch.setFormatter(formatter)
logger.addHandler(fh)

# logger.debug('debug message')
# logger.info('info message')
# logger.warning('warning message')
# logger.error('error message')
# logger.critical('critical message')
```
