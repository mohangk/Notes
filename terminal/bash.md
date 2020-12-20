###### systemctl

systemctl list-unit-files  -- get all installed systemd files

##### dmesg

tail -f | dmesg  

##### git

###### dealing with tags

[What is git tag, How to create tags &amp; How to checkout git remote tag(s) - Stack Overflow](https://stackoverflow.com/questions/35979642/what-is-git-tag-how-to-create-tags-how-to-checkout-git-remote-tags)

Shallow clone a specific tag

```
$ git clone <url> --branch=<tag_name>
```

Checkout the tag in the working dir

```
# Update the local git repo with the latest tags from all remotes
$ git fetch --all

# checkout the specific tag
$ git checkout tags/<tag> -b <branch>
```

##### split list of files to multiple subfolders

```
ls -1 | wc -l

read -p 'How Many Directories: ' F;
read -p 'Sub-Directories Prefix: ' S;

PARRENT=${PWD}
# cd $PARRENT 
n=0
for i in *
do
  if [ $((n+=1)) -gt $F ]; then
    n=1
  fi
  todir=$PARRENT/"$S"_$n
  [ -d "$todir" ] || mkdir "$todir" 
  mv "$i" "$todir" 
done
```
