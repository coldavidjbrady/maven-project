# maven-project
DevOps training during the MuleSoft Connect Conference (19 Apr 17)

#### Configuring HTTPS within a Mule Application

The TLS Configuration Element
The TLS Configuration element is independent of any module or transport. In Mule 3.7 onwards, it’s supported both by the HTTP Connector and in the Web Service Consumer.

You can create this element through the UI of the HTTP Connector’s Global configuration element, on the TLS/SSL tab, or through the UI of the Web Service Consumer, on the Security tab.

In XML, the basic syntax for the element is the following:

```
<tls:context name="customContext">
    <tls:trust-store path="path_to_trustStore_file" password="mulepassword"/>
    <tls:key-store path="clientKeystore" keyPassword="mulepassword"
password="mulepassword"/>
 </tls:context>
 ```
 Here are the specific steps I followed for this project:
 
 1.  Generated a new keystore file:  
 
    `keytool -genkey -alias mule -keyalg RSA -keystore keystore_mp.jks -dname "CN=localhost,O=g3,OU=g3Org,ST=CA,C=US"`
 
     A Java KeyStore (JKS) is a repository of security certificates – either authorization certificates or public key certificates – plus corresponding private keys, used for instance in SSL encryption.
     You will be asked to provdid a key password and a password when you generate the new keystore. Ensure to save the new keystore to the project's src/main/resources folder.
 
 2.  Added the following code to the Configuration XML within the HTTP listener configuration (note that this can be done through the designer as well):
 ```
    <http:listener-config name="mvn_proj-httpListenerConfig" 
    	host="0.0.0.0" port="${https.port}" doc:name="HTTPS Listener Configuration" protocol="HTTPS">
    	<tls:context>
			  <tls:key-store path="keystore_mp.jks" keyPassword="maven_proj"
				  password="maven_proj" alias="mule"/>
		</tls:context>
    </http:listener-config>
 ```
 3.  Add the keystore file to git:
     ```
     git add src/main/resources/keystore_mp.jks`
     git commit -m "added keystore file so project will deploy successfully to Cloudhub"`
     git push
     ```
 <span style="color:blue">
 You should be good to go. Don't forget to add the alias if you included that when the keystore was generated. That stumped me for several hours.
 </span>
 
