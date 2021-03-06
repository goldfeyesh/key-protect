---

copyright:
  years: 2020
lastupdated: "2020-02-25"

keywords: set deletion policy, dual authorization, policy-based, key deletion

subcollection: key-protect

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:preview: .preview}
{:term: .term}

# Setting dual authorization policies for keys
{: #set-dual-auth-key-policy}

You can use {{site.data.keyword.keymanagementservicefull}} to set dual authorization policies for individual encryption keys.
{: shortdesc}

Dual authorization is an extra policy that helps to prevent accidental or malicious deletion of keys in your {{site.data.keyword.keymanagementserviceshort}} service instance. When you enable this policy at the key level, {{site.data.keyword.keymanagementserviceshort}} requires an authorization from two users to delete a key.

After you enable dual authorization at the key level, the policy that is associated with the key can no longer be changed to allow a single authorization to delete the key. 
{: important}

To enable dual authorization settings at the instance level, check out [Managing service settings](/docs/key-protect?topic=key-protect-manage-settings#manage-dual-auth-instance-policies).
{: tip}

## Managing dual authorization policies with the API
{: #manage-dual-auth-key-policies-api}

Consider the following items before you enable dual authorization for a key:

- **Determine whether a dual authorization policy is required for the key.** As a security admin, assess the sensitivity of your workload to determine whether a key requires a dual authorization policy based on your requirements. After you enable dual authorization for a key, the policy can no longer be changed to allow a single authorization to delete the key. 
- **Determine who can authorize deletion of your {{site.data.keyword.keymanagementserviceshort}} resources.** After you create a dual authorization policy for a key, the key will require [an action from two users](/docs/key-protect?topic=key-protect-delete-dual-auth-keys) before it can be deleted. Be sure to identify two distinct users with the [appropriate levels of access](/docs/key-protect?topic=key-protect-manage-access#service-access-roles) to the instance or key.

To learn how to delete a key that has a dual authorization policy, see [Deleting keys using dual authorization](/docs/key-protect/key-protect?topic=key-protect-delete-dual-auth-keys).

### Viewing a dual authorization policy for a key
{: #view-dual-auth-key-policy-api}

For a high-level view, you can retrieve the dual authorization policy for a single key by making a `GET` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies?policy=dualAuthDelete
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

    To work with dual authorization policies, you must be assigned a _Manager_ access policy for the instance or key. To learn how IAM roles map to {{site.data.keyword.keymanagementserviceshort}} service actions, check out [Service access roles](/docs/key-protect?topic=key-protect-manage-access#service-access-roles).
    {: note}

2. Retrieve the dual authorization policy for a specified key by running the following cURL command.

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies?policy=dualAuthDelete \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'accept: application/vnd.ibm.kms.policy+json'
    ```
    {: codeblock}

    Replace the variables in the example request according to the following table.
    <table>
      <tr>
        <th>Variable</th>
        <th>Description</th>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td><strong>Required.</strong> The unique identifier for the key that has an existing rotation policy.</td>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>Required.</strong> The region abbreviation, such as <code>us-south</code> or <code>eu-gb</code>, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} service instance resides. For more information, see <a href="/docs/key-protect?topic=key-protect-regions#service-endpoints">Regional service endpoints</a>.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>Required.</strong> Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the <code>IAM</code> token, including the Bearer value, in the cURL request. For more information, see <a href="/docs/key-protect?topic=key-protect-retrieve-access-token">Retrieving an access token</a>.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>Required.</strong> The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance. For more information, see <a href="/docs/key-protect?topic=key-protect-retrieve-instance-ID">Retrieving an instance ID</a>.</td>
      </tr>
        <caption style="caption-side:bottom;">Table 1. Describes the variables that are needed to view a dual authorization policy for a key with the {{site.data.keyword.keymanagementserviceshort}} API.</caption>
    </table>

    A successful request returns dual authorization policy details that are associated with your key. The following JSON object shows an example response for a key that has an existing dual authorization policy.

    ```
    {
      "metadata": {
          "collectionTotal": 1,
          "collectionType": "application/vnd.ibm.kms.policy+json"
      },
      "resources": [
      {
          "id": "...",
          "crn": "...",
          "dualAuthDelete": {
            "enabled": true
          },
          "createdBy": "...",
          "creationDate": "...",
          "updatedBy": "...",
          "lastUpdateDate": "..."
        }
      ]
    }
    ```
    {:screen}

    For keys that do not have an existing dual authorization policy, the following JSON shows an example response.

    ```
    {
      "metadata": {
        "collectionTotal": 0,
        "collectionType": "application/vnd.ibm.kms.policy+json"
      }
    }
    ```
    {:screen}

### Creating a dual authorization policy for a key
{: #create-dual-auth-key-policy-api}

Create a dual authorization policy for single key by making a `PUT` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies?policy=dualAuthDelete
```
{: codeblock}

After you enable a dual authorization policy for a single key, the policy cannot be reverted.  
{: important}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

    To work with dual authorization policies, you must be assigned a _Manager_ access policy for the instance or key. To learn how IAM roles map to {{site.data.keyword.keymanagementserviceshort}} service actions, check out [Service access roles](/docs/key-protect?topic=key-protect-manage-access#service-access-roles).
    {: note}

2. Enable dual authorization for a specified key by running the following cURL command.

    ```cURL
    curl -X PUT \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/policies?policy=dualAuthDelete \
      -H 'authorization: Bearer <IAM_token>' \
      -H 'bluemix-instance: <instance_ID>' \
      -H 'content-type: application/vnd.ibm.kms.policy+json' \
      -d '{
      "metadata": {
        "collectionType": "application/vnd.ibm.kms.policy+json",
        "collectionTotal": 1
      },
      "resources": [ 
        {
        "type": "application/vnd.ibm.kms.policy+json",
        "dualAuthDelete": {
          "enabled": true
          }
        }
      ]
    }'
    ```
    {: codeblock}

    Replace the variables in the example request according to the following table.
    <table>
      <tr>
        <th>Variable</th>
        <th>Description</th>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td><strong>Required.</strong> The unique identifier for the key that you want to create a dual authorization policy for.</td>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>Required.</strong> The region abbreviation, such as <code>us-south</code> or <code>eu-gb</code>, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} service instance resides. For more information, see <a href="/docs/key-protect?topic=key-protect-regions#service-endpoints">Regional service endpoints</a>.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>Required.</strong> Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the <code>IAM</code> token, including the Bearer value, in the cURL request. For more information, see <a href="/docs/key-protect?topic=key-protect-retrieve-access-token">Retrieving an access token</a>.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>Required.</strong> The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance. For more information, see <a href="/docs/key-protect?topic=key-protect-retrieve-instance-ID">Retrieving an instance ID</a>.</td>
      </tr>
        <caption style="caption-side:bottom;">Table 2. Describes the variables that are needed to update a dual authorization policy with the {{site.data.keyword.keymanagementserviceshort}} API.</caption>
    </table>

    A successful request returns a `200 OK` response with dual authorization policy details for your key. The following JSON object shows an example response.
    
    ```
    {
      "metadata": {
        "collectionType": "application/vnd.ibm.kms.policy+json",
        "collectionTotal": 1
      },
      "resources": [
        {
          "id": "...",
          "crn": "...",
          "dualAuthDelete": {
            "enabled": true
          },
          "createdBy": "...",
          "creationDate": "...",
          "updatedBy": "...",
          "lastUpdateDate": "..."
        }
      ]
    }
    ```
    {: screen}

    The key now requires an authorization from two users before it can be deleted. For more information, see [Deleting keys using dual authorization](/docs/key-protect/key-protect?topic=key-protect-delete-dual-auth-keys).
    

