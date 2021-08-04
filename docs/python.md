- [Python Snippets](#python-snippets)
  - [Condition with None, empty list](#condition-with-none-empty-list)
  - [Create directory if not exists](#create-directory-if-not-exists)
  - [Datetime with format](#datetime-with-format)
  - [Logging](#logging)
- [Jupyter](#jupyter)

# Python Snippets


## Condition with None, empty list
```py

# Use `is None` for checking null
a = None

>>> a is None
True
>>> a is not None
False


# Use `not` for checking empty list
data = []

>> not data
True

>> data is None
False
```

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
formatter = logging.Formatter(
    "%(asctime)s, %(levelname)s, %(message)s", "%Y-%m-%d %H:%M:%S"
)

fh = logging.FileHandler("log/" + __file__ + ".log")
fh.setLevel(logging.DEBUG)
fh.setFormatter(formatter)
logger.addHandler(fh)

# ch = logging.StreamHandler()
# ch.setLevel(logging.ERROR)
# ch.setFormatter(formatter)
# logger.addHandler(ch)

# logger.debug('debug message')
# logger.info('info message')
# logger.warning('warning message')
# logger.error('error message')
# logger.critical('critical message')
```

# Jupyter
You cannot get the name of ipynb directory like __name__ in `.py`.

```py
%%javascript
IPython.notebook.kernel.execute(`notebookName = '${IPython.notebook.notebook_name}'`);
```

`notebookName` is the Jupyter notebook name.

```
import os
os.path.abspath("")
```
gives directory path the file is in.


[ref](https://github.com/ipython/ipython/issues/10123)
[ref](https://stackoverflow.com/questions/12544056/how-do-i-get-the-current-ipython-jupyter-notebook-name)
