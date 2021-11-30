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
  
  ### Step 2: Install Cloud Pak for Integration Operator
  
  Link: https://www.ibm.com/docs/en/cloud-paks/cp-integration/2021.3?topic=installing-operators-using-openshift-console
  
  Add scc restricted to user 
  
  ```
  oc adm policy add-scc-to-user restricted -z default
  ```
 
  You may choose to install all capabilities or selected capabilities
  
  ### A few tips
  
  1. Use manual approval to avoid auto upgrade
  2. Install in a namespace instead of all namespaces

  ### Troubleshooting Operator Installation
  
  Check subscription health
  ```
  oc get subs
  oc describe subs subs-name
  ```
  
  Check csv 
  ```
  oc get csv 
  oc describe csv csv-name 
  ```
  
