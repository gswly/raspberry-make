
# raspberry-make

raspberry-make is a command-line tool (Makefile) that can be used to build deterministic and ready-to-use OS images for the Raspberry Pi, defined by a set of configuration files. It is intended for creating images with pre-installed software and configuration, that can be deployed to headless computers, without the need for setup procedures or manual installations.

A provisioning-based approach, like the one made possible by this tool, leads to a decrease in deployment time, to a decrease in human-related errors, and allows to collect all the configuration files related to a specific machine in a single repository.

Features:
* all build stages are cached and therefore reprocessed only if needed
* compatible with multiple Linux distros, as it depends only on docker
* building happens in an isolated docker container
* building does not require root privileges, nor any additional capability

raspberry-make is implemented in the form of a Makefile, and makes use of Ansible, a common tool for configuring remote machines, that follows rules defined in YAML-formatted files.

## Installation and usage

1. Install dependencies:
   * docker (>= 18.09)
   * make
   * curl

2. Download software and sample configuration:
   ```
   curl -L https://github.com/gswly/raspberry-make/tarball/master | tar zxvf - --strip-components=1
   rm README.md LICENSE
   ```

3. Edit `config` to suit your needs, edit `00base/playbook.yml`, `01apt/playbook.yml`, `02ssh/playbook.yml` to suit your needs. These files are Ansible playbooks, whose format is documented [here](https://docs.ansible.com/ansible/latest/user_guide/playbooks.html). It is possible to create as many folders as needed, each with a file named `playbook.yml`. Folders are processed in alphabetical order, and rules are processed sequentially.

5. Launch:
   ```
   make -f rpimake.mk
   ```
   the resulting image will be available in `build/output.img`.

## Upgrade

Upgrade raspberry-make to the latest version:
```
make -f rpimake.mk self-update
```

## Limitations

Some files cannot be changed by playbooks since they are used by Docker; these are:
* /etc/hosts
* /etc/hostname
* /etc/mtab
* /etc/resolv.conf

File `config` contains options to change these files safely.

## Links

main image builder
* https://github.com/RPi-Distro/pi-gen

other Raspbian-based provisioning systems
* https://github.com/davidferguson/pibakery
* https://github.com/Scout24/rpi-image-creator

other non Raspbian-based provisioning systems
* https://buildroot.uclibc.org/
* https://www.yoctoproject.org/

inspired by
* https://github.com/plerup/makeEspArduino
