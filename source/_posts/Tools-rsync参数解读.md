---
title: Rsync命令参数详解
index_img: /img_index/index/20210412-001.jpg
categories:
  - 工具集
tags:
  - Rsync
abbrlink: 22710
date: 2021-04-12 20:42:53
updated: 2021-04-20 20:42:53
desc: Rsync命令的参数简介
keywords: Rsync, rsync, 命令, 参数
---
{% note primary %}
记录一下rsync命令的参数。
{% endnote %}


<!--more-->
<hr />

#### 常用参数

| ID  |         NAME         | ENGLISH                                      | CHINESE                                                            |
|:---:|:--------------------:|:-------------------------------------------- |:------------------------------------------------------------------ |
|  1  |    --verbose, -v     | increase verbosity                           | 详细模式输出                                                       |
|  2  |     --quiet, -q      | suppress non-error messages                  | 精简输出模式                                                       |
|  3  |      --progress      | show progress during transfer                | 在传输时现实传输过程                                               |
|  4  |    --archive, -a     | archive mode is -rlptgoD (no -A,-X,-U,-N,-H) | 归档模式，表示以递归方式传输文件，并保持所有文件属性，等于-rlptgoD |
|  5  |    -compress, -z     | compress file data during the transfer       | 对备份的文件在传输时进行压缩处理                                   |
|  6  |       --delete       | delete extraneous files from dest dirs       | 删除那些DST中SRC没有的文件                                         |
|  7  |    --config=FILE     | specify alternate rsyncd.conf file           | 指定其他的配置文件，不使用默认的rsyncd.conf文件                    |
|  8  | --password-file=FILE | read daemon-access password from FILE        | 从FILE中得到密码                                                   |
|  9  |     --port=PORT      | listen on alternate port number              | 指定其他的rsync服务端口                                            |
| 10  |      --existing      | skip creating new files on receiver          | 仅仅更新那些已经存在于DST的文件，而不备份那些新创建的文件          |
| 11  |      --partial       | keep partially transferred files             | 保留那些因故没有完全传输的文件，以是加快随后的再次传输             |
| 12  |       --daemon       | run as an rsync daemon                       | 以daemon模型运行                                                   |
| 13  |  --exclude=PATTERN   | exclude files matching PATTERN               | 指定排除不需要传输的文件模式                                       |
| 14  | --exclude-from=FILE  | read exclude patterns from FILE              | 排除FILE中指定模式的文件                                           |

#### 全部参数

官网参数说明：https://download.samba.org/pub/rsync/rsync.1

