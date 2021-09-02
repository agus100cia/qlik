## Api Rest Reload Task

Para poder ejecutar la recarga de una tarea de Qlik mediante el API, es necesario autenticarse con los certificados y definiendo el usuario.


#### Genere los certificados

Ref: https://help.qlik.com/en-US/sense-admin/November2020/Subsystems/DeployAdministerQSE/Content/Sense_DeployAdminister/QSEoW/Administer_QSEoW/Managing_QSEoW/export-certificates.htm

- 1 Abra https://QPS server name/qmc
- 2 Seleccione Certificates en QMC start page or from the Arrow down menu. The Export page for Certificates is displayed.
- 3 In the Machine name box, type the full computer name of the computer that you are creating the certificates for: MYMACHINE.mydomain.com or the IP address.
  

### Postman obtener la lista de todas las tasks

ref: https://community.qlik.com/t5/Knowledge/How-to-use-Postman-to-make-API-calls-and-use-verify-JSON-output/ta-p/1713581

- 1 Copy the Certificates to the server/computer where Postman is used.
- 2 Open Postman and go to menu "Settings" , In the General tab set "SSL certificate verification" to Off, In the Certificates Tab select "add Certificate" and set the path to the exported certs. The CRT file path will correspond to the path where the client.pem certificate resides, the Key file to the cleint_key.pem
- Pruebe

- GET: https://192.168.248.206:4242/qrs/task/full?xrfkey=12345678qwertyui

PARAMS:

- xrfkey=12345678qwertyui

HEADERS:

- X-Qlik-xrfkey=12345678qwertyui
- X-Qlik-User=UserDirectory=QLIKSENSE;UserId=qlik

### RELOAD TASK

https://help.qlik.com/en-US/sense-developer/May2021/Subsystems/RepositoryServiceAPI/Content/Sense_RepositoryServiceAPI/RepositoryServiceAPI-Task-Start-Task.htm


- POST :  https://192.168.248.206:4242/qrs/task/24133dbc-7713-4292-913a-b4d03553877e/start/synchronous?xrfkey=12345678qwertyui

PARAMS:

- xrfkey=12345678qwertyui

HEADERS:

- X-Qlik-xrfkey=12345678qwertyui
- X-Qlik-User=UserDirectory=QLIKSENSE;UserId=qlik

### CURL

REf: https://help.qlik.com/en-US/sense-developer/May2021/Subsystems/RepositoryServiceAPI/Content/Sense_RepositoryServiceAPI/RepositoryServiceAPI-Example-Connect-cURL-Certificates.htm


```sh
## TASKS
curl \
--insecure \
--key /home/airflow/airflow/cert/client_key.pem \
--cert /home/airflow/airflow/cert/client.pem \
--location \
--request GET \
'https://192.168.248.206:4242/qrs/task/full?xrfkey=12345678qwertyui' \
--header 'X-Qlik-xrfkey: 12345678qwertyui' \
--header 'X-Qlik-User: UserDirectory=QLIKSENSE;UserId=qlik'


## RELOAD TASK


curl \
--insecure \
--key client_key.pem \
--cert client.pem \
--location \
--request POST \
'https://192.168.248.206:4242/qrs/task/24133dbc-7713-4292-913a-b4d03553877e/start/synchronous?xrfkey=12345678qwertyui' \
-H "Content-Length: 0" \
--header 'X-Qlik-xrfkey: 12345678qwertyui' \
--header 'X-Qlik-User: UserDirectory=QLIKSENSE;UserId=qlik'


curl \
--insecure \
--key /home/airflow/airflow/cert/client_key.pem \
--cert /home/airflow/airflow/cert/client.pem \
--request POST \
'https://192.168.248.206:4242/qrs/task/24133dbc-7713-4292-913a-b4d03553877e/start/synchronous?xrfkey=12345678qwertyui' \
--header 'Content-Length: 0' \
--header 'X-Qlik-xrfkey: 12345678qwertyui' \
--header 'X-Qlik-User: UserDirectory=QLIKSENSE;UserId=qlik'

``` 


