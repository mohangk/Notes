

### mdfind

 mdfind -0 kind:folder bbm.ios | xargs -0 -J % mv % bbm_client_logs/

git archive --format zip --output /path/to/file.zip --prefix=newdir/ master

 Redirect stdout to one file and error to a different file
 1.	command > out 2>error

2.	Redirect stderr to stdout (&1), and then redirect stdout to a file: command >out 2>&1

 3.	Redirect both to a file: command &> out 
