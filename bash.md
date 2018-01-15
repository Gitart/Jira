## BASH - Jira

### get all Custom fields on Jira server
```
curl -D- -u user:password -X GET -H "Content-Type: application/json" https://jira.instance.com/rest/api/2/field
```

### get JSON output of an issue
```
curl -D- -u user:password -X GET -H "Content-Type: application/json" https://jira.instance.com/rest/api/2/search?jql=key=INFO-68
```

### make 2 issues linked to each other
```
curl -u user:password -w '<%{http_code}>' -H 'Content-Type: application/json' https://jira.instance.com/rest/api/2/issueLink -X POST --data '{"type":{"name":"Related Incident"},"inwardIssue":{"key":"ISS-123"},"outwardIssue":{"key":"SYS-456"}}'
```

### get all custom field Select child values
```
$ curl -u user:pw -X GET https://jira.instance.com/rest/jiracustomfieldeditorplugin/1.1/user/customfieldoptions/12600
[{"optionvalue":"Product1","id":14042,"sequence":0,"disabled":false},{"optionvalue":"Product2","id":14043,"sequence":1,"disabled":false},{"optionvalue":"Product3","id":14044,"sequence":2,"disabled":false},{"optionvalue":"Product4","id":14045,"sequence":3,"disabled":false}]
```

### create new issue
```
api_data="{\"fields\":{\"project\":{\"key\":\"${project}\"},\"summary\":\"my summary\",\"description\": \"my description\",\"issuetype\":{\"name\":\"${issuetype}\"}}}"
curl -u "${jira_user}":"${jira_pw}" -w "<%{http_code}>"  -H "Content-Type: application/json" https://${jira_server}/rest/api/2/issue/ -X POST --data "${api_data}")
```

### update existing ticket, add attachment
```
curl -D- -u "${jira_user}":"${jira_pw}" -w "<%{http_code}>" -X POST -H "X-Atlassian-Token: nocheck" -F "file=@/tmp/attachment_file.txt" https://${jira_server}/rest/api/2/issue/${jira_key}/attachments)
```

### update existing ticket, change assignee
```
curl -D- -u "$user":"$pw" -X PUT -H "Content-Type: application/json" https://$server/rest/api/2/issue/$jira_key --data '{"fields": {"assignee":{"name":"$assignee"}}}'
```

### update existing ticket, edit customfield option value
```
curl -D- -u "$user":"$pw" -X PUT -H "Content-Type: application/json" https://$server/rest/api/2/issue/$jira_key --data '{"customfield_12345":{"value":"myValue"}}'
```

### get all Status history (dont show HTTP header in response, -s -D )
```
curl -s -D -D- -u $user:$pw -X GET -H "Content-Type: application/json" http://$jira_server/rest/api/2/issue/$jira_key/transitions?expand=transitions.fields
```
