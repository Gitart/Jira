## Upadate Tasks
Sample for update exsisting task by code C12


```golang
// **************************************************************************************************************************
// 
// Title   : Обновление полей в JIRA
// Author  : Savchenko Arthur
// Date    : 12/01/2018 18:40
// Company : 
// 
// Контроль выполнения 
// http://jira.unity-bars.com:11000/rest/api/2/issue/CA-12
// 
// Запуск выполнения 
// http://localhost:5555/api/updatetask/
// 
// Help :
// curl -D- -u "$user":"$pw" -X PUT -H "Content-Type: application/json" https://$server/rest/api/2/issue/$jira_key --data '{"customfield_12345":{"value":"myValue"}}'
// *****************************************************************************************************************************
func api_update_rls(w http.ResponseWriter, r *http.Request){

    // Строка запроса
    var data Mst
    url:="http://jira.server.com:11000/rest/api/2/issue/C12"

    
    Data:=`{"fields":{
    	               "description":         "Новая заявка задача для получения версий Верс. Ver: 122330001-1111", 
    	               "customfield_11110":   "Обновлен релиз № 000 235467",
    	               "labels":              ["Reliase",  "New"],
    	               "customfield_11013":   "Am-000001",
    	               "customfield_11310":   "Sprint 00123",
    	               "customfield_10510":   123.00,
    	               "customfield_10511":   234,
    	               "customfield_11210":   "2018-05-02",
    	               "duedate":             "` + CTM() + `"
    	             }
    	    }`
    
    var jsonStr = []byte(Data)

    // Запрос  к JIRA
	req, _ := http.NewRequest("PUT", url, bytes.NewBuffer(jsonStr))
	req.SetBasicAuth(Suser, Spass)
	req.Header.Add("Content-Type",      "application/json")
	req.Header.Add("Cache-Control",     "no-cache")
    
    res, err := http.DefaultClient.Do(req)
	Err(err, "Error detect default client.")
	defer res.Body.Close()

    body, err := ioutil.ReadAll(res.Body)
	Err(err, "Error read all.")

	// Json processing
	errj := json.Unmarshal([]byte(body), &data)
	Err(errj, "Error read body.")

    // View body return from JIRA
	fmt.Println("Response Body:", data)
}
```
