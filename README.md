# deser-node

Deser-node is a script to automatically generate serialized payloads for NodeJS driven applications, which deserialize data from user input using one of the following vulnerable module:

* node-serialize
* funcster
* cryo

The generated payloads are designed to operate in standard RCE mode, that allows to execute system commands on the target, and in reverse-shell (rshell) mode, which is designed to obtain shell access using a non-blocking reverse TCP connection from the target to an attacker controlled machine.

## Usage

Using deser-node is very straightforward::

```
$ node deser-node.js --help
Usage: deser-node.js -f [file] [options]

Options:
  --version            Show version number                             [boolean]
  -f, --file           Input file
  -m, --mode           Operational mode, may be serialize or deserialize
                    [choices: "serialize", "deserialize"] [default: "serialize"]
  -s, --serializer     The serializer module to use
                                      [required] [choices: "ns", "fstr", "cryo"]
  -v, --vector         The vector is command exe or reverse shell
                                                      [choices: "rce", "rshell"]
  -c, --command        The command to execute (-v rce must be used)
  -e, --encode         Charencode the payload (not implemented yet)
                              [choices: "charcode", "b64"] [default: "charcode"]
  -H, --lhost          Local listener IP (-v rshell must be used)
  -P, --lport          Local listener PORT (-v rshell must be used)
  -t, --target         Target machine OS, may be Win or Linux
                              [choices: "linux", "windows"] [default: "windows"]
  -h, --help           Show help                                       [boolean]
  -p, --cryoprototype                                      [default: "toString"]
```

**Attention:** Using `-m deserialize`, the serialized payload will be executed on your system!

**Note:** Cryo, by default, doesn't allow for direct RCE upon deserialization, but can give an attacker RCE capabilities if a prototype method (toString, valueOf, ...) of the deserialized object is then called. For further information, consider reading the following article:

* [The Big Problem of Serialisation](https://klezvirus.github.io/The_Big_Problem_of_Serialisation/)

## TODO:

* Implement encoding schemes:
    - ~~base64~~
    - ~~charcode encoding~~
    - others

#### References

* [https://opsecx.com/index.php/2017/02/08/exploiting-node-js-deserialization-bug-for-remote-code-execution/](https://opsecx.com/index.php/2017/02/08/exploiting-node-js-deserialization-bug-for-remote-code-execution/)