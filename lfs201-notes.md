# LFS201
## Notes by Jeremiah Richter
***

1. ### Course Introduction ###
  1. #### Introduction
    * nothing  to note  

  2. #### Relation to LFS101x.2
    * *Use LFS101x.2 as reference*:
        * link is [here](https://courses.edx.org/courses/course-v1:LinuxFoundationX+LFS101x.2+1T2015/courseware/18780407cf8946c389bed38c4748418c/ "LFS101x")
        * `locate` uses `updatedb` which needs root, is indexed
        * `file` is a recursive search, non-indexed, uses cwd  
            * `<no args>` => all files in cwd
            * `-name` => pattern in name, -iname is case-insensitive
            * `-type [d,l,f]` => filter by directory, link, file classification
            * `-exec [command] {} [';',\]`  => run command on found files, must  
            terminate line with with either ';' or \\ to end command processing
            * `-ok` => same options and results as `-exec` but prompts before
              processing
            * `-ctime [days]` => find based on inode data changed (permissions,  
                ownership), use number of days, `+n` is greater than that range,  
                `-n` is less than, `n` is exactly that value
            * `-atime [days]` => uses last accessed/read time
            * `-mtime [days]` => uses modified/written time
            * `-cmin, -amin, -mmin` => are the same for minutes of time
            * `size [number{ⵁ,c,k,M,G}]` => size can be searched, a raw number
              being used for 512-byte blocks, c for bytes, k for kilobytes, M
              megabytes, G gigabytes, `n`, `+n` `-n` meaning as above
        * backup uses `cp` or `rsync`
            * `rsync` doesn't copy not changed files
            * `rsync` can remote copy using `[target]:[path]` where target
              can be `[user@]host`
            * `-r` => recursive copy
            * `-dry-run` => doesn't commit changes
        * `gzip`, `bzip2`, `xz`, `zip` are compression programs
            * `gzip -r` => for recursive zipping, .gz extension
            * `gunzip` => for uncompressing, same as `gzip -d`
            * `bunzip2` => uncompressing bzip2 files, .bz2 extension
            * `xz` removes original; files after archiving, use `-k` to keep the files  
            `-d` to decompress
            * `zip, unzip` can use `-r`
        * `tar` is tape archive, makes a 'tarball', can use compression
            * `tar xvf [file]` => extract verbosely a file named 'file'
            * `tar cvf [file]` => verbosely create a tarball named 'file'
            * `z` => add gzip compression
            * `j` => add bzip2 compression
            * `J` => add xz compression
        * `dd` is disk-to-disk copying
            * `if=` => input disk device file
            * `of=` => output file/device
            * `bs=` => block size
            * `count=` => how many blocks to copy
        * `sudo` is preferred to `su`
          * config in `/etc/sudoers` and in `/etc/sudoers.d/`
          * logs to `/var/log/secure`
          * use `visudo` to edit config
          * line entry is `who where = (as_whom) what`
          * add file to `/etc/sudoers.d/` with same name as user so as not  
            to edit the main file
          * commands run under `sudo` are logged to `/var/log/auth.log` in Debian  
            and family, `/var/log/messages` or `/var/log/secure` on others
        
