.. _sec-ref-connect:

Connecting to the System
========================

This section covers the basic methods for connecting to the NEXTGenIO prototype. 
The first part describes how to set up the initial connection, and gives a 
summary of how to ease the connecting process (after the initial login) using 
SSH keys. This section is designed for people familiar with using SSH and SSH
config files.

The second part of this section provides more information on using SSH: using SSH
on different platforms, managing keys, and the SSH agent.

Quick Setup
~~~~~~~~~~~

Acquiring Permissions
---------------------

In order to access the NEXTGenIO machine, you will have to acquire access to both
the gateway machine, called hydra-vpn, and the machine itself. Access to both
systems is granted via the `SAFE system <https://safe.epcc.ed.ac.uk/safadmin/>`_ 
(Note that this is not the same SAFE as the one used for access to ARCHER). 
If you do not have an account with the SAFE system yet, follow the *create 
account* link on the home screen.

Once you are registered with SAFE, login and proceed to the EPCC SAFE main menu. 
Select *Login accounts* from the top menu bar and select *request login account*. 
For the project choose 'nx01 - NextGenIO support' from the drop down menu and
click *next*. On the following page select 'hydra-vpn' and click *next*. Finally,
select a username and click *request*.

These steps are now repeated to request access to NextgenIO itself. From the 
EPCC SAFE main menu select *request login account*, set the project to 'nx01 - 
NextGenIO support', and now select 'NextGenIO' before clicking *next* and
again choosing a username.

You can follow the progress of the request under the *login accounts* tab in
the EPCC SAFE main menu, where the requested accounts should now appear in the
dropdown menu.

First Login
-----------

Once the two accounts have been approved and created, the temporary passwords can
be found on EPCC SAFE. These passwords will be altered on the first login to the
systems. To connect to hydra-vpn open a terminal and enter:

::

    ssh -X [username]@hydra-vpn.epcc.ed.ac.uk

Use the EPCC SAFE password to connect, after which you will be asked to supply 
(and confirm) a permanent password, of your choosing. The option '-X' following 
the ssh command allows for X11 forwarding over the connection.

After connecting to hydra-vpn, connect to NextgenIO by entering:

::

    ssh -X [username]@nextgenio-login1

Upon connecting, use the SAFE password and replace this with a password of choice.

Connecting using SSH keys
-------------------------

After adding your public key to the authorised keys on hydra-vpn (see :ref:`Copying
SSH Public Key <sec-ssh-pub-remote>`), add the following lines to your .ssh/config
file. Replace the name of the identifile with the name of your private key (e.g. if
made with a different encryption):

::

    Host hydra-vpn
       HostName hydra-vpn.epcc.ed.ac.uk
       User [username]
       ForwardX11 yes
       ForwardAgent yes
       IdentityFile ~/.ssh/id_rsa

    Host nextgenio
       ProxyCommand ssh -W %h:%p hydra-vpn
       HostName nextgenio-login1
       ForwardAgent yes
       ForwardX11 yes
       ForwardX11Trusted yes
       User [username]

Note that it is not necessary to copy your public key to NextgenIO. Following
this setup, logging in to the NextgenIO system can be done by entering

.. code:: bash

   ssh nextgenio


Using SSH Clients
~~~~~~~~~~~~~~~~~

Interaction with the system is done remotely, over an encrypted
communication channel, Secure Shell version 2 (SSH-2). This allows
command-line access to one of the login nodes of a Cirrus, from which
you can run commands or use a command-line text editor to edit files.
SSH can also be used to run graphical programs such as GUI text editors
and debuggers when used in conjunction with an X client.

Logging in from Linux and Macs
------------------------------

Linux distributions and OS X each come installed with a terminal
application that can be use for SSH access to the login nodes. Linux
users will have different terminals depending on their distribution and
window manager (e.g. GNOME Terminal in GNOME, Konsole in KDE). Consult
your Linux distribution's documentation for details on how to load a
terminal.

OS X users can use the Terminal application, located in the Utilities
folder within the Applications folder.

You can use the following command from the terminal window to login into
hydra-vpn:

::

    ssh [username]@hydra-vpn.epcc.ed.ac.uk

To allow remote programs, especially graphical applications to control
your local display, such as being able to open up a new GUI window (such
as for a debugger), use:

