## Initramfs-tools based systems(Ubuntu and derivatives)

For easiest installation [download and install the precompiled deb from releases.](https://github.com/shimunn/fido2luks/releases). However it is possible to build from source via the instructions on the main readme.

```
sudo -s

# Insert FIDO key.
fido2luks credential
# Tap FIDO key
# Copy returned string <CREDENTIAL>

nano /etc/fido2luks.conf
# Create a salt, e.g.:
# FIDO2LUKS_SALT=string:<random 512 bytes base64-encoded>
# Set it to use the token
# FIDO2LUKS_USE_TOKEN=1
# Use password fallback
# FIDO2LUKS_PASSWORD_FALLBACK=1

set -a
. /etc/fido2luks.conf
fido2luks add-key -t /dev/<LUKS PARTITION> <CREDENTIAL> --salt $FIDO2LUKS_SALT
# Current password: <Any current LUKS password>
# Password: <Password used as FIDO challange>
# Tap FIDO key

nano /etc/crypttab
# Append to end ",discard,initramfs,keyscript=fido2luks"
# E.g. sda6_crypt UUID=XXXXXXXXXX none luks,discard,initramfs,keyscript=fido2luks

update-initramfs -u


```

[Recording showing part of the setup](https://shimun.net/fido2luks/setup.svg)

