### 222

```ymal
name: build-gitbook
on: [ push ]
jobs:
  build-gitbook-job:
    name: build-gitbook-job
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-java@v1
        with:
          java-version: '9.0.4' # The JDK version to make available on the path.
          java-package: jdk # (jre, jdk, or jdk+fx) - defaults to jdk
          architecture: x64 # (x64 or x86) - defaults to x64
      - uses: stCarolas/setup-maven@v4
        with:
          maven-version: 3.5.4
      - uses: actions/setup-node@v1
      - run: npm install -g gitbook-cli
      - run: gitbook install
      - run: git config --global user.email "secrets.EMAIL"
      - run: git config --global user.name "secrets.NAME"
      - run: wget https://github.com/huxiaoning/gitbook-helper/releases/download/1.0/gitbook-helper-1.0-SNAPSHOT.jar
      - run: java -jar -Dwork.dir=$(pwd) gitbook-helper-1.0-SNAPSHOT.jar
      - run: rm -rf docs
      - run: gitbook build
      - run: mv _book docs
      - run: git add .
      - run: git commit -m "UP"
      - run: git push
```

