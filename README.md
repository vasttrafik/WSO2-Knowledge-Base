# wso2-knowledge-base
Lessons learned working with WSO2 products

## Utilizing Identity Server 5.3 as a Key Manager instance together with API Manager 1.10
In WSO2s documentation for using an Identity Server as a Key Manager together with an API Manager setup, the guide states that API Manager 1.10 works with Identity Server 5.1 and that later versions of Identity Server works together with API Manager 2.0+.
As we had a working setup of API Manager 1.10 deployed and needed it to work with a new setup of Identity Server 5.3 we had to do some workarounds to make it all work together.

1. Follow the guide at: https://docs.wso2.com/display/CLUSTER44x/Configuring+the+Identity+Server+5.1.0+as+a+Key+Manager+with+API+Manager+1.10.0 (Install feature API Key Manager 6.1.66 instead of 5.0.3)
2. Since the api-management database doesn't match up between API Manager 1.10 and Identity Server 5.3 it must be manually "fixed".
Compare the database-scripts located at <WSO2_HOME>/dbscripts/apimgt for your your prefered database between API Manager and Identity Server. Manually add the missing tables, columns and indexes (if starting from an API MAnager 1.10 database setup. If starting from an Identity Server 5.3 database setup, add the missing initial insert values to the tables).
3. Build a new version of org.wso2.carbon.apimgt.keymgt.stub (since it needs to adhere to the Identity Server 5.3s key validation WSDL). Use our feature-branch here: https://github.com/vasttrafik/carbon-apimgt/tree/feature/is-5.3/service-stubs/apimgt/org.wso2.carbon.apimgt.keymgt.stub and add it as a patch to API Manager (gateway nodes).
4. If using our client credentials grant handler (https://github.com/vasttrafik/vt-api-custom-grant-handlers) instead build it from feature branch: https://github.com/vasttrafik/vt-api-custom-grant-handlers/tree/feature/is-5.3