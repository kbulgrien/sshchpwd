
# sshchpwd.exp

Expect script to change login passwords using SSH

A few years ago I was supporting a very diverse environment with Solaris, AIX, and Linux servers; some with password logins, public/private key authentication, and several with SecurID passwords. All accounts were local, passwords expired every three months, and the accounts locked after three failed logins — so you can imagine the mess this created if you didn’t go around every server at least every three months. After I’d accumulated about half a dozen passwords, I wrote an Expect script to login and change my password and wrapped it with a bash script to try every old password I had. Since some servers needed a SecurID number to login, the bash script would pause on those and prompt me for the token before continuing. I’ve seen a few Expect scripts to change passwords, but none that can handle one-time tokens (like SecurID), expired passwords (changed before arriving at the prompt), and the variety of prompts to support Solaris, AIX, Linux, etc.

<pre><code>
# /usr/local/bin/sshchpwd.exp
#
# Expect script to change login passwords using SSH, with support for one-time
# tokens (like SecurID), expired passwords (changed before arriving at the
# prompt), on Solaris, AIX, Linux, etc.
#
# License: GPLv3
# License URI: http://www.gnu.org/licenses/gpl.txt
#
# Copyright 2012-2016 Jean-Sebastien Morisset (http://surniaulula.com/)
#   Modifications made to allow SCO Openserver 5.0.7 password changes:
#     Copyright 2020 Kevin R. Bulgrien (https://systemsdesignusa.com/)
#
# Example:
#
#       $ export OLD_PASSWORD="oldpwd"
#       $ export NEW_PASSWORD="newpwd"
#       $ ./sshchpwd {server} {port} {token} {passwd_command}
#
# The {server} parameter may incorporate a login username (i.e. user@server).
#
# The {port} and {token} command-line parameters are optional. If the server
# uses SecurID (or another one-time password), enter it on the command-line
# as the third parameter.
#
# The {passwd_command} parameter is optional.  It supports customizing the
# passwd command.  As this script handles only prompts produced by various
# versions of the passwd utility while actually changing a password, this
# parameter is primarily intended to tweak the command as needed to supply
# command-line arguments, or, for example, to prefix the command with sudo.
# In the case of sudo, however, as this script is not sudo-aware, sudo must
# permit the login user to run passwd without first retyping their password.
# (See the sudo "nopasswd" configuration option.)  If one or more optional
# parameters are unneeded, empty quotes ("") serve as a placeholder for
# unused optional parameters.
#
# Example:
#
#   $ export OLD_PASSWORD=""
#   $ export NEW_PASSWORD="newpwd"
#   $ ./sshchpwd login@hostname "" "login_password" "sudo passwd username"
#
# In the case of changing another user's password, OLD_PASSWORD, though not
# likely required, is defined to avoid an error (blank value is ok), and the
# ssh login user's password is supplied as {token} and not as an OLD_PASSWORD.
</code></pre>

