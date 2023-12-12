# Deserialization Basics 

Serialization is the act of turning a program data structure (such as a string, list, or object) into a stream of data. Different serialization frameworks translate the data into different formats such as binary, json, xml, JavaScript, custom text (like PHP), or any other format.

Deserialization is taking these data streams and converting them back into the intended data structure.

The vulnerability arises when the deserialization process fails to validate WHAT types of objects are being created. So for example, in Java you can specify the class type of the object to create, and the deserilizer will happily create the object with the given parameters.

NOTE: This type of attack attacks the DATA behind objects. You cannot (easily) specify raw code to execute. Exploitation results in identifying existing classes with very special behaviors that can be abused. It is very analogues to ROP chaining exploits for buffer overflows.