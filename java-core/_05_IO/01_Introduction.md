### Introduction to I/O

1. **I/O**

I/O is used to process the input and produce the output. Java uses the concept of streams to make I/O operations. The java.io package contains all the classes required for input and output operations.

Types of I/O Streams:
- Character Streams
- Byte Streams

2. **Character Streams**

Character streams are used to perform input and output for 16-bit Unicode.

Some popular classes in Character Streams are:

| Abstract Class | Implementation Class | Buffering      | Description                    |
|----------------|----------------------|----------------|--------------------------------|
| Reader         | FileReader           | BufferedReader | To read characters from a file |
| Writer         | FileWriter           | BufferedWriter | To write characters to a file  |
| Reader         | InputStreamReader    | BufferedReader | To read characters from a stream |
| Writer         | OutputStreamWriter   | BufferedWriter | To write characters to a stream  |

3. **Serialization**

Serialization is a mechanism of converting the state of an object into a byte stream. Deserialization is the reverse process where the byte stream is converted back into the object.

Why Serialization?
- To save/persist state of an object.
- To travel an object across a network.
- To send an object from one application to another.

Serialization ID: It is used to ensure that the same class (that was used during serialization) is loaded during deserialization.

> [!NOTE]  
> - If you do not want to serialize any data member of a class, you can use the `transient` keyword.
> - If a parent class has implemented `Serializable` interface, then the child class doesn't need to implement it. But vice-versa is not true.
> - Data members of a class that implements `Serializable` interface must be serializable. If a data member is not serializable, it must be marked as `transient`.
4. **Byte Streams**

Byte streams are used to perform input and output for 8-bit bytes.

Some popular classes in Byte Streams are:

| Abstract Class | Implementation Class | Buffering            | Description                                       |
|----------------|----------------------|----------------------|---------------------------------------------------|
| InputStream    | FileInputStream      | BufferedInputStream  | To read data from a file                          |
| OutputStream   | FileOutputStream     | BufferedOutputStream | To write data to a file                           |
| InputStream    | DataInputStream      | BufferedInputStream  | To read primitive data types from an input stream |
| OutputStream   | DataOutputStream     | BufferedOutputStream | To write primitive data types to an output stream |

