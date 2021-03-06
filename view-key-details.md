---

copyright:
  years: 2017, 2020
lastupdated: "2020-03-14"

keywords: get details for a key, get key configuration, get details, view encryption key details, view encryption key, retrieve encryption key details, API examples

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
{:term: .term}

# Viewing details about a key
{: #view-key-details}

You can retrieve the general characteristics of a single encryption key by using {{site.data.keyword.keymanagementservicefull}}.
{: shortdesc}

Retrieving a key requires a _Writer_ or _Manager_ access policy, but you might need a way to view only the details about a key, such as its transition history or configuration, without retrieving the key itself. If you have _Reader_ access permissions, you can use the {{site.data.keyword.keymanagementserviceshort}} API to retrieve only metadata about a key.

## Viewing key details with the API
{: #view-key-details-api}

To view detailed information about a specific key, you can make a `GET` call to the following endpoint.

```
https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/metadata
```
{: codeblock}

1. [Retrieve your authentication credentials to work with keys in the service](/docs/key-protect?topic=key-protect-set-up-api).

2. Retrieve the ID of the key that you would like to inspect.

    The ID value is used to access detailed information about the key. You can find the ID for a key in your service instance by [retrieving a list of your keys](/docs/key-protect?topic=key-protect-view-keys), or by accessing the {{site.data.keyword.keymanagementserviceshort}} dashboard.

3. Get details about the key by running the following cURL command.

    ```cURL
    curl -X GET \
      https://<region>.kms.cloud.ibm.com/api/v2/keys/<key_ID>/metadata \
        -H 'accept: application/vnd.ibm.kms.key+json' \
        -H 'authorization: Bearer <IAM_token>' \
        -H 'bluemix-instance: <instance_ID>' \
        -H 'correlation-id: <correlation_ID>'
    ```
    {: codeblock}

    Replace the variables in the example request according to the following table.

    <table>
      <tr>
        <th>Variable</th>
        <th>Description</th>
      </tr>
      <tr>
        <td><varname>region</varname></td>
        <td><strong>Required.</strong> The region abbreviation, such as <code>us-south</code> or <code>eu-gb</code>, that represents the geographic area where your {{site.data.keyword.keymanagementserviceshort}} service instance resides. See <a href="/docs/key-protect?topic=key-protect-regions#service-endpoints">Regional service endpoints</a> for more information.</td>
      </tr>
      <tr>
        <td><varname>IAM_token</varname></td>
        <td><strong>Required.</strong> Your {{site.data.keyword.cloud_notm}} access token. Include the full contents of the <code>IAM</code> token, including the Bearer value, in the cURL request. For more information, see <a href="/docs/key-protect?topic=key-protect-retrieve-access-token">Retrieving an access token</a>.</td>
      </tr>
      <tr>
        <td><varname>instance_ID</varname></td>
        <td><strong>Required.</strong> The unique identifier that is assigned to your {{site.data.keyword.keymanagementserviceshort}} service instance. For more information, see <a href="/docs/key-protect?topic=key-protect-retrieve-instance-ID">Retrieving an instance ID</a>.</td>
      </tr>
      <tr>
        <td><varname>correlation_ID</varname></td>
        <td>The unique identifier that is used to track and correlate transactions.</td>
      </tr>
      <tr>
        <td><varname>key_ID</varname></td>
        <td><strong>Required.</strong> The identifier for the key that you want to inspect.</td>
      </tr>
      <caption style="caption-side:bottom;">Table 1. Describes the variables that are needed to view a details about a key with the {{site.data.keyword.keymanagementserviceshort}} API</caption>
    </table>

    A successful `GET api/v2/keys/<key_ID>/metadata` response returns details about your key. The following JSON object shows an example returned value for a standard key.

    ```json
    {
      "metadata": {
        "collectionType": "application/vnd.ibm.kms.key+json",
        "collectionTotal": 1
      },
      "resources": [
        {
        "type": "application/vnd.ibm.kms.key+json",
        "id": "6efbc310-63a4-46ee-ae73-cb55ac072039",
        "name": "test-standard-key",
        "state": 1,
        "extractable": true,
        "crn": "crn:v1:bluemix:public:kms:us-south:a/f047b55a3362ac06afad8a3f2f5586ea:8e19aaff-df40-4623-bef2-86cb19a9d8bd:key:6efbc310-63a4-46ee-ae73-cb55ac072039",
        "imported": false,
        "creationDate": "2020-03-12T03:50:12Z",
        "createdBy": "IBMid-503CKNRHR7",
        "algorithmType": "AES",
        "algorithmMetadata": {
            "bitLength": "256",
            "mode": "CBC_PAD"
        },
        "algorithmBitSize": 256,
        "algorithmMode": "CBC_PAD",
        "lastUpdateDate": "2020-03-12T03:50:12Z",
        "dualAuthDelete": {
            "enabled": false
        },
        "deleted": false
        }
      ]
    }
    ```
    {:screen}

    Need to retrieve the `payload` value for a standard key? To learn more, see [Retrieving a key](https://test.cloud.ibm.com/docs/key-protect?topic=key-protect-retrieve-key).
    {: tip}

    For a detailed description of the response parameters, see the {{site.data.keyword.keymanagementserviceshort}} [REST API reference doc](https://{DomainName}/apidocs/key-protect){: external}.
