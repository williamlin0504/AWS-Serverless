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