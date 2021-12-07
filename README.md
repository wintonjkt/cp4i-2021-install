# cp4i-2021-install  
  
  ## Step 1: Add online operator catalog to Openshift Cluster
  
  Link: https://www.ibm.com/docs/en/cloud-paks/cp-integration/2021.3?topic=installing-adding-online-catalog-sources-your-openshift-cluster
  
  1. Log into the OpenShift web consolewith your OpenShift cluster admin credentials.
  2. In the top banner, click the plus (+) icon to open the Import YAML dialog box.
  3. Paste this resource definition into the dialog box
  ```
  apiVersion: operators.coreos.com/v1alpha1
  kind: CatalogSource
  metadata:
    name: ibm-operator-catalog
    namespace: openshift-marketplace
  spec:
    displayName: IBM Operator Catalog
    image: 'icr.io/cpopen/ibm-operator-catalog:latest'
    publisher: IBM
    sourceType: grpc
    updateStrategy:
      registryPoll:
        interval: 45m
  ```
  ## Step 2: Add entitlement key
  
  1. Go to https://myibm.ibm.com/products-services/containerlibrary
  2. Select the View library option to verify your entitlement(s).
  3. Select the Get entitlement key to retrieve the key.
  4. Create a secret containing your IBM Entitlement Key in the tools namespace (which is where the IBM API Connect operator has been installed/deployed into and there the IBM API Connect instance it will create afterwards will end up into as a result).
```
oc create secret docker-registry ibm-entitlement-key -n tools \
--docker-username=cp \
--docker-password="<entitlement_key>" \
--docker-server=cp.icr.io
```
  5. Link that docker-registry secret containing your IBM Entitlement Key with the default secret for pulling Docker images within your Red Hat OpenShift project
```
oc secrets link default ibm-entitlement-key --for=pull
```  
  ## Step 3: Install Cloud Pak for Integration Operator
  
  Link: https://www.ibm.com/docs/en/cloud-paks/cp-integration/2021.3?topic=installing-operators-using-openshift-console
  
  Add scc restricted to user
  
  ```
  oc adm policy add-scc-to-user restricted -z default
  ```
 
  You may choose to install all capabilities or selected capabilities, start with common services first and then platform navigator.
  Platform navigator password is in platform-auth-idp-credentials secret in ibm-common-services project.   
  
  ## A few tips
  
  1. Use manual approval to avoid auto upgrade
  2. Install in a namespace instead of all namespaces

  ## Troubleshooting Operator Installation
  
  ### Check subscription health
  ```
  oc get subs
  oc describe subs subs-name
  ```
  
  ### Check csv 
  ```
  oc get csv 
  oc describe csv csv-name 
  ```
  ### Operator installation in UpgradePending status
  
  Link: https://www.ibm.com/docs/en/cloud-paks/cp-management/2.2.x?topic=upgrade-operator-shows-upgradepending-status-olm-known-issue
  