```
--debug=FLAGS            fine-grained debug verbosity
--stderr=e|a|c           change stderr output mode (default: errors)
--quiet, -q              suppress non-error messages
--no-motd                suppress daemon-mode MOTD
--checksum, -c           skip based on checksum, not mod-time & size
--archive, -a            archive mode is -rlptgoD (no -A,-X,-U,-N,-H)
--no-OPTION              turn off an implied OPTION (e.g. --no-D)
--recursive, -r          recurse into directories
--relative, -R           use relative path names
--no-implied-dirs        don't send implied dirs with --relative
--backup, -b             make backups (see --suffix & --backup-dir)
--backup-dir=DIR         make backups into hierarchy based in DIR
--suffix=SUFFIX          backup suffix (default ~ w/o --backup-dir)
--update, -u             skip files that are newer on the receiver
--inplace                update destination files in-place
--append                 append data onto shorter files
--append-verify          --append w/old data in file checksum
--dirs, -d               transfer directories without recursing
--mkpath                 create the destination's path component
--links, -l              copy symlinks as symlinks
--copy-links, -L         transform symlink into referent file/dir
--copy-unsafe-links      only "unsafe" symlinks are transformed
--safe-links             ignore symlinks that point outside the tree
--munge-links            munge symlinks to make them safe & unusable
--copy-dirlinks, -k      transform symlink to dir into referent dir
--keep-dirlinks, -K      treat symlinked dir on receiver as dir
--hard-links, -H         preserve hard links
--perms, -p              preserve permissions
--executability, -E      preserve executability
--chmod=CHMOD            affect file and/or directory permissions
--acls, -A               preserve ACLs (implies --perms)
--xattrs, -X             preserve extended attributes
--owner, -o              preserve owner (super-user only)
--group, -g              preserve group
--devices                preserve device files (super-user only)
--specials               preserve special files
-D                       same as --devices --specials
--times, -t              preserve modification times
--atimes, -U             preserve access (use) times
--open-noatime           avoid changing the atime on opened files
--crtimes, -N            preserve create times (newness)
--omit-dir-times, -O     omit directories from --times
--omit-link-times, -J    omit symlinks from --times
--super                  receiver attempts super-user activities
--fake-super             store/recover privileged attrs using xattrs
--sparse, -S             turn sequences of nulls into sparse blocks
--preallocate            allocate dest files before writing them
--write-devices          write to devices as files (implies --inplace)
--dry-run, -n            perform a trial run with no changes made
--whole-file, -W         copy files whole (w/o delta-xfer algorithm)
--checksum-choice=STR    choose the checksum algorithm (aka --cc)
--one-file-system, -x    don't cross filesystem boundaries
--block-size=SIZE, -B    force a fixed checksum block-size
--rsh=COMMAND, -e        specify the remote shell to use
--rsync-path=PROGRAM     specify the rsync to run on remote machine
--existing               skip creating new files on receiver
--ignore-existing        skip updating files that exist on receiver
--remove-source-files    sender removes synchronized files (non-dir)
--del                    an alias for --delete-during
--delete                 delete extraneous files from dest dirs
--delete-before          receiver deletes before xfer, not during
--delete-during          receiver deletes during the transfer
--delete-delay           find deletions during, delete after
--delete-after           receiver deletes after transfer, not during
--delete-excluded        also delete excluded files from dest dirs
--ignore-missing-args    ignore missing source args without error
--delete-missing-args    delete missing source args from destination
--ignore-errors          delete even if there are I/O errors
--force                  force deletion of dirs even if not empty
--max-delete=NUM         don't delete more than NUM files
--max-size=SIZE          don't transfer any file larger than SIZE
--min-size=SIZE          don't transfer any file smaller than SIZE
--max-alloc=SIZE         change a limit relating to memory alloc
--partial                keep partially transferred files
--partial-dir=DIR        put a partially transferred file into DIR
--delay-updates          put all updated files into place at end
--prune-empty-dirs, -m   prune empty directory chains from file-list
--numeric-ids            don't map uid/gid values by user/group name
--usermap=STRING         custom username mapping
--groupmap=STRING        custom groupname mapping
--chown=USER:GROUP       simple username/groupname mapping
--timeout=SECONDS        set I/O timeout in seconds
--contimeout=SECONDS     set daemon connection timeout in seconds
--ignore-times, -I       don't skip files that match size and time
--size-only              skip files that match in size
--modify-window=NUM, -@  set the accuracy for mod-time comparisons
--temp-dir=DIR, -T       create temporary files in directory DIR
--fuzzy, -y              find similar file for basis if no dest file
--compare-dest=DIR       also compare destination files relative to DIR
--copy-dest=DIR          ... and include copies of unchanged files
--link-dest=DIR          hardlink to files in DIR when unchanged
--compress, -z           compress file data during the transfer
--compress-choice=STR    choose the compression algorithm (aka --zc)
--compress-level=NUM     explicitly set compression level (aka --zl)
--skip-compress=LIST     skip compressing files with suffix in LIST
--cvs-exclude, -C        auto-ignore files in the same way CVS does
--filter=RULE, -f        add a file-filtering RULE
-F                       same as --filter='dir-merge /.rsync-filter'
                         repeated: --filter='- .rsync-filter'
--exclude=PATTERN        exclude files matching PATTERN
--exclude-from=FILE      read exclude patterns from FILE
--include=PATTERN        don't exclude files matching PATTERN
--include-from=FILE      read include patterns from FILE
--files-from=FILE        read list of source-file names from FILE
--from0, -0              all *-from/filter files are delimited by 0s
--protect-args, -s       no space-splitting; wildcard chars only
--copy-as=USER[:GROUP]   specify user & optional group for the copy
--address=ADDRESS        bind address for outgoing socket to daemon
--port=PORT              specify double-colon alternate port number
--sockopts=OPTIONS       specify custom TCP options
--blocking-io            use blocking I/O for the remote shell
--outbuf=N|L|B           set out buffering to None, Line, or Block
--stats                  give some file-transfer stats
--8-bit-output, -8       leave high-bit chars unescaped in output
--human-readable, -h     output numbers in a human-readable format
--progress               show progress during transfer
-P                       same as --partial --progress
--itemize-changes, -i    output a change-summary for all updates
--remote-option=OPT, -M  send OPTION to the remote side only
--out-format=FORMAT      output updates using the specified FORMAT
--log-file=FILE          log what we're doing to the specified FILE
--log-file-format=FMT    log updates using the specified FMT
--password-file=FILE     read daemon-access password from FILE
--early-input=FILE       use FILE for daemon's early exec input
--list-only              list the files instead of copying them
--bwlimit=RATE           limit socket I/O bandwidth
--stop-after=MINS        Stop rsync after MINS minutes have elapsed
--stop-at=y-m-dTh:m      Stop rsync at the specified point in time
--write-batch=FILE       write a batched update to FILE
--only-write-batch=FILE  like --write-batch but w/o updating dest
--read-batch=FILE        read a batched update from FILE
--protocol=NUM           force an older protocol version to be used
--iconv=CONVERT_SPEC     request charset conversion of filenames
--checksum-seed=NUM      set block/file checksum seed (advanced)
--ipv4, -4               prefer IPv4
--ipv6, -6               prefer IPv6
--version, -V            print the version + other info and exit
--help, -h (*)           show this help (* -h is help only on its own)
Rsync can also be run as a daemon, in which case the following options are accepted:

--daemon                 run as an rsync daemon
--address=ADDRESS        bind to the specified address
--bwlimit=RATE           limit socket I/O bandwidth
--config=FILE            specify alternate rsyncd.conf file
--dparam=OVERRIDE, -M    override global daemon config parameter
--no-detach              do not detach from the parent
--port=PORT              listen on alternate port number
--log-file=FILE          override the "log file" setting
--log-file-format=FMT    override the "log format" setting
--sockopts=OPTIONS       specify custom TCP options
--verbose, -v            increase verbosity
--ipv4, -4               prefer IPv4
--ipv6, -6               prefer IPv6
--help, -h               show this help (when used with --daemon)

```
