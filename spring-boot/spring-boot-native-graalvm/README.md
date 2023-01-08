
Build:
```shell
wget -c https://github.com/graalvm/graalvm-ce-builds/releases/download/vm-22.3.0/graalvm-ce-java11-linux-amd64-22.3.0.tar.gz -O - | tar -xz -C $HOME

export GraalVM_HOME=$HOME/graalvm-ce-java11-22.3.0
alias switchOnGraalVM='export PATH=${GraalVM_HOME}/bin:$PATH'
alias switchOnGraalVMJavaHome="export JAVA_HOME=$GraalVM_HOME"
alias switchOffGraalVM='export PATH=`echo $PATH | tr ":" "\n" | grep -v "graalvm" | tr "\n" ":"`'
alias switchOffGraalVMJavaHome="export JAVA_HOME=$JAVA_HOME"

switchOnGraalVM
switchOnGraalVMJavaHome

# build jar and start
mvn spring-boot:run

curl http://localhost:8080/hi-graalvm

# build native-image binary
gu install native-image
mvn clean package -Pnative

target/spring-boot-native-graalvm

curl http://localhost:8080/hi-graalvm

# build docker image
mvn spring-boot:build-image

docker run -it --rm -p 8080:8080 spring-boot-native-graalvm:1.0-SNAPSHOT

curl http://localhost:8080/hi-graalvm

switchOffGraalVM
switchOffGraalVMJavaHome

```
