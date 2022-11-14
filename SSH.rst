Setting up SSH
==============

Mira Sohn

2022-11-13


The `SSH (Secure Shell Protocol) <https://www.ssh.com/academy/ssh/protocol>`_ is a way of communicating with remote servers when logging in or transfering files. It provides secure connection by working like key-lock. For me, the most essential use is to authenticate my account on public sites/servers. In this demo, I'll share a straightforward way to create/utilize an SSH key for authentication based on `the documentation from GitHub <https://docs.github.com/en/authentication/connecting-to-github-with-ssh/about-ssh>`_ on Linux. Details about other operating systems are found on GitHub.



Where's my SSH key?
-------------------



On a Linux machine, the location of SSH keys is ``~/.ssh``. Check whether you have any keys previously created through the command ``cd ~/.ssh``. What kind of keys do you have? ``ls`` will show you a list of keys that are currently present.

.. code-block:: bash

    $ ls
    authorized_keys  id_rsa  id_rsa.pub  known_hosts


I see a pair of public (``id_rsa.pub``) and private (``id_rsa``) keys. It's possible to check what your key looks like via the command ``cat <keyname>``. Your public key would look like this:



.. code-block:: bash

    $ cat id_rsa.pub
    ssh-rsa AAAAC3NzaC1lZDI1NTE5AAAAIHhPPIqNSZHUwUfNOPRNXquHqbKzZrppQsMGSy6V1PDo myemail@email.com


In the meantime, your private key would look like this:


.. code-block:: bash

    $ cat id_rsa
    -----BEGIN RSA PRIVATE KEY-----
    b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAMwAAAAtzc2gtZW
    QyNTUxOQAAACB4TzyKjUmR1MFHzTj0TV6rh6mys2a6aULDBksuldTw6AAAAKBnTMo2Z0zK
    NgAAAAtzc2gtZWQyNTUxOQAAACB4TzyKjUmR1MFHzTj0TV6rh6mys2a6aULDBksuldTw6A
    AAAEDXntdxX5CHDszf8zwg73jwpDKa3EAzd6ElvmPPT3owrXhPPIqNSZHUwUfNOPRNXquH
    qbKzZrppQsMGSy6V1PDoAAAAFnlvdXJfZW1haWxAZXhhbXBsZS5jb20BAgMEBQYH
    -----END RSA PRIVATE KEY-----


Why are they a pair? They work like a key and a lock. Your private key (e.g. ``id_rsa``) corresponds to a key being privately stored on your local machine and accessed by limited number of people like yourself. In contrast, your public key (e.g. ``id_rsa.pub``) corresponds to a lock on public places such as `GitHub <https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account>`_ or `NIH HPC <https://hpc.nih.gov/docs/sshkeys.html>`_ where you use it for your authentication.


How do I create a pair of SSH keys?



Creating a new SSH key
----------------------


The command ``ssh-keygen`` is used to create an SSH key. Explore the command via ``ssh-keygen --help``.


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


The flag ``-t`` configures the type of SSH key. In theory, it is expected to create 5 different types (``[-t dsa | ecdsa | ed25519 | rsa | rsa1]``). However, it's possible to fail in some types depending on what your SSH slient supports.
Let's follow what is guided `here <https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#generating-a-new-ssh-key>`_. Assume you're still in ``~/.ssh``.


.. code-block:: bash

    ~/.ssh$ ssh-keygen -t ed25519 -C "your_email@example.com"


Running above command returns further options below:


.. code-block:: bash

    enerating public/private ed25519 key pair.
    Enter file in which to save the key (/home/user/.ssh/id_ed25519):   # <Press enter to skip>
    Enter passphrase (empty for no passphrase):                          # <Optional. Enter to skip>
    Enter same passphrase again:                                         # <Optional. Enter to skip>


You'll get the following messages as a proof of successful key creation.


.. code-block:: bash

    Your identification has been saved in /home/user/.ssh/id_ed25519.
    Your public key has been saved in /home/user/.ssh/id_ed25519.pub.
    The key fingerprint is:
    SHA256:EiZiWkseuxScabC3szzySZ1LOBFpBAu+chyK4KSOjYY your_email@example.com
    The key's randomart image is:
    +--[ED25519 256]--+
    |+..              |
    |o* +             |
    |+o^ . o          |
    |*%.X o .         |
    |*o@   . S        |
    |=* B . .         |
    |EoX +            |
    |.+ = .           |
    |  o .            |
    +----[SHA256]-----+


Check your new keys as shown below:


.. code-block:: bash

    ~/.ssh$ ls
    authorized_keys  id_ed25519  id_ed25519.pub  id_rsa  id_rsa.pub  known_hosts


You got your new private (``id_ed25519``) and public (``id_ed25519``) keys.


SSH authentication
------------------


My primary use of SSH key is to authenticate my accounts on GitHub and NIH HPC. Before creating a new SSH key, ensure to have match your email address matched. For example, if a key will be used for GitHub, your GitHub account and SSH key have to have the same email address. If a key will be used for NIH HPC, you have to set up the key with your NIH email address like ``ssh-keygen -t ed25519 -C "username@nih.gov"``.
