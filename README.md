# Kubernetes-Proyecto
Proyecto Kubernetes UCreativa 

--- Deployment --- 

To deploy this wordpress kubernetes application with a MySQL backend perform the following steps:

-Download the files of this repository and place them under the same folder for an easier location of the files.

-Open a CLI session and navigate to the folder where the files of the repository are located.

-Enter the following commands in the order, ignore the "-- text", just copy the commands before the "--"

    > kubectl create -f proyecto-configmap.yaml -- Creates the configmap

    > kubectl create -f proyecto-secret.yaml -- Creates the secret

    > kubectl create -f FE-persistentvolumeclaim.yaml -- Creates PVC for the Frontend

    > kubectl create -f fe-deployment.yaml -- Creates the Frontend Deployment

    > kubectl create -f fe-service.yaml     --  Creates the NodePort Service
    > kubectl get services frontend         --      Under the Ports Column and the one next to the port 80:[xxxx]
                                            --      We will use it in our browser and enter "localhost:[xxxx]"


    > kubectl create -f BE-deployment.yaml  -- Creates the Backend Deployment of Mysql


    > kubectl create -f be-service.yaml     -- Creates the Cluster IP Service for the communication between the Backend and the Frontend

You can now enter to "localhost:[xxxx]" and enjoy your wordpress application.


--- Application Diagram ---

![Kubernetes DevOps](https://user-images.githubusercontent.com/78265698/168189200-aa7404d6-1eb6-4af4-b1b8-5f43264da726.jpg)

--- Rolling Updates --- 

For the Rolling Update we will update the version of MySQL from 5.7 to MySQL 8.0

For doing this we just need to open the "BE-deployment.yaml" on your preference text editor.
Go to Spec > Containers > Image 
    -Change from mysql:5.7 to mysql:8.0
    -Save the Changes to the yaml file
    -Open the CLI and enter "kubectl apply -f BE-deployment.yaml --record"

To check the version changes of MySQL that we peformed on the Rolling Updates steps, enter the following commands

    > kubectl rollout history deployment mysql

You will see a column with a number, the lower the number, the older of the change to mysql that we made.

    > kubectl rollout history deployment "asdad" --revision 3 
    
Check the number after "--revision", this will be the number of revision that appears after entering the rollout history command
You can now check the version of Mysql under Pod Template > Containers > MySQL-container > Image


--- Rollback --- 

To perform a Rollback, just enter the following command on the CLI session
 
    > kubectl rollout undo deployment mysql

This will make the deployment go to the earlier version that kubernetes has on file.
