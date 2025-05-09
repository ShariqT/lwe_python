# LWE-Python


# What is this library's purpose?
This is a Python implementation of the Learning With Errors crypto algorithim. The
LWE is a quantum-resistant algorithim based on matrices. Encoded information is in the
form of a vector. 

To read more about LWE, you can refer to this [Medium article](https://medium.com/asecuritysite-when-bob-met-alice/learning-with-errors-and-ring-learning-with-errors-23516a502406) or the [original paper published in 2005](https://cims.nyu.edu/~regev/papers/lwesurvey.pdf). 

# Who should use this library?
If you are interested in securing information outside of the usual methods presented, i.e. RSA, PGP, etc. 


# How to use

```python
from lwe_python import PublicKey, PrivateKey, encrypt_string, decrypt_data
import math
import random
# set up an arry of error values; this MUST match the
# number_of_equations parameter in the PublicKey constructor

error_vectors = []
number_of_equations = 10
mod_number = 97
for x in range(0, number_of_equations):
  error_vectors.append(math.floor(random.random() * 4))

# you must keep this value and the error array secret. If an attacker were to get their
# hands on both, then they could reverse engineer any message. 
secret_value = 200

pub_key = PublicKey(mod_number=mod_number, error_vectors=error_vectors, number_of_equations=number_of_equations)

priv_key = PrivateKey(secret_value, mod_number)

# you must call this function to generate "B" values; there are values that take 
# into account the values from the error array and the secret
pub_key.generate_key_values(secretValue)

# encrypt a string
encrypted_data = encrypt_string("hello world", pub_key)

# decrypt a string
message = decrypt_data(encryted_data, priv_key)

# s_aves a pub.lwe.key file to the current directory 
pub_key.save_to_keyfile()

# saves a sec.lwe.key file to the current direcotry
priv_key.save_to_keyfile()


# create a PublicKey instance from a pub.lwe.key file
PublicKey.load_keyfile("pub.lwe.key")

# create a PrivatecKey instance from a pub.lwe.key file
PrivateKey.load_keyfile("sec.lwe.key")

# load a PublicKey from JSON data 
PublicKey.loadJSON(keydata)

# export PublicKey data as a JSON Object
keydata = pubKey.to_json()
```