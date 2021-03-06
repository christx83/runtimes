---

copyright:
  years: 2015, 2018
lastupdated: "2018-07-03"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}

# Optionen zur Durchführung von Push-Operationen für Liberty-Apps
{: #options_for_pushing}

Das Verhalten des Liberty-Servers in {{site.data.keyword.Bluemix}} wird durch das Liberty-Buildpack gesteuert. Buildpacks können eine vollständige Laufzeitumgebung für bestimmte Anwendungsklassen bereitstellen. Sie sind der Schlüssel für die Portierbarkeit in Clouds und die Bereitstellung einer offenen Cloudarchitektur. Mit dem Liberty-Buildpack wird ein WebSphere Liberty-Container bereitgestellt, mit dem Java EE 7- und OSGi-Anwendungen ausgeführt werden können. Es unterstützt gängige Frameworks wie Spring und umfasst die IBM JRE. WebSphere Liberty ermöglicht eine zeiteffiziente, an die Cloud angepasste Anwendungsentwicklung. Das Liberty-Buildpack unterstützt mehrere Anwendungen, die in einem einzigen Liberty-Server implementiert werden. Im Rahmen der Integration des Liberty-Buildpacks in {{site.data.keyword.Bluemix_notm}} stellt das Buildpack sicher, dass die Umgebungsvariablen zum Binden von Services im Liberty-Server als Konfigurationsvariablen dargestellt werden.

Zur Implementierung Ihrer Liberty-Anwendungen in {{site.data.keyword.Bluemix_notm}} können Sie die folgenden Methoden verwenden:

* Eigenständige Anwendung mit einer Push-Operation übertragen
* Serververzeichnis mit einer Push-Operation übertragen
* Paketierten Server mit einer Push-Operation übertragen

Wichtig: Wenn Sie eine Anwendung mit dem Liberty-Buildpack bereitstellen, müssen Sie als Speicherbegrenzung für Ihre Anwendung mindestens 512 MB angeben. Weitere Informationen finden Sie in [Speicherbegrenzungen und das Liberty-Buildpack](memoryLimits.html).

## Eigenständige Apps
{: #stand_alone_apps}

Eigenständige Anwendungen, wie z. B. die in WAR- oder EAR-Dateien enthaltenen Anwendungen, können in {{site.data.keyword.Bluemix_notm}} für Liberty implementiert werden.

Zur Implementierung einer eigenständigen Anwendung müssen Sie den Befehl `ibmcloud cf push` mit dem Parameter '-p' ausführen, der auf Ihre WAR- oder EAR-Datei verweist.
Beispiel:

```
    ibmcloud cf push <yourappname> -p myapp.war
```
{: codeblock}

Wenn eine eigenständige Anwendung implementiert wird, dann wird für diese Anwendung eine Liberty-Standardkonfiguration bereitgestellt. Die Standardkonfiguration ermöglicht die
Nutzung der folgenden Liberty-Features:

* beanValidation-1.1
* [cdi-1.2](optionsForPushing.html#cdi12)
* ejbLite-3.2
* el-3.0
* jaxrs-2.0
* jdbc-4.1
* jndi-1.0
* jpa-2.1
* jsf-2.2
* jsonp-1.0
* jsp-2.3
* managedBeans-1.0
* servlet-3.1
* websocket-1.1
* icap:managementConnector-1.0
* appstate-2.0

Diese Features entsprechen den Java EE 7 Web Profile-Features. Sie können eine andere Gruppe von Liberty-Features angeben, indem Sie die Umgebungsvariable JBP_CONFIG_LIBERTY festlegen. Um zum Beispiel nur die Features
'jsp-2.3' und 'websocket-1.1' zu aktivieren, führen Sie mit dem folgenden Befehl ein erneutes Staging für die Anwendung aus:

```
    ibmcloud cf set-env myapp JBP_CONFIG_LIBERTY "app_archive: {features: [jsp-2.3, websocket-1.1]}"
```
{: codeblock}

Hinweis: Die besten Ergebnisse erzielen Sie, wenn Sie die Liberty-Features mit der Umgebungsvariablen JBP_CONFIG_LIBERTY festlegen oder Ihre Anwendung als [Serververzeichnis](optionsForPushing.html#server_directory) oder [paketierten Server](optionsForPushing.html#packaged_server) mit einer angepassten Datei 'server.xml' bereitstellen. Indem Sie diese Umgebungsvariable festlegen, stellen Sie sicher, dass Ihre Anwendung nur das Feature verwendet, das auch
benötigt wird, und dass sie nicht von den Änderungen am Liberty-Standard-Feature-Set des Buildpacks beeinträchtigt wird. Wenn die benötigte
Liberty-Konfiguration über das Feature-Set hinausgeht, verwenden Sie die Optionen für
[Serververzeichnisse](optionsForPushing.html#server_directory) oder
[paketierte Server](optionsForPushing.html#packaged_server), um die Anwendung bereitzustellen.

Wenn Sie eine WAR-Datei bereitgestellt haben, können Sie über das Kontextstammverzeichnis, das in der eingebetteten Datei 'ibm-web-ext.xml' festgelegt ist, auf die Webanwendung zugreifen. Wenn die Datei 'ibm-web-ext.xml' nicht vorhanden ist oder das Kontextstammverzeichnis darin nicht angegeben ist, können Sie über den Stammkontext auf die Anwendung zugreifen. Beispiel:

```
    http://<yourappname>.mybluemix.net/
```
{: codeblock}

Wenn Sie eine EAR-Datei implementiert haben, dann kann auf die eingebettete Webanwendung unter dem Kontextstammverzeichnis zugegriffen werden, das im EAR-Implementierungsdeskriptor definiert ist. Beispiel:

```
    http://<yourappname>.mybluemix.net/acme/
```
{: codeblock}

Die vollständige Standardkonfigurationsdatei 'server.xml' für Liberty lautet wie folgt:
```
    <server>
       <featureManager>
          <feature>beanValidation-1.1</feature>
          <feature>cdi-1.2</feature>
          <feature>ejbLite-3.2</feature>
          <feature>el-3.0</feature>
          <feature>jaxrs-2.0</feature>
          <feature>jdbc-4.1</feature>
          <feature>jndi-1.0</feature>
          <feature>jpa-2.1</feature>
          <feature>jsf-2.2</feature>
          <feature>jsonp-1.0</feature>
          <feature>jsp-2.3</feature>
          <feature>managedBeans-1.0</feature>
          <feature>servlet-3.1</feature>
          <feature>websocket-1.1</feature>
          <feature>icap:managementConnector-1.0</feature>
          <feature>appstate-2.0</feature>
       </featureManager>

       <application name='myapp' location='myapp.war' type='war' context-root='/'/>
       <httpEndpoint id='defaultHttpEndpoint' host='*' httpPort='${port}'/>
       <webContainer trustHostHeaderPort='true' extractHostHeaderPort='true'/>
       <include location='runtime-vars.xml'/>
       <logging logDirectory='${application.log.dir}' consoleLogLevel='INFO'/>
       <httpDispatcher enableWelcomePage='false'/>
       <applicationMonitor dropinsEnabled='false' updateTrigger='mbean'/>
       <config updateTrigger='mbean'/>
       <cdi12 enableImplicitBeanArchives='false'/>
       <appstate2 appName='myapp'/>
    </server>
```
{: codeblock}

### CDI 1.2
{: #cdi12}

Für eine bessere Leistung ist die [Funktion für das implizite Bean-Archiv-Scannen in CDI 1.2](https://www.ibm.com/support/knowledgecenter/SSAW57_8.5.5/com.ibm.websphere.wlp.nd.doc/ae/cwlp_cdi_behavior.html) standardmäßig inaktiviert,
wenn nur WAR- und EAR-Dateien bereitgestellt werden. Die Funktion für das implizite Bean-Archiv-Scannen kann über die Umgebungsvariable JBP_CONFIG_LIBERTY aktiviert werden.
Beispiel:

```
    ibmcloud cf set-env myapp JBP_CONFIG_LIBERTY "app_archive: { implicit_cdi: true }"
```    
{: codeblock}

Wichtig: Damit Ihre Änderungen an der Umgebungsvariablen wirksam werden, müssen Sie ein erneutes Staging für Ihre Anwendung durchführen:

```
    ibmcloud cf restage myapp
```
{: codeblock}

## Serververzeichnis
{: #server_directory}

In einigen Fällen müssen Sie möglicherweise eine angepasste Liberty-Serverkonfiguration mit Ihrer Anwendung bereitstellen. Diese angepasste Konfiguration kann erforderlich sein, wenn Sie eine WAR- oder EAR-Datei bereitstellen und die Standarddatei 'server.xml' bestimmte Einstellungen nicht enthält, die für Ihre Anwendung benötigt werden.

Wenn Sie das Liberty-Profil auf Ihrer Workstation installiert und bereits einen Liberty-Server mit Ihrer Anwendung erstellt haben, können Sie den Inhalt dieses Verzeichnisses mit einer Push-Operation an {{site.data.keyword.Bluemix_notm}} übertragen.
Wenn Ihr Liberty-Server beispielsweise den Namen 'defaultServer' hat, führen Sie den folgenden Befehl aus:

```
    ibmcloud cf push <yourappname> -p wlp/usr/servers/defaultServer
```
{: codeblock}

Wenn auf Ihrer Workstation kein Liberty-Profil installiert ist,
können Sie die folgenden Schritte befolgen, um ein Serververzeichnis mit Ihrer Anwendung zu erstellen:

1. Erstellen Sie ein Verzeichnis mit dem Namen 'defaultServer'.
2. Erstellen Sie im Verzeichnis 'defaultServer' ein Verzeichnis mit dem Namen 'apps'.
3. Kopieren Sie die WAR- oder EAR-Datei in das Verzeichnis 'defaultServer/apps'.
4. Erstellen Sie im Verzeichnis 'defaultServer' eine Datei 'server.xml' mit dem folgenden Beispielinhalt.  Zusätzlich:
  * Vergewissern Sie sich, dass das Attribut 'location' oder 'type' des Anwendungselements so aktualisiert wurde, dass die Angaben mit dem Dateinamen und dem Typ Ihrer Anwendung übereinstimmen.
  * Die im Diagramm enthaltene Datei 'server.xml' enthält ein minimales Feature-Set. Möglicherweise müssen Sie das Feature-Set abhängig von den Anforderungen Ihrer Anwendung anpassen.

```
    <server>
        <featureManager>
            <feature>jsp-2.3</feature>
        </featureManager>

        <httpEndpoint id="defaultHttpEndpoint" host="*" httpPort="8080" />

        <application name="myapp" context-root="/" type="war" location="myapp.war"/>
    </server>
```
{: codeblock}

Nach Vorbereitung des Serververzeichnisses können Sie es in {{site.data.keyword.Bluemix_notm}} implementieren.

```
    ibmcloud cf push <yourappname> -p defaultServer
```
{: codeblock}

Hinweis: Auf die Webanwendungen, die als Teil des Serververzeichnisses implementiert werden, kann unter dem [Kontextstammverzeichnis gemäß Liberty-Profil](http://www.ibm.com/support/knowledgecenter/SSAW57_8.5.5/com.ibm.websphere.wlp.nd.doc/ae/twlp_dep_war.html?cp=SSAW57_8.5.5%2F1-3-11-0-5-6) zugegriffen werden. Beispiel:

```
    http://<yourappname>.mybluemix.net/acme/
```
{: codeblock}

## Paketierter Server
{: #packaged_server}

Sie können auch eine Datei für einen paketierten Server mit einer Push-Operation an {{site.data.keyword.Bluemix_notm}} übertragen. Die Datei für den paketierten Server wird mithilfe des Liberty-Befehls zum Erstellen von Serverpaketen erstellt. Zusätzlich zu den Anwendungs- und Konfigurationsdateien kann die Datei des paketierten Servers gemeinsam genutzte Ressourcen und Liberty-Benutzerfeatures enthalten, die von der Anwendung benötigt werden.

Verwenden Sie zum Paketieren eines Liberty-Servers den Befehl `./bin/server package` aus dem Liberty-Installationsverzeichnis. Geben Sie den Servernamen an und verwenden Sie die Option `--include=usr`.
Wenn Ihr Liberty-Server beispielsweise den Namen `defaultServer` hat, führen Sie den folgenden Befehl aus:

```
    wlp/bin/server package defaultServer --include=usr
```
{: codeblock}

Dieser Befehl generiert die Datei 'serverName.zip' im Serververzeichnis. Wenn Sie mit der Option `--archive` eine andere Archivdatei angegeben haben, stellen Sie sicher, dass die Erweiterung `.zip` anstelle von `.jar` verwendet wird. **Das Buildpack unterstützt keine paketierten Serverdateien mit der Erweiterung `.jar`**.

Anschließend können Sie die generierte `.zip`-Datei mithilfe des Befehls `ibmcloud cf push` an {{site.data.keyword.Bluemix_notm}} übertragen.
Beispiel:

```
    ibmcloud cf push <yourappname> -p wlp/usr/servers/defaultServer/defaultServer.zip
```
{: codeblock}

Hinweis: Auf die Webanwendungen, die als Teil des paketierten Servers implementiert werden, kann unter dem Kontextstammverzeichnis gemäß Liberty-Profil zugegriffen werden.

### Änderung der Datei 'server.xml'
{: #modifications_of_serverxml}

Bei der Übertragung eines paketierten Servers oder eines Liberty-Serververzeichnisses mit einer Push-Operation erkennt das Liberty-Buildpack die Datei 'server.xml' zusammen mit Ihrer Anwendung. Das Liberty-Buildpack nimmt die folgenden Änderungen an der Datei 'server.xml' vor:

* Das Buildpack stellt sicher, dass sich genau ein Element 'httpEndpoint' in der Datei befindet.
* Das Buildpack stellt sicher, dass das Attribut 'httpPort' im Element 'httpEndpoint' auf eine Systemvariable namens '${port}' verweist. Das Buildpack überschreibt außerdem das Hostattribut.
* Das Buildpack definiert das Attribut 'logDirectory' im Protokollierungselement so, dass auf ein Systemprotokollverzeichnis verwiesen wird.
* Das Buildpack stellt sicher, dass die Datei 'runtime-vars.xml' logisch mit der Datei 'server.xml' zusammengeführt wird. Insbesondere fügt das Buildpack der Datei 'server.xml' die Zeile *&lt;include location="runtime-vars.xml" /&gt;* hinzu.

### Referenzierbare Variablen
{: #referenceable_variables}

Die folgenden Variablen sind in der Datei `runtime-vars.xml` definiert und werden von der mit einer Push-Operation übertragenen Datei `server.xml` referenziert. Bei allen Variablen muss die Groß-/Kleinschreibung beachtet werden.

* ${port}: Der HTTP-Port, an dem der Liberty-Server empfangsbereit ist.
* ${vcap_app_port}: Identisch mit ${port}. Wird bei der Ausführung unter Diego nicht definiert.
* ${application_name}: Der Name der Anwendung wie mithilfe der Optionen im Befehl `ibmcloud cf push` definiert.
* ${application_version}: Die Version dieser Anwendungsinstanz in Form einer UUID wie beispielsweise `b687ea75-49f0-456e-b69d-e36e8a854caa`. Diese Variable ändert sich bei jeder weiteren Push-Operation für die Anwendung, die neuen Code oder Änderungen an den Anwendungsartefakten enthält.
* ${host}: Die IP-Adresse der Anwendungsinstanz.
* ${application_uris}: Ein JSON-Array der Endpunkte, die für den Zugriff auf diese Anwendung verwendet werden können, zum Beispiel: 'myapp.mydomain.com'.
* ${start}: Der Zeitpunkt (Uhrzeit und Datum), an dem die Anwendung gestartet wurde, in einem Format ähnlich wie `2013-08-22 10:10:18 -0400`. Wird bei der Ausführung unter Diego nicht definiert.

### Auf Informationen gebundener Services zugreifen
{: #accessing_info_of_bound_services}

Wenn Sie einen Service an Ihre Anwendung binden möchten, sind Informationen zum Service, wie beispielsweise Verbindungsberechtigungsnachweise, in der Umgebungsvariablen [VCAP_SERVICES ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.cloudfoundry.org/devguide/deploy-apps/environment-variable.html#VCAP-SERVICES) enthalten, die Cloud Foundry für die Anwendung festlegt. Für [automatisch konfigurierte Services](autoConfig.html) werden Servicebindungseinträge in der Datei 'server.xml' vom Liberty-Buildpack generiert oder aktualisiert. Der Inhalt der Servicebindungseinträge kann in einem der folgenden Formate vorliegen:

* cloud.services.&lt;service-name&gt;.&lt;property&gt; - Gibt Informationen wie Name, Typ und Plan des Service an.
* cloud.services.&lt;service-name&gt;.connection.&lt;property&gt; - Gibt die Verbindungsinformationen für den Service an.

Die typischen Informationen lauten wie folgt:
* name: Der Name des Service; zum Beispiel 'mysql-e3abd'.
* label: Der Typ des erstellten Service; zum Beispiel 'mysql-5.5'.
* plan: Der Serviceplan gemäß der eindeutigen ID für diesen Plan; zum Beispiel 100.
* connection.name: Eine eindeutige Kennung für die Verbindung in Form einer UUID; zum Beispiel d01af3a5fabeb4d45bb321fe114d652ee.
* connection.hostname: Der Hostname des Servers, der den Service ausführt; zum Beispiel mysql-server.mydomain.com.
* connection.host: Die IP-Adresse des Servers, der den Service ausführt; zum Beispiel 9.37.193.2.
* connection.port: Der Port, auf dem der Service für eingehende Verbindungen empfangsbereit ist; zum Beispiel 3306,3307.
* connection.user: Der Benutzername zur Authentifizierung dieser Anwendung für den Service. Der Benutzername wird automatisch von Cloud Foundry generiert; zum Beispiel unHwANpjAG5wT.
* connection.username: Ein Aliasname für 'connection.user'.
* connection.password: Das Kennwort zur Authentifizierung dieser Anwendung für den Service. Das Kennwort wird automatisch von Cloud Foundry generiert; zum Beispiel pvyCY0YzX9pu5.

Bei gebundenen Services, die nicht automatisch vom Liberty-Buildpack konfiguriert werden, muss die Anwendung den Zugriff der Back-End-Ressource selbst verwalten.
