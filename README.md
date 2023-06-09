# ADF-ADB-end-to-end

Movie recommendations system using ADB and ADF.


![image](https://user-images.githubusercontent.com/66850958/226788005-5cbe5ae4-a936-425a-84fe-3d051dd40dcc.png)



Dataset: https://grouplens.org/datasets/movielens/

Step1: Create a resource group and in that create a storage account. In that create 4 blob containers and upload files from dataset.


![image](https://user-images.githubusercontent.com/66850958/226775841-cfb68752-76bd-4241-be5e-cfb628f5ad04.png)


![image](https://user-images.githubusercontent.com/66850958/226775983-1412c4d4-88ad-485b-8173-498005148e3f.png)


Step2: Create a key vault with a secret named 'accesskey' and store the access key 1 of storage account as a secret in that vault.


![image](https://user-images.githubusercontent.com/66850958/226776038-ed7cce88-4b03-4917-a5ad-ac209561e268.png)


Step3: Create a azure databricks workspace. Create a secret scope with the Vault URI(DNS name) and resource ID of keyvault and name it 'myscope'. 
After that, create a python notebook(mount_nb.ipynb) and attach it to a single node cluster(applicable to student account/free trial) to mount the workspace to the storage account using the secret in the key vault created above.


![image](https://user-images.githubusercontent.com/66850958/226776463-103bea1b-cf92-4153-89d5-e27c86e98570.png)


Step4: Create a new notebook which has main logic(movie_nb.ipynb).

![image](https://user-images.githubusercontent.com/66850958/226822993-4f53b9fe-cdcb-490d-835c-1041b90c7ec9.png)


Step5: Create a logic app to send an email to the user with the movie recommendations or a error message.


![image](https://user-images.githubusercontent.com/66850958/226825915-4e394959-a8a0-46cf-b490-26707caf04bb.png)



Step6: Create a Azure Data Factory. In that create a pipeline with various activites. 


![image](https://user-images.githubusercontent.com/66850958/226823209-6406551b-6bf4-41a5-a21b-a49a7851194e.png)


ADF Pipeline explanation:


1) We create 2 metadata activites to get structure of dummy files containing only the columns stored in refdata container.
2) We create 2 more metadata activites to get structure of actual files stored in rawdata container.
3) We create a if condition that matches the schema of original files with dummy files.


![image](https://user-images.githubusercontent.com/66850958/226824080-5dc7f11f-e57c-4a43-b79f-411ffdf59cda.png)

If True, then we copy data from rawdata to validated container, then run the notebook(movie_nb.ipynb) to get the output of movie recommendations.
(NOTE: we pass the parameter "input" as pipeline parameter "UserforRec")


![image](https://user-images.githubusercontent.com/66850958/226824424-5648ec52-a534-48ea-b3c6-a76749faad8a.png)


If False, then we copy data from rawdata to rejected container.


4) We add 2 web activity to send an email to the user(i.e me) with the output of movie recommendations given by the ADB notebook.


![image](https://user-images.githubusercontent.com/66850958/226825626-5188eba8-c797-4acc-bac8-2bd6cf055794.png)


5) We add a blobeventtrigger i.e if any files is added to the rawdata container starting with movie and ending with .csv ,the pipeline will be triggered automatically.


![image](https://user-images.githubusercontent.com/66850958/226826463-be9b1699-7a3a-4cfe-b758-34cbfbc7cf1d.png)


Step7: I triggered the pipeline with a user id of 677 as shown below, it was passed as a pipeline parameter.

![image](https://user-images.githubusercontent.com/66850958/226826744-d87136fa-7d40-4770-a4eb-c7f9dd209a04.png)


Successful output on ADF monitoring.


![image](https://user-images.githubusercontent.com/66850958/226826880-4d01d207-ae83-4a59-9319-718a683d4653.png)


Got a success email with movie recommendations for userid with 677.


![image](https://user-images.githubusercontent.com/66850958/226827116-15ddbcd6-b915-4b77-9d0c-d941abc8c6ad.png)


Delete the resource group containing all the resources.


![image](https://user-images.githubusercontent.com/66850958/226828334-fd0435b6-f6df-4d5f-88ec-d2a5aab8485f.png)















