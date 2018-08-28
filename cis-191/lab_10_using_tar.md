
## Introduction 

In this lab you will use the TAR command to do incremental backups.

## Part 1: Create Files 

Start by creating files and directories to backup.

```
mkdir -p foo/bar/bak
mkdir -p foo/baz
mkdir -p foo/zot
echo "partone" > foo/file1
echo "partone" > foo/file2
echo "partone" > foo/bar/file3
echo "partone" > foo/bar/file4
echo "partone" > foo/baz/file5
echo "partone" > foo/baz/file6
echo "partone" > foo/zot/file7
echo "partone" > foo/zot/file8
```

If you preformed this correctly you should see this output from the tree command:

```
$ tree foo/
foo/
âââ bar
â  âââ bak
â  âââ file3
â  âââ file4
âââ baz
â  âââ file5
â  âââ file6
âââ file1
âââ file2
âââ zot
  âââ file7
  âââ file8
4 directories, 8 files
```

## Part 2: Backup the Data 

Create a tar files that contain your directory structure. Create tar files using:
  - No compression
  - GZIP compression
  - BZIP2 compression
  - XZ compression

Which file is largest?
Verify that each of your TAR files is correct using the -t option to list the contents of the file:

```
tar -tvf <your-tar-file-here>
```

The files should contain a directory that looks like this:

```
$ tar -tvf foo.tar.xz
drwxrwxr-x mimatera/mimatera 0 2015-11-12 12:33 foo/
-rw-rw-r-- mimatera/mimatera 8 2015-11-12 12:33 foo/file2
drwxrwxr-x mimatera/mimatera 0 2015-11-12 12:33 foo/baz/
-rw-rw-r-- mimatera/mimatera 8 2015-11-12 12:33 foo/baz/file6
-rw-rw-r-- mimatera/mimatera 8 2015-11-12 12:33 foo/baz/file5
drwxrwxr-x mimatera/mimatera 0 2015-11-12 12:33 foo/zot/
-rw-rw-r-- mimatera/mimatera 8 2015-11-12 12:33 foo/zot/file7
-rw-rw-r-- mimatera/mimatera 8 2015-11-12 12:33 foo/zot/file8
drwxrwxr-x mimatera/mimatera 0 2015-11-12 12:33 foo/bar/
drwxrwxr-x mimatera/mimatera 0 2015-11-12 12:33 foo/bar/bak/
-rw-rw-r-- mimatera/mimatera 8 2015-11-12 12:33 foo/bar/file4
-rw-rw-r-- mimatera/mimatera 8 2015-11-12 12:33 foo/bar/file3
-rw-rw-r-- mimatera/mimatera 8 2015-11-12 12:33 foo/file1
```

Does your TAR file contain relative or absolute paths? How do you know?

## Part 3: Create an Incremental Backup 

Start by creating a full backup of your directory:

```
tar -g foo.table.0.snar -cvf foo.0.tar foo/
```

Now update some of the files in your directory:

```
echo "partthree" >> foo/file1
echo "partthree" >> foo/baz/file5
echo "partthree" >> foo/zot/file8
rm foo/zot/file7
```

The TAR command overwrites the file table that you give to the -g option. So you should make a copy.

```
cp foo.table.0.snar foo.table.1.snar
tar -g foo.table.1.snar -cvf foo.1.tar foo/
```

What files does foo.1.tar contain?

## Part 4: Restoring Files 

Now you will restore the files from you full and incremental backups. You must untar your backups in order:

```
tar -G -xvf foo.0.tar
tar -G -xvf foo.1.tar
```

The rule is if you used the "-g" option to tar you should use the "-G" option when you untar. Do the above steps again without the -G what's the difference?