::

    ssh -X [username]@hydra-vpn.epcc.ed.ac.uk 

Some sites recommend using the ``-Y`` flag. While this can fix some
compatibility issues, the ``-X`` flag is more secure.

Current OS X systems do not have an X window system. Users should
install the XQuartz package to allow for SSH with X11 forwarding on OS X
systems:

* `XQuartz website <http://www.xquartz.org/>`__

Logging in from Windows using MobaXterm
---------------------------------------

A typical Windows installation will not include a terminal client,
though there are various clients available. We recommend all our Windows
users to download and install MobaXterm to access Cirrus. It is very
easy to use and includes an integrated X server with SSH client to run
any graphical applications on Cirrus.

You can download MobaXterm Home Edition (Installer Edition) from the
following link:

* `Install MobaXterm <http://mobaxterm.mobatek.net/download-home-edition.html>`__

Double-click the downloaded Microsoft Installer file (.msi), and the
Windows wizard will automatically guides you through the installation
process. Note, you might need to have administrator rights to install on
some Windows OS. Also make sure to check whether Windows Firewall hasn't
blocked any features of this program after installation.

Start MobaXterm using, for example, the icon added to the Start menu
during the installation process.

If you would like to run any small remote GUI applications, then make
sure to use -X option along with the ssh command (see above) to enable
X11 forwarding, which allows you to run graphical clients on your local
X server.


Making access more convenient using an SSH Agent
------------------------------------------------

Using a SSH Agent makes accessing the resources more convenient as you
only have to enter your passphrase once per day to access any remote
resource - this can include accessing resources via a chain of SSH
sessions.

This approach combines the security of having a passphrase to access
remote resources with the convenience of having password-less access.
Having this sort of access set up makes it extremely convenient to use
client applications to access remote resources, for example:

-  the `Tramp <http://www.gnu.org/software/tramp/>`__ Emacs plugin that
   allows you to access and edit files on a remote host as if they are
   local files;
-  the `Parallel Tools Platform <http://www.eclipse.org/ptp/>`__ for the
   Eclipse IDE that allows you to edit your source code on a local
   Eclipse installation and compile and test on a remote host;

**Note:** this description applies if your local machine is Linux or macOS.
The procedure can also be used on Windows using the PuTTY SSH
terminal with the PuTTYgen key pair generation tool and the Pageant SSH
Agent. See the `PuTTY
documentation <http://the.earth.li/~sgtatham/putty/0.62/htmldoc/>`__ for
more information on how to use these tools.

**Note:** not all remote hosts allow connections using a SSH key pair.
If you find this method does not work it is worth checking with the
remote site that such connections are allowed.

Setup a SSH key pair protected by a passphrase
----------------------------------------------

Using a terminal (the command line), set up a key pair that contains
your e-mail address and enter a passphrase you will use to unlock the
key. This example uses RSA encryption:

::

    ssh-keygen -t rsa -C "your@email.com"
    ...
    -bash-4.1$ ssh-keygen -t rsa -C "your@email.com"
    Generating public/private rsa key pair.
    Enter file in which to save the key (/Home/user/.ssh/id_rsa): [Enter]
    Enter passphrase (empty for no passphrase): [Passphrase]
    Enter same passphrase again: [Passphrase]
    Your identification has been saved in /Home/user/.ssh/id_rsa.
    Your public key has been saved in /Home/user/.ssh/id_rsa.pub.
    The key fingerprint is:
    03:d4:c4:6d:58:0a:e2:4a:f8:73:9a:e8:e3:07:16:c8 your@email.com
    The key's randomart image is:
    +--[ RSA 2048]----+
    |    . ...+o++++. |
    | . . . =o..      |
    |+ . . .......o o |
    |oE .   .         |
    |o =     .   S    |
    |.    +.+     .   |
    |.  oo            |
    |.  .             |
    | ..              |
    +-----------------+

(remember to replace "your@email.com" with your e-mail address).

.. _sec-ssh-pub-remote:

Copy the public part of the key to the remote host
--------------------------------------------------

Using you normal login password, add the public part of your key pair to
the "authorized\_keys" file on the remote host you wish to connect to
using the SSH Agent. This can be achieved by appending the contents of
the public part of the key to the remote file:

