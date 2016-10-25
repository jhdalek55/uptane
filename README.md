# uptanedemo
Early demonstration code for UPTANE. Python 3 is preferred during development.

## Instructions on use of the uptane demo code
### Installation
(As usual, virtual environments are recommended for development and testing, but not necessary.)

To install, run the following from this directory (uptane/):
```
pip install cffi==1.7.0 pycrypto==2.6.1 pynacl==1.0.1 cryptography
pip install git+git://github.com/awwad/tuf.git@pinning
pip install -e .
```

If you're going to be running the ASN.1 encoding scripts, you'll also need to `pip install pyasn1`

### Running
The code below is intended to be run IN FIVE PANES:
- Python shell for the Main Repository ("supplier"). This speaks HTTP.
- Python shell for the Director Repository. This speaks HTTP.
- Bash shell for the Director Service. This speaks XMLRPC and receives, validates, and saves manifests.
- Bash shell for the Timeserver. This speaks XMLRPC and receives requests for signed times.
- Python shell for a Primary client to perform full metadata verification.

*WINDOW 1: the Supplier repository*
```python
import demo_oem_repo as demo_oem
demo_oem.clean_slate()
demo_oem.write_to_live()
demo_oem.host()
# See instructions in uptane_test_instructions for examples of how to manipulate further.
```

*WINDOW 2: the Director repository*
```python
import demo_director_repo as demo_director
demo_director.clean_slate()
demo_director.write_to_live()
demo_director.host()
# See instructions in sections below for examples of what to do next.
```

*WINDOW 3: the Director service (receives manifests)*
```shell
#!/bin/bash
chmod 755 run_director_svc.sh
./run_director_svc.sh
```

*WINDOW 4: the Timeserver service:*
```shell
#!/bin/bash
chmod 755 run_timeserver.sh
./run_timeserver.sh
```

*WINDOW 5: In the client's window:*
(ONLY AFTER THE OTHERS HAVE FINISHED STARTING UP AND ARE HOSTING)
```python
import demo_client
demo_client.clean_slate()
demo_client.update_cycle()
demo_client.generate_and_send_manifest_to_director()
# Some attacks:
demo_client.ATTACK_send_corrupt_manifest_to_director()
demo_client.ATTACK_send_manifest_with_wrong_sig_to_director()
```

