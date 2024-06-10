# Quarkus Cheat Sheet

## quarkus installation

```

curl -Ls https://sh.jbang.dev | bash -s - trust add https://repo1.maven.org/maven2/io/quarkus/quarkus-cli/
curl -Ls https://sh.jbang.dev | bash -s - app install --fresh --force quarkus@quarkusio
```

## create quarkus project

```

quarkus create && cd code-with-quarkus
quarkus dev
```

then open http://localhost:8080/

* page: src/main/resources/META-INF/resources/index.html
* App configuration: src/main/resources/application.properties
* Static assets: src/main/resources/META-INF/resources/
* Code: src/main/java