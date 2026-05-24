# Floodlight & OpenDaylight Installation Guide — Ubuntu 20.04

## 1. Prerequisites
Ensure the following are available before starting:
- Ubuntu 20.04 LTS (x86_64)
- Internet connection
- `sudo` privileges
- At least 4GB RAM recommended

---

## 2. Installing Floodlight SDN Controller

### 2.1 Install Dependencies
```bash
sudo apt-get update
sudo apt-get install -y openjdk-8-jdk ant git thrift-compiler
java -version        # Should show openjdk 1.8.x
thrift --version     # Should show Thrift 0.13.x
```

### 2.2 Clone Floodlight Repository
```bash
cd ~
git clone https://github.com/floodlight/floodlight.git
cd floodlight
```

### 2.3 Fix `build.xml` — Thrift Output Path
The default `build.xml` causes double-nesting of generated Java files. Fix it:
```bash
sed -i 's|value="lib/gen-java"|value="lib"|g' ~/floodlight/build.xml
```

### 2.4 Upgrade Thrift Runtime JAR
The bundled `libthrift-0.9.1.jar` is incompatible with the 0.13.x compiler. Download the matching jar:
```bash
cd ~/floodlight/lib
wget https://repo1.maven.org/maven2/org/apache/thrift/libthrift/0.13.0/libthrift-0.13.0.jar
sed -i 's|libthrift-0.9.1.jar|libthrift-0.13.0.jar|g' ~/floodlight/build.xml
```

### 2.5 Fix Test File
Comment out an invalid `@Override` annotation in the test suite:
```bash
sed -i '105s/@Override/\/\/@Override/' ~/floodlight/src/test/java/net/floodlightcontroller/core/test/TestEventLoop.java
```

### 2.6 Build Floodlight
```bash
cd ~/floodlight
ant clean
ant gen-thrift
ant
```
> **Note:** Expected output: `BUILD SUCCESSFUL` — `floodlight.jar` created in `target/`

### 2.7 Run Floodlight
```bash
java -jar ~/floodlight/target/floodlight.jar
```
Verify it is running (new terminal):
```bash
curl http://localhost:8080/wm/core/health/json
```
> **Note:** Expected response: `{"healthy":true}`

---

## 3. Installing OpenDaylight (ODL)

### 3.1 Install Java 17
ODL 0.18.x requires Java 17. Install it alongside Java 8:
```bash
sudo apt-get install -y openjdk-17-jdk
/usr/lib/jvm/java-17-openjdk-amd64/bin/java -version
```

### 3.2 Download OpenDaylight
```bash
cd ~
wget https://nexus.opendaylight.org/content/repositories/opendaylight.release/org/opendaylight/integration/karaf/0.18.2/karaf-0.18.2.tar.gz
```
> **Note:** File size is ~240MB. Download may take a few minutes.

### 3.3 Extract and Start ODL
```bash
tar -xzf karaf-0.18.2.tar.gz
cd karaf-0.18.2
JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64 ./bin/karaf
```
> **Note:** Wait for the ODL banner and `opendaylight-user@root>` prompt.

### 3.4 Install ODL Features (inside Karaf console)
Type these commands one by one inside the Karaf shell:
```text
feature:install odl-restconf
feature:install odl-openflowplugin-flow-services
feature:install odl-mdsal-apidocs
```

### 3.5 Verify ODL (new terminal, keep Karaf running)
```bash
curl -u admin:admin http://localhost:8181/rests/data/network-topology:network-topology
```
> **Note:** Expected: JSON response with `data-missing` or empty topology (no switches connected yet). Both mean ODL is working.

---

## 4. Managing Multiple Java Versions
Floodlight uses Java 8. ODL uses Java 17. Both can coexist:

- **Switch system default Java:**
  ```bash
  sudo update-alternatives --config java
  ```
- **Run ODL with Java 17 without changing system default:**
  ```bash
  JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64 ~/karaf-0.18.2/bin/karaf
  ```
- **Run Floodlight (uses system Java 8 by default):**
  ```bash
  java -jar ~/floodlight/target/floodlight.jar
  ```

---

## 5. Testing with Mininet
Mininet creates a virtual network to test your SDN controller.

- **Connect Mininet to Floodlight (port 6653):**
  ```bash
  sudo mn --controller=remote,ip=127.0.0.1,port=6653 --topo=tree,2
  ```
- **Connect Mininet to OpenDaylight (port 6633):**
  ```bash
  sudo mn --controller=remote,ip=127.0.0.1,port=6633 --topo=tree,2
  ```

---

## 6. Quick Reference

| Task | Command | Port |
| :--- | :--- | :---: |
| Start Floodlight | `java -jar ~/floodlight/target/floodlight.jar` | 8080 |
| Start ODL | `JAVA_HOME=.../java-17... ./bin/karaf` | 8181 |
| Floodlight health | `curl localhost:8080/wm/core/health/json` | 8080 |
| ODL REST API | `curl -u admin:admin localhost:8181/rests/...` | 8181 |
| Mininet → Floodlight | `sudo mn --controller=remote,port=6653` | 6653 |
| Mininet → ODL | `sudo mn --controller=remote,port=6633` | 6633 |
