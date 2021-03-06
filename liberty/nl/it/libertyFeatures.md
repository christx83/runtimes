---

copyright:
  years: 2015, 2018
lastupdated: "2018-09-21"

---

{:new_window: target="_blank"}
{:codeblock: .codeblock}

# Funzioni Liberty supportate in {{site.data.keyword.cloud_notm}}
{: #liberty_features}

Il runtime Liberty for Java include un sottoinsieme di funzioni Liberty. Per utilizzare una funzione che non è inclusa nel runtime, vedi [Installa le funzioni Liberty](installFeatures.html). Per un elenco completo delle funzioni disponibili in Liberty, insieme alle versioni Java EE e altre informazioni, vedi [Liberty features ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/support/knowledgecenter/SSEQTP_liberty/com.ibm.websphere.wlp.doc/ae/rwlp_feat.html).

## Funzioni specifiche per il cloud
{:#cloud-features}

Le seguenti funzioni sono incluse e sono specifiche per {{site.data.keyword.Bluemix_notm}}:
* appstate-2.0
* cloudAutowiring-1.0
* logAnalysis-1.0

Una sottoserie di funzioni disponibili vengono abilitate per impostazione predefinita quando si distribuiscono file WAR o EAR. Consulta [Applicazioni autonome](optionsForPushing.html#stand_alone_apps) per i dettagli.

Il runtime Liberty for Java rende inoltre disponibili alcune funzioni beta di Liberty. Queste funzioni sono elencate in [Utilizzo delle funzioni beta](/docs/runtimes/liberty/usingBetaFeatures.html).

## Compatibilità delle funzioni
{:#feature-compatibility}

Alcune funzioni fornite da Liberty non sono disponibili nel runtime Liberty for Java perché non sono applicabili nell'ambiente cloud.

Tieni presente che un server non può caricare funzioni non compatibili, assicurati che sia configurato per abilitare solo le funzioni che sono compatibili. Vedi [Supported Java EE 6 and 7 feature combinations] (https://www.ibm.com/support/knowledgecenter/SSEQTP_liberty/com.ibm.websphere.wlp.doc/ae/rwlp_prog_model_supported_combos.html").

Le applicazioni che utilizzano EJB remoti possono essere distribuite a {{site.data.keyword.Bluemix_notm}} ma gli EJB remoti non sono accessibili in remoto con il protocollo CORBA/IIOP a causa di limitazioni delle porte nell'ambiente {{site.data.keyword.Bluemix_notm}}.

## Indice funzioni Liberty
{: #libertyfeat_index}

Passa alla sezione dell'elenco di funzioni utilizzando il seguente indice o puoi ricercare nell' [Elenco di funzioni Liberty for Java](#libertyfeat_list).

### A-E
* [A](#libertyfeat_A)
* [B](#libertyfeat_B)
* [C](#libertyfeat_C)
* [E](#libertyfeat_E)

### F-J
* [F](#libertyfeat_F)
* [H](#libertyfeat_H)
* [J](#libertyfeat_J)

### K-O
* [L](#libertyfeat_L)
* [M](#libertyfeat_M)
* [O](#libertyfeat_O)

### P-T
* [P](#libertyfeat_P)
* [R](#libertyfeat_R)
* [S](#libertyfeat_S)
* [T](#libertyfeat_T)

### U - Z
* [W](#libertyfeat_W)


## Elenco di funzioni Liberty
{: #libertyfeat_list}

### A
{: #libertyfeat_A}

* apiDiscovery-1.0
* appSecurity-1.0
* appSecurity-2.0
* appstate-1.0
* appstate-2.0

### B
{: #libertyfeat_B}

* batch-1.0
* batchManagement-1.0
* beanValidation-1.1
* bells-1.0
* blueprint-1.0

### C
{: #libertyfeat_C}

* cdi-1.0
* cdi-1.2
* cloudant-1.0
* cloudAutowiring-1.0
* concurrent-1.0
* constrainedDelegation-1.0
* couchdb-1.0

### E
{: #libertyfeat_E}

* ejb-3.2
* ejbLite-3.1
* ejbLite-3.2
* el-3.0
* eventLogging-1.0

### F
{: #libertyfeat_f}

* federatedRegistry-1.0

### H
{: #libertyfeat_h}

* httpWhiteboard-1.0

### J
{: #libertyfeat_J}
* jacc-1.5
* jaspic-1.1
* javaee-7.0
* javaMail-1.5
* jaxb-2.2
* jaxrs-1.1
* jaxrs-2.0
* jaxrsClient-2.0
* jaxws-2.2
* jca-1.6
* jca-1.7
* jcaInboundSecurity-1.0
* jdbc-4.0
* jdbc-4.1
* jdbc-4.2
* jms-1.1
* jms-2.0
* jmsMdb-3.1
* jmsMdb-3.2
* jndi-1.0
* jpa-2.0
* jpa-2.1
* jsf-2.0
* jsf-2.2
* jsfContainer-2.2
* json-1.0
* jsonp-1.0
* jsp-2.2
* jsp-2.3
* jwt-1.0

### L
{: #libertyfeat_L}
* ldapRegistry-3.0
* localConnector-1.0
* logAnalysis-1.0
* logstashCollector-1.0

### M
{: #libertyfeat_M}
* managedBeans-1.0
* mdb-3.1
* mdb-3.2
* mediaServerControl-1.0     
* microProfile-1.0
* microProfile-1.2
* microProfile-1.3
* mongodb-2.0
* monitor-1.0
* mpConfig-1.1
* mpFaultTolerance-1.0
* mpHealth-1.0
* mpJwt-1.0
* mpMetrics-1.0

### O
{: #libertyfeat_O}
* oauth-2.0
* openapi-3.0
* openapi-3.1
* openid-2.0
* openidConnectClient-1.0
* openidConnectServer-1.0
* opentracing-1.0
* osgi.jpa-1.0
* osgiAppIntegration-1.0
* osgiBundle-1.0
* osgiConsole-1.0

### P
{: #libertyfeat_P}
* passwordUtilities-1.0

### R
{: #libertyfeat_R}
* requestTiming-1.0
* restConnector-1.0
* restConnector-2.0
* rtcomm-1.0
* rtcommGateway-1.0

### S
{: #libertyfeat_S}
* samlWeb-2.0
* scim-1.0
* servlet-3.0
* servlet-3.1
* sessionDatabase-1.0
* sipServlet-1.1
* socialLogin-1.0
* spnego-1.0
* ssl-1.0

### T
{: #libertyfeat_T}
* timedOperations-1.0
* transportSecurity-1.0

### W
{: #libertyfeat_W}
* wab-1.0
* wasJmsClient-1.1
* wasJmsClient-2.0
* wasJmsSecurity-1.0
* wasJmsServer-1.0
* webCache-1.0
* webProfile-6.0
* webProfile-7.0
* websocket-1.0
* websocket-1.1
* wmqJmsClient-1.1
* wmqJmsClient-2.0
* wsAtomicTransaction-1.2
* wsSecurity-1.1
* wsSecuritySaml-1.1
