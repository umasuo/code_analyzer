# code_analyzer
the tools we used to analyze our code

## how to use this project

add the project as a submodule, so that you can keep your resources up to date

`git submodule add https://github.com/reactivesw/code_analyzer.git`

use the fellow command to update the submodule

`git submodule foreach git pull`

go to the submodule's folder and use `git add ` `git commit ` `git push` to submit your change

if you want to use your own config, please use your own branch, like `customer_authentication`, and add `branch = customer_authentication` to your file: `.gitmodules`
