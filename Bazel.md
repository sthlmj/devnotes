# Getting started with bazel and jenkins
More info: https://blog.nrwl.io/dockerize-apps-with-jenkins-and-bazel-797c7964ae3b?gi=3db02254a07b
More info on Bazel build python: https://pycon.org.il/2016/static/sessions/benjamin-peterson.pdf

### Example pipeline to run bazel on a jenkins agent

```
pipeline {
  agent any
  stages {
    stage('Publish image') {
      steps {
        sh 'bazel run //src/server:push'
      }
    }
  }
}
```

### Bazel BUILD file
```
BUILD
##################
py_library(
   name = 'greeting',
   srcs = ['greeting.py'],
)

py_binary(
   name = 'hello',
   main = 'hello.py',
   srcs = ['hello.py'],
   deps = [':greeting'],
)
```


### greeting.py
```
greeting.py
##############
def greet(name):
   print("Hello, {}.".format(name))
```

### hello.py
```
hello.py
###############
from greeting import greet
greet("Joe")
```


### Trying it out with bazel
```
Trying it out with bazel
###################
$ bazel build hello
$ bazel-bin/hello
```

### Bazel query
```
bazel query 
#####################
$ bazel query 'deps(hello)' 
greeting 
greeting.py 
hello.py 
$ bazel query 'kind("source file", deps(hello))' 
greeting.py 
hello.py
```
