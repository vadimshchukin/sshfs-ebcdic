sshfs-ebcdic
=========

sshfs filesystem uses SFTP protocol as an I/O for virtual files. SFTP itself uses SSH protocol as a transport. SSH works in binary mode only - it doesn't perform any character conversion (unlike FTP for example, which has text mode as well as binary).

Very few applications support EBCDIC encodings, that's why it's sometimes desirable to allow character conversion between ASCII and EBCDIC for mainframe servers.

This patch should be applied to [2.5 version of sshfs] using:
```sh
cd sshfs-fuse-2.5 && patch -p1 <sshfs.patch
```

[2.5 version of sshfs]:http://sourceforge.net/projects/fuse/files/sshfs-fuse/2.5/sshfs-fuse-2.5.tar.gz/download

"-t" command-line option should be used to specify a regular expression pattern for files that should be translated between ASCII and EBCDIC.
The following example defines conversion for the list of registered extensions as well as special cases - ".ssh" root folder, "makefile" file name:
```sh
exts='txt|c|h|cpp|hpp|java|sh|bash_history|bashrc|profile|sh_history'
convpat="^(\/\.ssh)|(.*?(\.($exts))|makefile)$"
sshfs -t"$convpat" ...
```