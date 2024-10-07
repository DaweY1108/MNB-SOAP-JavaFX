<h3>Magyar Nemzeti Bank SOAP Implementalasa:</h3>

<h3>1. Lepes</h3>
<h5>A pom.xml-ben a dependencies részhez hozzaadjuk a kovetkezoket:</h5>

```xml
<dependency>
    <groupId>jakarta.xml.bind</groupId>
    <artifactId>jakarta.xml.bind-api</artifactId>
    <version>4.0.2</version>
</dependency>
<dependency>
    <groupId>jakarta.xml.ws</groupId>
    <artifactId>jakarta.xml.ws-api</artifactId>
    <version>4.0.2</version>
</dependency>
<dependency>
    <groupId>com.sun.xml.ws</groupId>
    <artifactId>jaxws-rt</artifactId>
    <version>4.0.2</version>
</dependency>
<dependency>
    <groupId>org.glassfish.hk2</groupId>
    <artifactId>osgi-resource-locator</artifactId>
    <version>2.5.0-b42</version>
</dependency>
```
<h3>2. Lepes</h3>
<h5>A pom.xml-ben a build plugins reszhez hozzáadjuk a következőt:</h5>

```xml
<build>
    <plugins>
        .
        .
        .
        <plugin>
            <groupId>com.sun.xml.ws</groupId>
            <artifactId>jaxws-maven-plugin</artifactId>
            <version>4.0.2</version>
            <executions>
                <execution>
                    <goals>
                        <goal>wsimport</goal>
                    </goals>
                </execution>
            </executions>
            <configuration>
                <wsdlUrls>
                    <wsdlUrl>http://www.mnb.hu/arfolyamok.asmx?wsdl</wsdlUrl>
                </wsdlUrls>
                <packageName>soap</packageName>
                <sourceDestDir>
                    src/main/java
                </sourceDestDir>
            </configuration>
        </plugin>
    </plugins>
</build>
```

<h3>3. Lepes</h3>
<h5>Jobb oldalt talalunk egy m gombot, <br>ott lenyitjuk a projekt nevet, <br><br>majd Lifecycle->clean -re kattintunk. <br><br>Ha lefutott, akkor Lifecycle->install -ra kattintunk.<br><br> Ha lefutott es mindent jol csinaltunk, akkor kell lennie egy soap mappanak a src/main/java konyvtaron belul.</h5>

<h3>4. Lepes</h3>
<h5>Megnyitjuk a src/main/java/module-info.java fajlt<br><br>Hozzaadjuk a kovetkezoket:</h5>

```java
requires jakarta.xml.ws;
opens soap to com.sun.xml.bind, com.sun.xml.ws;
opens {package név pl.: me.dawey.erettsegifx} to javafx.fxml, com.sun.xml.bind;
```

<h3>5. Lepes</h3>
<h5>Ha mindennel megvagyunk tesztelhetjuk a kovetkezo kod segitsegevel:</h5>

```java
MNBArfolyamServiceSoapImpl impl = new MNBArfolyamServiceSoapImpl();
MNBArfolyamServiceSoap service = impl.getCustomBindingMNBArfolyamServiceSoap();
try {
    System.out.println(service.getInfo());
    System.out.println(service.getCurrentExchangeRates());
    System.out.println(service.getExchangeRates("2022-08-14","2022-09-14","EUR"));
} catch (Exception e) {
    System.err.print(e);
}
```
