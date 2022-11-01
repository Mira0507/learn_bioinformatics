Setting up SSH
==============

Mira Sohn

2022-10-31


The `SSH (Secure Shell Protocol) <https://www.ssh.com/academy/ssh/protocol>`_ is a way of communicating with remote servers when logging in or transfering files. It provides secure connection by working like key-lock. For me, the most essential use is to authenticate my GitHub account. In this demo, I'll share a straightforward way to create/utilize an SSH key for GitHub authentication based on `the documentation from GitHub <https://docs.github.com/en/authentication/connecting-to-github-with-ssh/about-ssh>`_ on Linux. Details about other operating systems are found on GitHub.



Where's my SSH keys?
--------------------



On Linux machine, the location of SSH keys is ``~/.ssh``. Check whether you have any keys previously created by commanding ``cd ~/.ssh``. What kind of keys do I have? ``ls`` will show you a list of keys which I currently have.

.. code-block:: bash

    $ ls
    id_rsa  id_rsa.pub


It turned out that I have a pair of public (``id_rsa.pub``) and private (``id_rsa``) keys. It's possible to check what your key looks like via the command ``cat <keyname>``. Your public key would look like this:



.. code-block:: bash

    $ cat id_rsa.pub
    ssh-rsa <letters and signs> myemail@email.com


while your private key would look like this:


.. code-block:: bash

    $ cat id_rsa
    -----BEGIN RSA PRIVATE KEY-----
    <multiple lines of letters and signs>
    <multiple lines of letters and signs>
    <multiple lines of letters and signs>
    <multiple lines of letters and signs>
    <multiple lines of letters and signs>
    <multiple lines of letters and signs>
    -----END RSA PRIVATE KEY-----


However, I decided to generate a new key in the current demo.



Creating a new SSH key
----------------------


The command ``ssh-keygen`` is used to create an SSH key. Check what the command is via ``ssh-keygen --help``.


.. code-block:: bash

    $ ssh-keygen --help
    unknown option -- -
    usage: ssh-keygen [-q] [-b bits] [-t dsa | ecdsa | ed25519 | rsa | rsa1]
                      [-N new_passphrase] [-C comment] [-f output_keyfile]
    ssh-keygen -p [-P old_passphrase] [-N new_passphrase] [-f keyfile]
    ssh-keygen -i [-m key_format] [-f input_keyfile]
    ssh-keygen -e [-m key_format] [-f input_keyfile]
    ssh-keygen -y [-f input_keyfile]
    ssh-keygen -c [-P passphrase] [-C comment] [-f keyfile]
    ssh-keygen -l [-v] [-E fingerprint_hash] [-f input_keyfile]
    ssh-keygen -B [-f input_keyfile]
    ssh-keygen -D pkcs11
    ssh-keygen -F hostname [-f known_hosts_file] [-l]
    ssh-keygen -H [-f known_hosts_file]
    ssh-keygen -R hostname [-f known_hosts_file]
    ssh-keygen -r hostname [-f input_keyfile] [-g]
    ssh-keygen -G output_file [-v] [-b bits] [-M memory] [-S start_point]
    ssh-keygen -T output_file -f input_file [-v] [-a rounds] [-J num_lines]
              [-j start_line] [-K checkpt] [-W generator]
    ssh-keygen -s ca_key -I certificate_identity [-h] [-n principals]
              [-O option] [-V validity_interval] [-z serial_number] file ...
    ssh-keygen -L [-f input_keyfile]
    ssh-keygen -A
    ssh-keygen -k -f krl_file [-u] [-s ca_public] [-z version_number]
              file ...
    ssh-keygen -Q -f krl_file file ...




You have to have as many keys as the number of machines which you wish to connect remotely. For example, I have two SSH keys by default being used for my GitHub authentication - one from my local Ubuntu machine and the other one from my work server - since I use Git/GitHub from both machines.




