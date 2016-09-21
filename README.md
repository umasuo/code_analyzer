# code_analyzer
the tools we used to analyze our code

## how to use this project

add the project as a submodule, so that you can keep your resources up to date

`git submodule add https://github.com/reactivesw/code_analyzer_test.git`

use the fellow command to update the submodule

`git submodule foreach git pull`


# add your own config
add folder:`code_analyzer_test_local` to your project root, it contains two files:`code_analyzer.gradle`,`code_unit_test.gradle`
## code_analyzer.gradle
>     apply plugin: 'java'
>     apply plugin: 'maven'
>     apply plugin: 'checkstyle'
>     apply plugin: 'pmd'
>     apply plugin: 'findbugs'
>     apply from:'code_analyzer_test/code_analyzer_config.gradle'
>     
>     repositories {
>       maven { url "http://jcenter.bintray.com" }
>       maven { url "https://repo.maven.apache.org/maven2" }
>     }
>     
>     /*************checkstyle(use google java style)***************/
>     
>     checkstyle{
>       // exclude the package you do not want to check
>       // your own config
>       //checkstyleMain.exclude 'io/reactivesw/customerauthentication/grpc/*'
>     }
>     
>     /*************PMD(Project Manager Design)***************/
>     tasks.withType(Pmd) {
>       //exclude the package you do not want to check
>       // your own config
>       //exclude 'io/reactivesw/customerauthentication/App*'
>     }
>     
>     
>     /*************find bug***************/
>     tasks.withType(FindBugs) {
>       //exclude the package you do not want to check
>       classes = classes.filter {
>         // your own config
>         //!it.path.contains('io/reactivesw/customerauthentication/grpc/')
>       }
>     }
> 

## code_unit_test.gradle
>     apply plugin: "com.palantir.jacoco-coverage"
>     apply plugin: 'groovy'
>     apply from:'code_analyzer_test/unit_test_config.gradle'
>     
>     //config of coverage check, see document: https://github.com/palantir/gradle-jacoco-coverage
>     jacocoTestReport {
>       afterEvaluate {
>         classDirectories = files(classDirectories.files.collect {
>           fileTree(dir: it, exclude: [
>             // your own config
>             //'io/reactivesw/customerauthentication/grpc/*',
>           ])
>         })
>       }
>     }
>     
> 

# change build.gradle
>     apply from: 'code_analyzer_test_local/code_analyzer.gradle'
>     apply from: 'code_analyzer_test_local/code_unit_test.gradle'
