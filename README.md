<img height="90" alt="Slonik Duke" align="right" src="docs/media/img/slonik_duke.png" />

# PostgreSQL JDBC Driver

PostgreSQL JDBC Driver (PgJDBC for short) allows Java programs to connect to a PostgreSQL database using standard, database independent Java code. Is an open source JDBC driver written in Pure Java (Type 4), and communicates in the PostgreSQL native network protocol.

### Status
[![Build status](https://ci.appveyor.com/api/projects/status/d8ucmegnmourohwu/branch/master?svg=true)](https://ci.appveyor.com/project/davecramer/pgjdbc/branch/master)
[![Build Status](https://travis-ci.com/pgjdbc/pgjdbc.svg?branch=master)](https://travis-ci.com/pgjdbc/pgjdbc)
[![codecov.io](http://codecov.io/github/pgjdbc/pgjdbc/coverage.svg?branch=master)](http://codecov.io/github/pgjdbc/pgjdbc?branch=master)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/org.postgresql/postgresql/badge.svg)](https://maven-badges.herokuapp.com/maven-central/org.postgresql/postgresql)
[![Javadocs](http://javadoc.io/badge/org.postgresql/postgresql.svg)](http://javadoc.io/doc/org.postgresql/postgresql)
[![License](https://img.shields.io/badge/License-BSD--2--Clause-blue.svg)](https://opensource.org/licenses/BSD-2-Clause)
[![Join the chat at https://gitter.im/pgjdbc/pgjdbc](https://badges.gitter.im/pgjdbc/pgjdbc.svg)](https://gitter.im/pgjdbc/pgjdbc?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

## Supported PostgreSQL and Java versions
The current version of the driver should be compatible with **PostgreSQL 8.4 and higher** using the version 3.0 of the protocol and **Java 8** (JDBC 4.2) or above. Unless you have unusual requirements (running old applications or JVMs), this is the driver you should be using.

PgJDBC regression tests are run against all PostgreSQL versions since 9.1, including "build PostgreSQL from git master" version. There are other derived forks of PostgreSQL but they have not been certified to run with PgJDBC. If you find a bug or regression on supported versions, please file an [Issue](https://github.com/pgjdbc/pgjdbc/issues).

## Get the Driver
Most people do not need to compile PgJDBC. You can download the precompiled driver (jar) from the [PostgreSQL JDBC site](https://jdbc.postgresql.org/download.html) or using your chosen dependency management tool:

### Maven Central
You can search on The Central Repository with GroupId and ArtifactId [![Maven Search](https://img.shields.io/badge/org.postgresql-postgresql-yellow.svg)][mvn-search] for:

[![Java 8](https://img.shields.io/badge/Java_8-42.3.1-blue.svg)][mvn-jre8]
```xml
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <version>42.3.1</version>
</dependency>
```

[mvn-search]: https://search.maven.org/search?q=g:org.postgresql%20AND%20a:postgresql "Search on Maven Central"
[mvn-jre8]: https://search.maven.org/artifact/org.postgresql/postgresql/42.3.1/jar

#### Development snapshots
Snapshot builds (builds from `master` branch) are also deployed to Maven Central, so you can test current development version (test some bugfix) using:
```xml
<dependency>
  <groupId>org.postgresql</groupId>
  <artifactId>postgresql</artifactId>
  <version>42.3.2-SNAPSHOT</version>
</dependency>
```

There are also available (snapshot) binary RPMs in [Fedora's Copr repository](https://copr.fedorainfracloud.org/coprs/g/pgjdbc/pgjdbc-travis/).

----------------------------------------------------
## Documentation
For more information you can read [the PgJDBC driver documentation](https://jdbc.postgresql.org/documentation/head/) or for general JDBC documentation please refer to [The Java™ Tutorials](http://docs.oracle.com/javase/tutorial/jdbc/).

### Driver and DataSource class

| Implements                          | Class                                          |
| ----------------------------------- | ---------------------------------------------- |
| java.sql.Driver                     | **org.postgresql.Driver**                      |
| javax.sql.DataSource                | org.postgresql.ds.PGSimpleDataSource           |
| javax.sql.ConnectionPoolDataSource  | org.postgresql.ds.PGConnectionPoolDataSource   |
| javax.sql.XADataSource              | org.postgresql.xa.PGXADataSource               |

### Building the Connection URL
The driver recognises JDBC URLs of the form:
```
jdbc:postgresql:database
jdbc:postgresql:
jdbc:postgresql://host/database
jdbc:postgresql://host/
jdbc:postgresql://host:port/database
jdbc:postgresql://host:port/
jdbc:postgresql://?service=myservice
```
The general format for a JDBC URL for connecting to a PostgreSQL server is as follows, with items in square brackets ([ ]) being optional:
```
jdbc:postgresql:[//host[:port]/][database][?property1=value1[&property2=value2]...]
```
where:
 * **jdbc:postgresql:** (Required) is known as the sub-protocol and is constant.
 * **host** (Optional) is the server address to connect. This could be a DNS or IP address, or it could be *localhost* or *127.0.0.1* for the local computer. To specify an IPv6 address your must enclose the host parameter with square brackets (jdbc:postgresql://[::1]:5740/accounting). Defaults to `localhost`.
 * **port** (Optional) is the port number listening on the host. Defaults to `5432`.
 * **database** (Optional) is the database name. Defaults to the same name as the *user name* used in the connection.
 * **propertyX** (Optional) is one or more option connection properties. For more information see *Connection properties*.

### Logging
PgJDBC uses java.util.logging for logging.
To configure log levels and control log output destination (e.g. file or console), configure your java.util.logging properties accordingly for the org.postgresql logger.
Note that the most detailed log levels, "`FINEST`", may include sensitive information such as connection details, query SQL, or command parameters.

#### Connection Properties
In addition to the standard connection parameters the driver supports a number of additional properties which can be used to specify additional driver behaviour specific to PostgreSQL™. These properties may be specified in either the connection URL or an additional Properties object parameter to DriverManager.getConnection.

| Property                      | Type    | Default | Description   |
| ----------------------------- | ------- | :-----: | ------------- |
| user                          | String  | null    | The database user on whose behalf the connection is being made. |
| password                      | String  | null    | The database user's password. |
| options                       | String  | null    | Specify 'options' connection initialization parameter. |
| service                       | String  | null    | Specify 'service' name described in pg_service.conf file. References: [The Connection Service File](https://www.postgresql.org/docs/current/libpq-pgservice.html) and [The Password File](https://www.postgresql.org/docs/current/libpq-pgpass.html). 'service' file can provide all properties including 'hostname=', 'port=' and 'dbname='. |
| ssl                           | Boolean | false   | Control use of SSL (true value causes SSL to be required) |
| sslfactory                    | String  | null    | Provide a SSLSocketFactory class when using SSL. |
| sslfactoryarg (deprecated)    | String  | null    | Argument forwarded to constructor of SSLSocketFactory class. |
| sslmode                       | String  | prefer  | Controls the preference for opening using an SSL encrypted connection. |
| sslcert                       | String  | null    | The location of the client's SSL certificate |
| sslkey                        | String  | null    | The location of the client's PKCS#8 SSL key |
| sslrootcert                   | String  | null    | The location of the root certificate for authenticating the server. |
| sslhostnameverifier           | String  | null    | The name of a class (for use in [Class.forName(String)](https://docs.oracle.com/javase/6/docs/api/java/lang/Class.html#forName%28java.lang.String%29)) that implements javax.net.ssl.HostnameVerifier and can verify the server hostname. |
| sslpasswordcallback           | String  | null    | The name of a class (for use in [Class.forName(String)](https://docs.oracle.com/javase/6/docs/api/java/lang/Class.html#forName%28java.lang.String%29)) that implements javax.security.auth.callback.CallbackHandler and can handle PasswordCallback for the ssl password. |
| sslpassword                   | String  | null    | The password for the client's ssl key (ignored if sslpasswordcallback is set) |
| sendBufferSize                | Integer | -1      | Socket write buffer size |
| receiveBufferSize             | Integer | -1      | Socket read buffer size  |
| logServerErrorDetail          | Boolean | true    | Allows server error detail (such as sql statements and values) to be logged and passed on in exceptions.  Setting to false will mask these errors so they won't be exposed to users, or logs. |
| allowEncodingChanges          | Boolean | false   | Allow for changes in client_encoding |
| logUnclosedConnections        | Boolean | false   | When connections that are not explicitly closed are garbage collected, log the stacktrace from the opening of the connection to trace the leak source |
| binaryTransferEnable          | String  | ""      | Comma separated list of types to enable binary transfer. Either OID numbers or names |
| binaryTransferDisable         | String  | ""      | Comma separated list of types to disable binary transfer. Either OID numbers or names. Overrides values in the driver default set and values set with binaryTransferEnable. |
| prepareThreshold              | Integer | 5       | Statement prepare threshold. A value of -1 stands for forceBinary |
| preparedStatementCacheQueries | Integer | 256     | Specifies the maximum number of entries in per-connection cache of prepared statements. A value of 0 disables the cache. |
| preparedStatementCacheSizeMiB | Integer | 5       | Specifies the maximum size (in megabytes) of a per-connection prepared statement cache. A value of 0 disables the cache. |
| defaultRowFetchSize           | Integer | 0       | Positive number of rows that should be fetched from the database when more rows are needed for ResultSet by each fetch iteration |
| loginTimeout                  | Integer | 0       | Specify how long to wait for establishment of a database connection.|
| connectTimeout                | Integer | 10      | The timeout value used for socket connect operations. |
| socketTimeout                 | Integer | 0       | The timeout value used for socket read operations. |
| tcpKeepAlive                  | Boolean | false   | Enable or disable TCP keep-alive. |
| tcpNoDelay                    | Boolean | false   | Enable or disable TCP no delay. |
| ApplicationName               | String  | PostgreSQL JDBC Driver    | The application name (require server version >= 9.0). If assumeMinServerVersion is set to >= 9.0 this will be sent in the startup packets, otherwise after the connection is made |
| readOnly                      | Boolean | false   | Puts this connection in read-only mode |
| disableColumnSanitiser        | Boolean | false   | Enable optimization that disables column name sanitiser |
| assumeMinServerVersion        | String  | null    | Assume the server is at least that version |
| currentSchema                 | String  | null    | Specify the schema (or several schema separated by commas) to be set in the search-path |
| targetServerType              | String  | any     | Specifies what kind of server to connect, possible values: any, master, slave (deprecated), secondary, preferSlave (deprecated), preferSecondary |
| hostRecheckSeconds            | Integer | 10      | Specifies period (seconds) after which the host status is checked again in case it has changed |
| loadBalanceHosts              | Boolean | false   | If disabled hosts are connected in the given order. If enabled hosts are chosen randomly from the set of suitable candidates |
| socketFactory                 | String  | null    | Specify a socket factory for socket creation |
| socketFactoryArg (deprecated) | String  | null    | Argument forwarded to constructor of SocketFactory class. |
| autosave                      | String  | never   | Specifies what the driver should do if a query fails, possible values: always, never, conservative |
| cleanupSavepoints             | Boolean | false   | In Autosave mode the driver sets a SAVEPOINT for every query. It is possible to exhaust the server shared buffers. Setting this to true will release each SAVEPOINT at the cost of an additional round trip. |
| preferQueryMode               | String  | extended | Specifies which mode is used to execute queries to database, possible values: extended, extendedForPrepared, extendedCacheEverything, simple |
| reWriteBatchedInserts         | Boolean | false   | Enable optimization to rewrite and collapse compatible INSERT statements that are batched. |
| escapeSyntaxCallMode          | String  | select  | Specifies how JDBC escape call syntax is transformed into underlying SQL (CALL/SELECT), for invoking procedures or functions (requires server version >= 11), possible values: select, callIfNoReturn, call |
| maxResultBuffer               | String  | null    | Specifies size of result buffer in bytes, which can't be exceeded during reading result set. Can be specified as particular size (i.e. "100", "200M" "2G") or as percent of max heap memory (i.e. "10p", "20pct", "50percent") |
| gssEncMode                    | String  | allow   | Controls the preference for using GSSAPI encryption for the connection, values are disable, allow, prefer, and require |
| adaptiveFetch                 | Boolean | false   | Specifies if number of rows fetched in ResultSet by each fetch iteration should be dynamic. Number of rows will be calculated by dividing maxResultBuffer size into max row size observed so far. Requires declaring maxResultBuffer and defaultRowFetchSize for first iteration. 
| adaptiveFetchMinimum          | Integer | 0       | Specifies minimum number of rows, which can be calculated by adaptiveFetch. Number of rows used by adaptiveFetch cannot go below this value. 
| adaptiveFetchMaximum          | Integer | -1      | Specifies maximum number of rows, which can be calculated by adaptiveFetch. Number of rows used by adaptiveFetch cannot go above this value. Any negative number set as adaptiveFetchMaximum is used by adaptiveFetch as infinity number of rows.
| localSocketAddress            | String  | null    | Hostname or IP address given to explicitly configure the interface that the driver will bind the client side of the TCP/IP connection to when connecting.
| quoteReturningIdentifiers     | Boolean | true    | By default we double quote returning identifiers. Some ORM's already quote them. Switch allows them to turn this off
| authenticationPluginClassName | String  | null    | Fully qualified class name of the class implementing the AuthenticationPlugin interface. If this is null, the password value in the connection properties will be used.

## Contributing
For information on how to contribute to the project see the [Contributing Guidelines](CONTRIBUTING.md)

----------------------------------------------------
### Sponsors

* [PostgreSQL International](http://www.postgresintl.com)
