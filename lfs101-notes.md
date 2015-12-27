# LFS101

## Notes by Jeremiah Richter
---

*The link to the free LFS101 course is [here](https://courses.edx.org/courses/course-v1:LinuxFoundationX+LFS101x.2+1T2015/courseware/18780407cf8946c389bed38c4748418c/ "LFS101x")*

---
* ### Finding files
  * `locate` uses `updatedb` which needs root, is indexed
  * `file` is a recursive search, non-indexed, uses cwd  
      * `<no args>`: all files in cwd
      * `-name`: pattern in name, -iname is case-insensitive
      * `-type [d,l,f]`: filter by directory, link, file classification
      * `-exec [command] {} [';',\]` : run command on found files, must  
      terminate line with with either ';' or \\ to end command processing
      * `-ok`: same options and results as `-exec` but prompts before
        processing
      * `-ctime [days]`: find based on inode data changed (permissions,  
          ownership), use number of days, `+n` is greater than that range,  
          `-n` is less than, `n` is exactly that value
      * `-atime [days]`: uses last accessed/read time
      * `-mtime [days]`: uses modified/written time
      * `-cmin, -amin, -mmin`: are the same for minutes of time
      * `size [number{ⵁ,c,k,M,G}]`: size can be searched, a raw number
        being used for 512-byte blocks, c for bytes, k for kilobytes, M
        megabytes, G gigabytes, `n`, `+n` `-n` meaning as above
---
* ### Backing up
  * backup uses `cp` or `rsync`
      * `rsync` doesn't copy not changed files
      * `rsync` can remote copy using `[target]:[path]` where target
        can be `[user@]host`
      * `-r`: recursive copy
      * `-dry-run`: doesn't commit changes
  * `gzip`, `bzip2`, `xz`, `zip` are compression programs
      * `gzip -r`: for recursive zipping, .gz extension
      * `gunzip`: for uncompressing, same as `gzip -d`
      * `bunzip2`: uncompressing bzip2 files, .bz2 extension
      * `xz` removes original; files after archiving, use `-k` to keep the files  
      `-d` to decompress
      * `zip, unzip` can use `-r`
  * `tar` is tape archive, makes a 'tarball', can use compression
      * `tar xvf [file]`: extract verbosely a file named 'file'
      * `tar cvf [file]`: verbosely create a tarball named 'file'
      * `z`: add gzip compression
      * `j`: add bzip2 compression
      * `J`: add xz compression
  * `dd` is disk-to-disk copying
      * `if=`: input disk device file
      * `of=`: output file/device
      * `bs=`: block size
      * `count=`: how many blocks to copy
---
* ### Authentication
  * `sudo` is preferred to `su`
    * config in `/etc/sudoers` and in `/etc/sudoers.d/`
    * logs to `/var/log/secure`
    * use `visudo` to edit config
    * line entry is `who where = (as_whom) what`
    * add file to `/etc/sudoers.d/` with same name as user so as not  
      to edit the main file
    * commands run under `sudo` are logged to `/var/log/auth.log` in Debian  
      and family, `/var/log/messages` or `/var/log/secure` on others
---
* ### Networking
  * `/etc/hosts`: map of IP addresses to hostnames
  * `/etc/resolve.conf`: order of where to look for info
  * `host, dig`: look up IP's for hostnames
  * Debian family network scripts are in `/etc/network/interfaces` and one can  
    start the network interfaces with `/etc/init.d/networking start`
    * For systemd-based systems, routing and host info are in
      `/etc/sysconfig/network` and interface scripts are in `/etc/sysconfig/network-scripts/ifcfg-eth0`
    * you can start networks on those systems with `/etc/init.d/network start`, as well
  * `/sbin/ip addr show`: for IP addresses
  * `/sbin/ip route show`: for routing info
  * `ping`: check attached machine to see if can send/receive
    * `-c number` is number of pings, or press `Ctrl-C` to exit
  * `route` is to show network routes and change them
    * `-n`: to show current routes
    * `route add -net address`: add static route
    * `route del -net address`: delete static route
  * `traceroute` inspects which route to reach host is taken
    * `traceroute <domain>`: usage
  * `ethtool`: Queries network interfaces and can also set various  
    parameters such as the speed.
  * `netstat`	=> Displays all active connections and routing tables.  
    Useful for monitoring performance and troubleshooting.
  * `nmap`: Scans open ports on a network. Important for security analysis
  * `tcpdump`: Dumps network traffic for analysis.
  * `iptraf`: Monitors network traffic in text mode.
* browsers:
  * `lynx`: text-based, older
  * `links/elinks`: newer, with tables frames, but still text-based
  * `w3m`: newer text-based browser
* `wget`: is a command line utility that can capably handle the following  
  types of downloads:
  Large file downloads,
  Recursive downloads, where a web page refers to other web pages and all are downloaded at once
  Password-required downloads,
  Multiple file downloads
* `curl`: can get info on file to download, like source
  * `curl -o 'file' <url>`: to download <url> to file 'file'
* ftp:
  * `ftp, sftp, ncftp, yafc`: CLI FTP clients
    * `sftp` uses ssh, cannot connect anonymously
* `ssh`: command to have a remote shell securely
* `scp`: use SSH to copy files securely between two systems
  * `scp <localfile> <user@remote>:/path/to/copy`: usage
---
* ### Text Editing
  * `sed`: stream editor
    * `sed -e command <file>`: run commands on file on CLI
    * `sed -f scriptfile <file>`: run commands from scriptfile on file
    * `sed s/pattern/replace/ file`: replace first 'pattern' found per line
    * `sed s/pattern/replace/g file`: replace many times per line
    * `sed 1,3s/pattern/replace/g file`: limit replacements to range of lines 1-3
    * `sed -i s/pattern/replace/g file`: save replacements back to same file,
      dangerous
  * `awk` is used to extract and print specific contents of a file for reports
    * `awk 'command' var=value file`: run command directly on file
    * `awk -f scriptfile var=value file`: run commands from scriptfile on file
    * `awk '{ print $0 }' /etc/passwd`: prints entire password file
    * `awk -F: '{ print $1 }' /etc/passwd`: prints first field of lines delimited by ':'
    * `awk -F: '{ print $1 $6 }' /etc/passwd`: print first field and sixth field  
      in /etc/passwd separated by a space for every line
  * `sort` sorts lines of text
    * `sort -u`: filters out unique values after sorting; `uniq` filters but requires  
      sorted input, so `sort -u` saves a step
    * `sort -r`: sorts in reverse order
  * `uniq -c`: counts the number of duplicate entries
  * `paste` merges lines from two files into single lines separated by characters
    * `-d`: a list of delimiters instead of tabs to use, each one used consecutively
    * `-s`: append in a series without line breaks
  * `join`: like paste but checks for common fields before joining
  * `split`: splits a long file into smaller files with a prefix
    * `split file <prefix>`: usage
  * `grep` is a pattern-matching text search tool
    * `grep <pattern> filename`: basic usage
    * `grep -v <pattern> filename`: print lines that do **NOT** match pattern
    * `grep -C [number] <pattern> filename`: prints 'number' of lines around  
      and below the lines matching the patterns
  * `tr [options] set1 set2 < 'inputfile' > 'outputfile'` replaces one set of  
    *characters* for another set, one for one from inputfile and saves to outputfile
    * `-c` is a set complement, `-d` is to delete
  * `tee 'newfile'` reads output to the screen and saves the output as well to newfile
  * `wc` counts # of lines, words, characters in a file
    * `-l`: just line count
    * `-c`: byte count
    * `-w`: just word count
  * `cut` extracts columns based on delimiters
    * `cut -d" " -f3"`: cut out third column separated by spaces
  * `less, head, tail` displays parts of files
    * `-n`: in `head`/`tail`, limit to *n* lines
    * `-f`: with tail continues to monitor and re-output last few lines
  * `strings`: finds strings in binary files
---
* ### Printing
  * CUPS = **C** ommon **U** nix **P** rinting **S** ystem  
    * `/etc/cups/{cupsd.conf,printers.conf}`: config files for CUPS
      * `cupsd.conf`: system-wide and network configuration
      * `printers.conf`: specific settings for printers, don't hand-modify
    * `/var/spool/cups/` stores files for when they are sent to a printer for  
      printing, data files are prefixed with a `d`, and are removed after a print
      job, and `c`-files are control files
    * log files are in `/var/log/cups`
    * `/etc/cups/ppd/` where connected printers' have config files
    * `http://localhost:631` is address for cups web interface
---
* ### Shell Scripting
  * `#!/path/to/interpreter`: need an interpreter, can be perl/python/bash/tcsh...
  * `read VARIABLE_NAME` to read input
  * `echo $?` to see exit value of child process
  * one can declare functions:
        function_name () {
          command...
        }
    to repeat tasks; arguments are the auto-variables `$1`, `$2`, etc...
  * At times, you may need to substitute the result of a command as a portion of  
    another command. It can be done in two ways:
    * By enclosing the inner command with backticks (\`)
    * By enclosing the inner command in `$( )`, this allows nesting
    * `var=value`: setting of variables; `$var` to read; `export var` for child  
      processes to access
    * arguments to script:
      * `$0`: script name
      * `$1,$2,$3`: additional parameters, `$*` for all in an array
      * `$#`: number of arguments
  * In compact form, the syntax of an if statement is:
        if TEST-COMMANDS; then CONSEQUENT-COMMANDS; fi
    A more general definition is:
        if condition
        then
         statements
        else
         statements
        fi
  * `elif`: can be used for subsequent truth testing after first `if`
  * `[[]]`: double brackets should surround the test condition
  * can be used like:
        if [ -f /etc/passwd ] ; then
          ACTION
        fi
    with different test conditions(`man 1 test` for full list):
        Condition Meaning
        -e file	Check if the file exists.
        -d file	Check if the file is a directory.
        -f file	Check if the file is a regular file (i.e., not a symbolic link, device node, directory, etc.)
        -s file	Check if the file is of non-zero size.
        -g file	Check if the file has sgid set.
        -u file	Check if the file has suid set.
        -r file	Check if the file is readable.
        -w file	Check if the file is writable.
        -x file	Check if the file is executable.
  * strings can be tested with `==` operator
  * numbers can be tested with these operators:
        Operator  Meaning
        -eq	Equal to
        -ne	Not equal to
        -gt	Greater than
        -lt	Less than
        -ge	Greater than or equal to
        -le	Less than or equal to
  * Arithmetic expressions can be evaluated in the following three ways  
    (spaces are important!):
    Using the `expr` utility: `expr` is a standard but somewhat deprecated program.  
    The syntax is as follows:

    `expr 8 + 8;
    echo $(expr 8 + 8)`

    Using the `$((...))` syntax: This is the built-in shell format. The syntax is as  
    follows:

    `echo $((x+1))`
    Using the built-in shell command let. The syntax is as follows:

    `let x=( 1 + 2 ); echo $x`
    In modern shell scripts the use of `expr` is better replaced with `var=$((...))`
---
* ### Advanced Bash Scripting
* string operators:
      Operator	Meaning
      [[ string1 > string2 ]]	Compares the sorting order of string1 and string2.
      [[ string1 == string2 ]]	Compares the characters in string1 with the characters in string2.
      myLen1=${#string1}	Saves the length of string1 in the variable myLen1.
* parts of a string:
  At times, you may not need to compare or use an entire string. To extract the  
  first character of a string we can specify:

  `${string:0:1}`: Here 0 is the offset in the string (i.e., which character to  
  begin from) where the extraction needs to start and 1 is the number of
  characters to be extracted.

  To extract all characters in a string after a dot (.), use the following
   expression: `${string#*.}`
* `-z` tests a string for the empty string ''
* boolean expressions:
      &&  AND "both true"
      ||  OR  "either true"
      !   NOT "only if false"
* case structure:
      case expression in
         pattern1) execute commands;;
         pattern2) execute commands;;
         pattern3) execute commands;;
         pattern4) execute commands;;
         * )       execute some default commands or nothing ;;
      esac
* for loop:
      for variable-name in list
      do
          execute one iteration for each item in the
                  list until the list is finished
      done
* while loop:
      while condition is true
      do
          Commands for execution
          ----
      done
* until loop:
  The until loop repeats a set of statements as long as the control command is false. Thus it is essentially the opposite of the while loop. The syntax is:

      until condition is false
      do
          Commands for execution
          ----
      done
* script debugging:
  Before fixing an error (or bug), it is vital to know its source.

  In bash shell scripting, you can run a script in debug mode by doing  
  `bash –x ./script_file`. Debug mode helps identify the error because:

  It traces and prefixes each command with the + character.
  It displays each command before executing it.
  It can debug only selected parts of a script (if desired) with:
  `set -x `   # turns on debugging
  ...
  `set +x`    # turns off debugging
---
