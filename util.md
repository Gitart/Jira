## Utils for update field in JIRA

```golang
package main

import (
	"bytes"
	"fmt"
	"log"
	"net/http"
	"os"
	"strings"
	"time"
)

type Mst map[string]interface{} // Map - string - interface

// var (
// 	Suser = "user"
// 	Spass = "pass"
// )

/***************************************************************
  Author       Savchenko Arthur
  Description  Main procedure
  Time         10-12-2017 12:00
  Title
 ****************************************************************/
func main() {

	fmrt := false
	if fmrt {
		fmt.Println(os.Args[1], os.Args[2])
	}

	api_update_rls(os.Args[1], os.Args[2])
}

/*************************************************************
 *
 *   Title       : Текущее время в формате (YYYY-MM-DD HH:MM:SS)
 * 	 Date        : 2015-12-14
 *
 ************************************************************/
func CTM() string {
	return time.Now().Format("2006-01-02 15:04:05")
}

func Sepr(Arg string) []string {
	t := strings.Split(Arg, "*")
	return t
}

/***************************************************************
  Author      Savchenko Arthur
  Description Error
  Time        11-12-2017
  Title
 ****************************************************************/
func Err(Er error, Txt string) {
	if Er != nil {
		log.Println("ERROR : "+Txt, "Description : "+Er.Error())
		return
	}
}

// **************************************************************************************************************************
//
// Title   : Обновление полей в JIRA
// Author  : Savchenko Arthur
// Date    : 12/01/2018 18:40
//
// Контроль выполнения
// http://jira.com:1000/rest/api/2/issue/C-12
//
// Запуск выполнения
// http://localhost:5555/api/upd/
//
// Help :
// https://docs.atlassian.com/software/jira/docs/api/REST/7.6.1/#api/2/issue-updateRemoteIssueLink
// curl -D- -u "$user":"$pw" -X PUT -H "Content-Type: application/json" https://$server/rest/api/2/issue/$jira_key --data '{"customfield_12345":{"value":"myValue"}}'
// *****************************************************************************************************************************
func api_update_rls(Ord, Fix string) {

	Suser := os.Getenv("Suser")
	Spass := os.Getenv("Spass")

	fmt.Println(Suser, Spass)

	// Строка запроса
	url := "http://jira.com:1000/rest/api/2/issue/" + Ord
	Data := `{"fields":{"customfield_15213":   "` + Fix + `"}}`

	var jsonStr = []byte(Data)

	// Запрос  к JIRA
	req, ert := http.NewRequest("PUT", url, bytes.NewBuffer(jsonStr))
	Err(ert, "Error detect default client.")

	req.SetBasicAuth(Suser, Spass)
	req.Header.Add("Content-Type", "application/json")
	req.Header.Add("Cache-Control", "no-cache")

	res, err := http.DefaultClient.Do(req)
	Err(err, "Error detect default client.")
	defer res.Body.Close()
}
```
