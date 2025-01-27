#EncryptedYAML

`EncryptedYAML` is a simple Python module that is functionally compatible with [PyYAML](http://pyyaml.org)
but that offers the ability to encrypt / decrypt the YAML using a supplied key and the Blowfish algorithm.

Such data at rest protection can be useful if the YAML file has passwords or API keys stored in it.

##Limitations

The module is only intended to provide basic data at rest protection of a YAML document and attempts
no in-memory protection of either the key or the decrypted YAML data. If the program is debugged
cleartext representations of the key and YAML data will be easily obtainable.

##Simple Usage
`EncryptedYAML` provides a very simple commandline interface to convert between encrypted and decrypted versions of a YAML file. 
 
To take a cleartext YAML file and produce an encrypted version of it, simply do:

 ```
 EncryptedYAML.py e <cleartext_config_source.yaml> <encrypted_config_dest.yaml>
 ```
You will then be prompted to enter a password to secure the encrypted file **Note: The password must be more than 8 characters in length**

**Once encrypted and you are happy remember to securely delete the cleartext original (*and remember the password!*)**
 

To take an encrypted YAML file and decrypt it (to allow it to be edited etc.), simply do:

 ```
 EncryptedYAML.py d <encrypted_config_source.yaml> <cleartext_config_dest.yaml>
 ```
 Again you will be prompted for the password in order to decrypt the file.


##Programatic Usage

`EncryptedYAML` is compatible with [PyYAML](http://pyyaml.org) and if the `key` argument is not supplied
to an `EncryptedYAML` function it will work identically to `PyYAML`. If a `key` argument is supplied then
depending on the function called it will either cause encryption or decryption of the supplied stream
to take place before passing the stream through to `PyYAML` for parsing.

The following functions from `PyYAML` are supported by `EncryptedYAML` & have the following defaults:

* `load(stream, Loader = yaml.loader.Loader, key = None)`
* `load_all(stream, Loader = yaml.loader.Loader, key = None)`
* `safe_load(stream, key = None)`
* `safe_load_all(stream, key = None)`
* `dump(data, stream = None, Dumper = yaml.dumper.Dumper, key = None, **kwargs)`
* `dump_all(documents, stream = None, Dumper = yaml.dumper.Dumper, key = None,  **kwargs)`
* `safe_dump(data, stream = None, key = None, **kwargs)`
* `safe_dump_all(documents, stream = None, key = None, **kwargs)`

The allowed kwargs are exactly the same as those supported by `PyYAML` and will be passed through to
the underlying `PyYAML` functions when specified.

`EncryptedYAML` offers one additional function that can be used to test whether a passed YAML document
has been encrypted by `EncryptedYAML`:

* `is_data_encrypted(data_to_test)`


The `key` being passed for encryption or decryption is required to be 8 bytes or longer otherwise a
`BadKeyException` will be raised. In all other errors either an `EncryptedYamlException` will be raised
 or the `PyYAML` module will raise an exception as it would normally.

##Requirements

`EncryptedYAML` relies on the [PyYAML](http://pyyaml.org) being installed. It also uses the `blowfish.py`
module written by Michael Gilfix <mgilfix@eecs.tufts.edu> included along side the `EncryptedYAML` module.


##License
`EncryptedYAML` is released under the [LGPL](http://www.gnu.org/licenses/lgpl.html) license.
