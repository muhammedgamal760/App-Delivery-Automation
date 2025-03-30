# You can change this base image to anything else
# But make sure to use the correct version of Java
FROM abhishekf5/maven-abhishek-docker-agent:v1

RUN apt-get update && apt-get install -y openjdk-17-jdk && \
    update-alternatives --install /usr/bin/java java /usr/lib/jvm/java-17-openjdk-amd64/bin/java 1 && \
    update-alternatives --set java /usr/lib/jvm/java-17-openjdk-amd64/bin/java && \
    apt-get update && apt-get install -y openjdk-17-jdk && \
    export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64 && \
    export PATH=$JAVA_HOME/bin:$PATH && \
    echo "JAVA version:" && \
    java -version
