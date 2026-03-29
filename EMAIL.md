As discussed, please find the pre-configured JBoss EAP 8.0 package here: https://github.com/varkrish/saml-configuration-test.git

Please extract this on a server and try. To secure a WAR, there are two options:

**Option 1: Server-level configuration (subsystem approach)**
Define the SAML SP/IDP settings in the server's `standalone.xml` under the `<subsystem xmlns="urn:jboss:domain:keycloak-saml:1.4">` section using a `<secure-deployment name="yourapp.war">` block. This is the approach already pre-configured in this package for `helloworld.war`. The advantage here is that SAML configuration is managed centrally at the server level and doesn't require repackaging the application.

**Option 2: Application-level configuration (keycloak-saml.xml)**
Place a `keycloak-saml.xml` file inside the application's `WEB-INF/` directory. A reference copy of this file is included in the repo. This approach keeps the SAML configuration bundled with the application itself, making it portable across servers.

For both options, the application's `WEB-INF/web.xml` must include:
- A `<login-config>` with `<auth-method>KEYCLOAK-SAML</auth-method>`
- A `<security-constraint>` defining which URL patterns to protect
- The required `<security-role>` entries

**To test standalone mode:**
1. Start a local Keycloak on port 8180 with a realm called `broker_saml_test`
2. Start JBoss EAP: `./bin/standalone.sh`
3. Deploy `helloworld.war` to `standalone/deployments/`
4. Navigate to `http://127.0.0.1:8080/helloworld` — you should be redirected to Keycloak for SAML login

Please let me know if you have any questions or run into any issues.
