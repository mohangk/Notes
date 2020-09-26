#### Basic commands

- GoBuild, GoInstall, GoTest, GoRun

#### Imports related commands

- GoImport package-name. Supports completion with tab
- GoImportAs - rename the package name in the import
- GoDrop - drops a package
- GoImports runs goimports - set as the default to run after save

#### Test related commands

- GoTest - Can run tests for the referred to source code, doenst have to be in the test code

- GoTestFunc - Runs tests for the function under the cursor

- GoTestCompile - Compile test but dont run it

- GoCoverage - runs go test -coverprofile tempfile.  Highlights parts of the code with no tests. GoCoverageClear - removes the highlighting. Just use GoCoverageToggle

#### Linting

- errcheck - checks unchecked errors in go

- go vet - heuristic checks to find errors

- golint - stylistics that are mentioned in effoective go and code sytle guide

##### Navigation between files

- :GoAlternate - between code and  the test file 
  
  - Added vim commands for 
    
    - :A - open in buffer
    
    - :Av - 

- :GoDef - gd, or Ctrl-] -- to jump to the definition

- :GoDefPop - to go back to where you were before jumpting to the definition or Ctrl-t

- :GoDefStack - see how deep you have gone on definitions. GoDefStackClear - clears the stack

- :GoDecls - lists out all the decalarations in the file, GoDeclsDir, lists out all definition in the director

##### Access to documentation and information

- :GoDock or "K" - pulls up doc for that particular function etc    

- GoDocBrowser - open the docs in the browser

- :GoInfo - shows information about the identifier in the status bar, <leader>i and also automatically if waiting long enough

- :GoSameIds - highlight all common identifiers, does this automatically based on vimrc setting

##### Additional access to information using guru (package wide scanning)

- GoImplements - lists out the interfaces that is implemented

- GoReferrers - Lists out where the identifer is being used

- GoDescribe - shows the declarations and where 

- GoWhicherrs - shows what are the possible errors returned?

- GoChannelPeers - shows where the channel is declared, values are set, values are retrieved

- GoCallees - for anonymous functions, get the functions that are called at that site

- GoCallers - Identify where the  function get  called

- GoCallstack - Root of the callgraph to where the curson is pointing

##### List all files related to the project

- GoFiles: - lists files from the same package as the current file

- GoDeps - lists all files that are dependencies

##### Refactoring

- GoRename - renames identifiers correctl using gopls - need to set` let g:go_rename_command = 'gopls'`

- GoFreevars - Identifies all the inputs (variables that are referenced but not defined in the selection). This will tell you what you need to pass in if you were to extract that selection

##### Code generation

- GoGenerate - create methods stubs from interface

##### Vim movements

###### Move between functions

- ]] -> jumpt to next function

- [[ -> jump to previous function

###### Text objects - function

- "af" - a function

- "if" - in function

- yaf, vaf, dif etc

###### struct split and join

- "gS" - split struct into multiline
- "gJ" - join multiline struct into one line

###### snippets - press tab after keying in the following

- errp -> check if error is !nil, and panic
- fn, ff -> fmt.Println, fmt.Printf
- ln, lf -> log.Println, log.Printf
- json -> json:"field name"
