# API Gateway Integrate with DynamoDB

## Properties

* [Insert into DynamoDB](#insert-into-dynamoDB)
* [Get all Items from DynamoDB](#get-all-items-from-dynamoDB)
* [Updtae Items in DynamoDB](#update-items-in-dynamodb)

## Insert into DynamoDB
* **Stage Method:** *POST* 
### Integration Request
* **HTTP Method:** *POST*
* **Action:** *PutItem*
* **Mapping Templates:** *application/json*
```
{ 
    "TableName": "TableName",
    "Item": {
	"id": {
            "S": "$context.requestId"
            },
        "Item_1": {
            "S": "$input.path('$.Item_1')"
            },
        "Item_2": {
            "S": "$input.path('$.Item_2')"
        }
    }
}
```

## Get all Items from DynamoDB
* **Stage Method:** *GET* 
### Integration Request
* **HTTP Method:** *POST*
* **Action:** *Scan*
* **Mapping Templates:** *application/json*
```
{
    "TableName": "TableName"
}
```

### Integration Response
* **Mapping Templates:** *application/json*
```
#set($inputRoot = $input.path('$'))
{
    "comments": [
        #foreach($elem in $inputRoot.Items) {
            "id": "$elem.id.S",
            "Item_1": "$elem.Item_1.S",
            "Item_2": "$elem.Item_2.S",
        }#if($foreach.hasNext),#end
	#end
    ]
}
```

## Updtae Items in DynamoDB
* **Stage Method:** *POST* 
### Integration Request
* **HTTP Method:** *POST*
* **Action:** *UpdateItem*
* **Mapping Templates:** *application/json*
```
{
    "TableName": "TableName",
    "Key": {
        "id": {
            "S": "$input.path('$.id')"
        }
    },
    "UpdateExpression": "set theItemToUpdate = :val1",
    "ExpressionAttributeValues": {
        ":val1": {"S": "Updated_Value"}
    },
    "ReturnValues": "ALL_NEW"
}
```

## Create Table in DynamoDB
* **Stage Method:** *POST* 
### Integration Request
* **HTTP Method:** *POST*
* **Action:** *CreateTable*
* **Mapping Templates:** *application/json*
```
{
    "TableName" = "<Your-Table-Name>",
    "BillingMode": "PAY_PER_REQUEST",//On-Demand
    "AttributeDefinitions": [ 
      { 
         "AttributeName" = "<Your-PartitionKey>",
         "AttributeType": "S" //Type: B, S, N
      },
      { 
         "AttributeName" = "<Your-SortKey>",
         "AttributeType": "S" //Type: B, S, N
      }
   ],
    "KeySchema": [ 
      { 
         "AttributeName": "<Same as the PartitionKey you input above>",
         "KeyType": "HASH" //"HASH" represent partition key
      },
      {
         "AttributeName": "<Same as the SortKey you input above>",
         "KeyType": "RANGE" //"RANGE" represent sort key
      }
   ] 
}
```
> Type: B, S, N = Boolean, String, Number