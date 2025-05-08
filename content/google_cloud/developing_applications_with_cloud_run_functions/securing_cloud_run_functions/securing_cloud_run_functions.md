◀️ [Home](../../../../README.md)

## Securing Cloud Run functions

### Accessing and authenticating to functions
You secure access to Cloud Run functions with identity-based access controls and network-based access controls.

#### Identity-based access controls
To secure access with identity-based access controls, the first step is to validate the identity credential, and make sure that the requestor is who it says it is. Once the requestor's identity has been authenticated, its level of access or permissions can be evaluated. **This step is called authorization**. By default, functions are deployed as private, and require authentication. You can also deploy a function as public and not require authentication.

Cloud Run functions supports two different kinds of identities: Service accounts, which serve as the identity of a non-person, like a function, application, or a VM. And user accounts, which represent people - either as individual Google Account holders or as part of a Google Group.

To authenticate to Cloud Run functions, clients create a token based on the service account or user account credential. The token has a limited lifetime, is passed with the request, and serves to safely authenticate the account. The token-based authentication is used to limit the potential damage that might occur if the service or user account credential is leaked. Cloud Run functions uses two types of tokens (created using the OAuth 2.0 framework and Open Identity Connect (OIDC)): 
- OAuth 2.0 access tokens, used to authenticate API calls.
- ID tokens, used to authenticate calls to developer-created code, for example, a function calling another function.

Once the identity of the requesting entity has been confirmed, the permissions of the identity are evaluated. Cloud Run functions uses Identity and Access Management (IAM) to evaluate permissions using roles. A role is a set of individual permissions that are grouped together and assigned to the account, either directly or through a policy configuration.

Event-driven functions can only be invoked by the event source that they are subscribed to. HTTP functions can be invoked by different types of identities, for example, a developer who is testing the function, or a service that uses the function. When testing a function as a developer, you must have a user account to access Cloud Run functions with a role that contains the appropriate permissions on the function being invoked. Then, generate an ID token using this account, and pass the token in the Authorization header of the request to the function.

#### Network-based access controls
You can also limit access by specifying network settings for Cloud Run functions. Network settings enable you to control network ingress and egress to and from individual functions. Ingress settings restrict whether a function can be invoked by resources outside of your Google Cloud project or VPC Service Controls service perimeter. You can choose to allow all traffic, allow internal traffic only, or allow internal traffic and traffic from Cloud Load Balancing. Internal traffic is traffic from Workflows and VPC networks in the same project or VPC Service Controls perimeter.

Egress settings control the routing of outbound HTTP requests from a function. To specify egress settings, you must connect functions to a VPC network using a Serverless VPC Access connector. Egress settings control which types of traffic are routed through the connector to your VPC network. You can set egress settings to route all outbound traffic from the function through the connector, or, route only requests to private IPs through the connector.

### Protecting Cloud Run functions
You can use Cloud Key Management Service (KMS) to create and manage encryption keys that protect your Cloud Run functions and related data at rest. The keys are known as customer-managed encryption keys (CMEK) and are owned by you and not controlled by Google. They can be stored as software keys, in an HSM cluster, or externally. Deploying a function with a CMEK protects the data associated with it by using an encryption key that only you can access. If the key is disabled or destroyed, no one (including you) can access the data that is protected by the key. 

If a key is destroyed or disabled, or the requisite permissions on it are revoked: Active instances of functions protected by the key are not shut down. Executions of these functions that are already in progress will continue to run. New executions will fail as long as Cloud Run functions does not have access to the key. Executions that require new function instances will fail.

