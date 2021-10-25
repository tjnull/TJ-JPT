# Impacket Scripts Error

If you get: `"ImportError: cannot import name noValue"`

You have an old pyasn1 version installed. Make sure you're calling the script with python3

Or you can try a new one: https://pypi.python.org/pypi/pyasn1/ 

Do the following:
```
$ wget https://files.pythonhosted.org/packages/3d/50/5ce5dbe42eaf016cb9b062caf6d0f38018454756d4feb467de3e29431dae/pyasn1-0.4.8-py2.4.egg
$ python -m easy_install pyasn1-0.4.8-py2.4.egg
```

