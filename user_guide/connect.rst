.. _sec-ref-connect:

Connecting to the System
========================

This section covers the basic methods for connecting to the NEXTGenIO prototype. 
The first part describes how to set up the initial connection.

The second part of this section provides more information on using SSH: using SSH
on different platforms.

Quick Setup
~~~~~~~~~~~

Acquiring Permissions
---------------------

In order to access the NEXTGenIO machine, you will have to acquire access to both
the gateway machine, called hydra-vpn, and to the NGIO machine itself. Access to both
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
drop down menu.

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
