<h1>Portron</h1>
<h4>(C) 2018 Rodolfo Lopez Pintor - Nebular Streams</h4>

Portron is a minimalistic, Super-Slim wrapper Linux distribution for your electron
application.

<h3>Transform any electron application into a bootable bulletproof kiosk system</h3>

A One-liner will transform your electron application into an installable ISO
with a super-slim linux system fine-tuned to launch your electron application in kiosk
mode, and provide all network and multimedia services. The script can be used standalone,
or ideally integrated into your build system to automatically generate the bootable
media with the rest of your artifacts.


<H3>Porteus meets Electron</h3>

Portron is based on  Porteus-Kiosk distributions, a famous distribution for Kiosk devices
that features a modern 64bit Linux kernel and relevant video, audio, network and multimedia drivers
for today's devices, as well as a rock-solid, bulletproof and slimmed down linux system.

We have further finetuned the system to play nice with electron. Features like multi-monitors,
touch screens, HDMI audio, hotplugging, storage of app cache settings in memory or storage partition ...
have been taken care of.

The bootable system includes graphical wizards to configure the system from a fresh run:
network, screen resolution, sound, microphone, keyboard layout ... as well as an option
to install the system to the Hard Disk, SD-Card or the same USB Key (persistent installation)

But all this will be done in a whisper, then your application takes over.

<h3>Your application runs the show</h3>

Portron star feature is invisibility. Besides the UI of the Wizards that are launched
to change settings, configure network or install to disk, there's nothing else but your
application full-screen, without any borders, distractions or possiblity to interact with the
underlying OS.

A typical screen flow of your lectron app launched on a computer from power-on would be:

    * The splash screen YOU provide
    * Your application, fullscreen.

    nothing else.

<h2>How to use PorTron?</h2>

<h4>1 - Install Portron builder</h4>

<li>clone PorTron-builder in eg. /opt/portron and run the installation script for
    linux or OSX. It should install the dependencies.</li>

    cd /directory_of_your_choice
    git clone https://github.com/nebular/Portron.git .

    cd Portron
    ./install.sh

The above command will create symbolic links on /usr/local/bin for the Portron tools

    electron-tgz2iso


<h4>2 - build your electron application</h4>
<li>use <code>electron-builder</code> to build your electron application. Select "linux" and
    "tar.gz" as application output type.
<h5>
NOTE: ONLY applications generated by eletron-builder will work. Also, please make sure your application filename is
    just as provided by electronbuilder (do not rename it), like: mycoolapplication-1.0.3.tar.gz or
    mycoolapplication-1.0.3-xxx.tar.gz, as the scripts depend on that format and version info to strip
    the application filename, executable, etc.
</h5>

Likewise, that name should also be the name of your electron package. If it sounds confusing remember it boils down to a simple rule: Use the tar.gz as-is. Do not rename or change anything.


<h4>3 - build your portron distribution</h4>
<li>invoke the builder:</li>

    electron-tgz2iso input-1.0.0-cool.tar.gz splashscreen.jpg input-1.0.0-cool.iso

            - first parameter: Your app in .tar.gz format as provided by eletron-builder
            - second parameter: A Splash screen to be displayed while your application is loading
            - third parameter: Name of the resulting ISO file


... and that's it. You will get an ISO file that will be like 70Mb bigger than your original application. It's all self-contained, run it on a Virtual Machine to test it, or ...

<h4>4 - Write the ISO into a USB Stick or CDROM</h4>
<li> optional - Create a bootable USB or CD. For example, to create a bootable USB:</li>

    dd bs=1m if=yourapplication.iso of=/dev/xxx
    (set XX to your unix USB device, eg. /dev/rdisk2 or /dev/sdd, etc. Be careful, you know)


<h2>PorTron dependencies and requirements</h2>

The scripts are still a little rough, and if this dependencies are not met they can fail unpredictably.

    mksquashfs (debian package squashfs-tools / brew package squashfs)
    mkisofs (debian package cdrtools / brew cdrtools))
    isohybrid (included, compiles on installation)

<code>Isohybrid</code> source code is provided, as it is a very small program, you can compile it yourself with gcc if not able to install it from your repository.

Considerations
--------------

Package structure:

    ./portron/

        ./bin           portron command-line utilities. Here are the scripts
                        you want to use, specifically electron-tgz2iso. You
                        can also generate the electron XZM module only with
                        electron-tgz2xzm.

        ./src           Portron SDK, not needed for packaging applications. Allows to cook
                        "distro images" that are used by the tool to package your application.
                        You can have several distro images finetuned for different purposes..

        ./lib           build support files

            ./portron.default/
                    Portron Distribution used by the build tool to create the ISO.
                    You can create additional distributions using the Portron Development
                    Tools in src/

            ./xzm.fs
                    a stub for your electron application with linux libraries and
                    startup scripts. You can place scripts here to be executed at
                    certain stages of the system initialization: Add ports to firewall, etc...,
                    or add any required library

            ./src.initrd
                    source initrd. contains Portron low level startup.

<h2>Troubleshooting</h2>
---------------

    This tool is tested mostly on Mac OS-X and Debian systems, but it should work in any Linux meeting the
    dependencies.

    Mac OSX:
        brew install squashfs-tools cdrtools gcc
        compile isohybrid in bin/isohybrid (make.sh provided, needs a basic gcc)

    Linux:
        apt-get install cdrtools squashfs-tools gcc


License
-------

GNU GPL 2.0
Please see attached <a href="GNU_GPL">file</a>.


TO-DO
------

- Improve Installation wizards