::

    -bash-4.1$ ssh-copy-id -i .ssh/id_rsa [username]@hydra-vpn.epcc.ed.ac.uk

(remember to replace [username] with your username).

Now you can test that your key pair is working correctly by attempting
to connect to the remote host and run a command. You should be asked
for your key pair *passphase* (which you entered when you created the
key pair) rather than your remote machine *password*.

::

    -bash-4.1$ ssh [username]@hydra-vpn.epcc.ed.ac.uk 'date'
    Enter passphrase for key '/Home/user/.ssh/id_rsa': [Passphrase]
    Wed May  8 10:36:47 BST 2013

(remember to replace [username] with your username). The 'date' at the end of
the command ensures that the remote system provides the current time, after
which you are logged out again.

Enabling the SSH Agent
----------------------

So far we have just replaced the need to enter a password to access a
remote host with the need to enter a key pair passphrase. The next step
is to enable an SSH Agent on your local system so that you only have to
enter the passphrase once per day and after that you will be able to
access the remote system without entering the passphrase.

Most modern Linux distributions (and macOS) should have ssh-agent
running by default. If your system does not then you should find the
instructions for enabling it in your distribution using Google.

To add the private part of your key pair to the SSH Agent, use the
'ssh-add' command (on your local machine), you will need to enter your
passphrase one more time:

::

    -bash-4.1$ ssh-add ~/.ssh/id_rsa
    Enter passphrase for Home/user.ssh/id_rsa: [Passphrase]
    Identity added: Home/user.ssh/id_rsa (Home/user.ssh/id_rsa)

Now you can test that you can access the remote host without needing to
enter your passphrase:

::

    -bash-4.1$ ssh [username]@hydra-vpn.epcc.ed.ac.uk 'date'
    Warning: Permanently added the RSA host key for IP address '192.62.216.27' to the list of known hosts.
    Wed May  8 10:42:55 BST 2013

(remember to replace [username] with your username).

Adding access to other remote machines
--------------------------------------

If you have more than one remote host that you access regularly, you can
simply add the public part of your key pair to the 'authorized\_keys'
file on any hosts you wish to access by repeating step 2 above.

SSH Agent forwarding
--------------------

Now that you have enabled an SSH Agent to access remote resources you
can perform an additional configuration step that will allow you to
access all hosts that have your public key part uploaded from any host
you connect to with the SSH Agent without the need to install the
private part of the key pair anywhere except your local machine.

This increases the security of the key pair as the private part is only
stored in one place (your local machine) and makes access more
convenient (as you only need to enter your passphrase once on your local
machine to enable access between all machines that have the public part
of the key pair).

Forwarding is controlled by a configuration file located on your local
machine at ".ssh/config". Each remote site (or group of sites) can have
an entry in this file which may look something like:

::

    Host hydra-vpn
      HostName hydra-vpn.epcc.ed.ac.uk
      User [username]
      ForwardAgent yes

(remember to replace [username] with your username).

The "Host cirrus" line defines a short name for the entry. In this case,
instead of typing "ssh login.cirrus.ac.uk" to access the Cirrus login
nodes, you could use "ssh cirrus" instead. The remaining lines define
the options for the "cirrus" host.

-  ``Hostname hydra-vpn.epcc.ed.ac.uk`` - defines the full address of the
   host
-  ``User [username]`` - defines the username to use by default for this
   host (replace [username] with your own username on the remote host)
-  ``ForwardAgent yes`` - tells SSH to forward the local SSH Agent to
   the remote host, this is the option that allows you to store the
   private part of your key on your local machine only and export the
   access to remote sites

Now you can use SSH to access hydra-vpn without needing to enter my
username or the full hostname every time:

::

    -bash-4.1$ ssh hydra-vpn
    Tue Dec 20 16:48:32 GMT 2016

The second entry in the ".ssh/config" file suggested in the section `Connecting
using SSH keys`_ automatically connects to NextgenIO via hydra-vpn. It uses
the entry for hydra-vpn to make the first connection, and then continues
straight to NextgenIO.

You can set up as many of these entries as you need in your local
configuration file. Other options are available. See the `ssh_config
man page <http://linux.die.net/man/5/ssh_config>`__ (or ``man
ssh_config`` on any machine with SSH installed) for a description of the
SSH configuration file.

