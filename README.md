# gpg-notes
Documenting how I check GPG signatures

# Step one, download files and detached signature

> curl -sLO https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.42.0.tar.sign
> curl -sLO https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.42.0.tar.xz

# Step two, search for auther's key, I used keyserver.ubuntu.com
https://keyserver.ubuntu.com/pks/lookup?search=gitster%40pobox.com&fingerprint=on&op=index

Why choose keyserver.ubuntu.com ?  I just kept on searching.  How did I find the authors email? 
Again a lot of fruitless searching until I tripped over a git source mailing list.

# Step three, copy the top entry (above all the subkeys) paste that into a file such as gitster@pobox.com.asc
In our example it is `pub rsa4096/96e07af25771955980dad10020d04e5a713660a7 2011-10-01T02:07:40Z`

# Step four, import the key
`gpg --import gitster@pobox.com.asc`
Alternately 
`key='https://keyserver.ubuntu.com/pks/lookup?op=get&search=0x96e07af25771955980dad10020d04e5a713660a7'`
`gpg --import <(curl -s "$key")`

# Step five, check the signature
[pedaaadmins@omaedcwww129 Centos7-RPM-buildserver]$ xzcat git-2.42.0.tar.xz | gpg --verify git-2.42.0.tar.sign -
gpg: Signature made Mon 21 Aug 2023 12:29:53 PM CDT
gpg:                using RSA key E1F036B1FEE7221FC778ECEFB0B5E88696AFE6CB
gpg: Good signature from "Junio C Hamano <gitster@pobox.com>" [unknown]
gpg:                 aka "Junio C Hamano <jch@google.com>" [unknown]
gpg:                 aka "Junio C Hamano <junio@pobox.com>" [unknown]
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: 96E0 7AF2 5771 9559 80DA  D100 20D0 4E5A 7136 60A7
     Subkey fingerprint: E1F0 36B1 FEE7 221F C778  ECEF B0B5 E886 96AF E6CB

