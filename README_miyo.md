# todo-backendをOCPへデプロイ
## ビルド
> export POSTGRESQL_DRIVER_VERSION=42.2.19  
> mvn clean package -Popenshift
## oc login
> oc login https://api.cluster-kjxc9.kjxc9.sandbox1444.opentlc.com:6443 -u admin
## oc project
> oc project xxx
## postgresql
```
oc new-app postgresql-ephemeral \
  -p DATABASE_SERVICE_NAME=todo-backend-db \
  -p POSTGRESQL_DATABASE=todos
```
```
--> Deploying template "fes/postgresql-ephemeral" to project fes

     PostgreSQL (Ephemeral)
     ---------
     PostgreSQL database service, without persistent storage. For more information about using this template, including OpenShift considerations, see https://github.com/sclorg/postgresql-container/.
     
     WARNING: Any data stored will be lost upon pod destruction. Only use this template for testing

     The following service(s) have been created in your project: todo-backend-db.
     
            Username: userQ0B
            Password: **************
       Database Name: todos
      Connection URL: postgresql://todo-backend-db:5432/
     
     For more information about using this template, including OpenShift considerations, see https://github.com/sclorg/postgresql-container/.

     * With parameters:
        * Memory Limit=512Mi
        * Namespace=openshift
        * Database Service Name=todo-backend-db
        * PostgreSQL Connection Username=userQ0B # generated
        * PostgreSQL Connection Password=************** # generated
        * PostgreSQL Database Name=todos
        * Version of PostgreSQL Image=10-el8

--> Creating resources ...
    secret "todo-backend-db" created
    service "todo-backend-db" created
Warning: apps.openshift.io/v1 DeploymentConfig is deprecated in v4.14+, unavailable in v4.10000+
    deploymentconfig.apps.openshift.io "todo-backend-db" created
--> Success
    Application is not exposed. You can expose services to the outside world by executing one or more of the commands below:
     'oc expose service/todo-backend-db' 
    Run 'oc status' to view your app.
```

## helm install
helm install
> helm install todo-backend -f charts/helm.yaml eap-charts/eap8 
※README.mdには末尾がjboss-eap/eap8となっているが、以下のコマンドでリポジトリを確認すると名前が異なる。
> % helm repo list  
> NAME            URL                                    
> eap-charts      https://jbossas.github.io/eap-charts/

初回はすごい時間がかかるのでbuild PODのlogを見ておく。
> todo-backend-build-artifacts-1-build
