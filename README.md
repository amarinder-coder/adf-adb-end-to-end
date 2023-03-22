# adf-adb-end-to-end

Movie recommendations system using ADB and ADF.


Dataset: https://grouplens.org/datasets/movielens/

Step1: Create a resource group and in that create a storage account. In that create 4 blob containers and upload files from dataset.


![image](https://user-images.githubusercontent.com/66850958/226775841-cfb68752-76bd-4241-be5e-cfb628f5ad04.png)


![image](https://user-images.githubusercontent.com/66850958/226775983-1412c4d4-88ad-485b-8173-498005148e3f.png)


Step2: Create a key vault with a secret named 'accesskey' and store the access key 1 of storage account as a secret in that vault.


![image](https://user-images.githubusercontent.com/66850958/226776038-ed7cce88-4b03-4917-a5ad-ac209561e268.png)


Step3: Create a azure databricks workspace. Create a secret scope with the Vault URI(DNS name) and resource ID of keyvault and name it 'myscope'. 
After that, create a python notebook(mount_nb.ipynb) and attach it to a single node cluster(applicable to student account/free trial) to mount the workspace to the storage account using the secret in the key vault created above.


![image](https://user-images.githubusercontent.com/66850958/226776463-103bea1b-cf92-4153-89d5-e27c86e98570.png)












