# Send HTTP POST Request to API Gateway in Golang

## Function Example
```
func "Function Name" (Parameters Parameters Type){
	values := map[string]string{"id": "id value"}
    json_data, err := json.Marshal(values)

    if err != nil {
        log.Fatal(err)
    }
    resp, err := http.Post("API url", "application/json",
        bytes.NewBuffer(json_data))

    if err != nil {
        log.Println("Fail Message")
    }

    var res map[string]interface{}

    json.NewDecoder(resp.Body).Decode(&res)
    log.Println(res["json"])
}
```