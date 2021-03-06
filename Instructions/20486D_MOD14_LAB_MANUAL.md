# Module 14: Hosting and Deployment

# Lab: Hosting and Deployment

#### Scenario

You have been asked to create a web-based aquarium management application for your organization's customers, and deploy this application to Microsoft Azure using AppService. The application should consits of a page showing all the different variety of fish in the aquarium, and should allow the user to edit the details of a fish, and delete a fish.

#### Objectives

After completing this lab, you will be able to:

- Create App Service in Microsoft Azure
- Deploy your application to Microsoft Azure
- Work with Azure SQL database.


#### Lab Setup

Estimated Time: **60 minutes**

### Preparation Steps

1.	Ensure that you have cloned the **20486D** directory from GitHub. It contains the code segments for this course's labs and demos. (**https://github.com/MicrosoftLearning/20486D-DevelopingASPNETMVCWebApplications/tree/master/Allfiles**)

### Exercise 1: Deploying a Web Application to Microsoft Azure

#### Scenario

In this exercise, you will first build and run a simple web application locally. For this you will run migrations for your local db. You will then create an AppService and SQL database in Microsoft Azure. You will configure your application to use the Azure SQL database. You will populate the database using Migrations. After that you will deploy your application to Microsoft Azure. 

The main tasks for this exercise are as follows:

1.	Explore and run the application locally.

2.	Create a new Web App in Microsoft Azure.

3.	Prepare the application for deployment.

4.	Deploy the application.

5.	Run the application in Microsoft Azure.


#### Task 1: Explore and run the application locally

1. Open the **UnderWater.sln** file from the following location: **Allfiles\Mod14\Labfiles\01_Underwater_begin**.

2. Run migrations to create the database locally.

4. Start the application without debugging

5. Select a fish of your choice, and click on **Details**.

    >**Note:** The browser displays the scientific name of the fish. 

4. Verify the fish details, and then click **Back to List**.

5. Close **Microsoft Edge.**. 

#### Task 2: Create a new Web App in Microsoft Azure.

1. Open **Microsft Edge**

2. In **Microsoft Edge**, in the address bar type **http://portal.azure.com**, and then press Enter.

3. Sign-in, and then navigate to **App Services**.

4. Create a Web App in **App Services** by providing all the necessary details.

5. Navigate to **SQL Databases**.

6. Create a new **SQl Server** and **SQL Database** by providing all the necessary details.


#### Task 3: Prepare the application for deployment.

1. In Microsoft Visual Studio 2017, open **Solutions Explorer**, right click **Underwater** and click **Edit Underwater.csproj**

2. In **Underwate.csproj**, add the following code under <PropertyGroup>
```cs 

  <ItemGroup>
         <Content Include="node_modules\**" CopyToPublishDirectory="PreserveNewest" />
  </ItemGroup>

```
#### Task 4: Deploy and explore the application.

1. In Microsoft Visual Studio 2017, open **Solutions Explorer**, right click **Underwater** and click **Publish**

2. Sign in and select the Web App you created.

3. Click **Configure**

4. In the **Publish** window, click **Settings**

5. In the **Publish** window, expand **Databases** and edit  **Default Connection String** to point to SQL Database created in Azure.

6. In the **Publish** window, expand **Entity Framework Migration** and edit **Apply this migration on publish** to point to SQL Database in Azure.

7. In the **Publish** window, Click **Save**.

8. In Microsoft Visual Studio 2017, **publish** window, click **Publish**.


#### Task 5: Update the application and deploy in Microsoft Azure.

1. In the **Underwater** application, in the **Fish** class, add a new property with the following information
    - Scope: **public**
    - Name: **FishType**
    - Type: **string**
    - Access: **Read and write**

2. In the **Underwater** application, in the **Create.cshtml**, after the **INPUT** element of **Scientific Name**, Add an **INPUT** element with the following information:

     - asp-for: **FishType**

3. In the **Underwater** application, in the **Edit.cshtml**, after the **div** element of **Scientific Name**, add a **DIV** with the following information: 

     - asp-for: **Price**


4. Add a **DIV** element with the following information:

     - Class: **form-field**

5. In the **DIV** element, add a **LABEL** element with the following information:

    - asp-for: **FishType**

6. Add an **INPUT** element with the following information:

     - asp-for: **FishType**

7. After the **INPUT** element with **asp-for="FishType"** helper, add a **SPAN** element with the following information:
    - asp-validation-for: **FishType**

8. In the **Details.cshtml** file, add a **DIV** element after the **DIV** element for **Scientific Name**, with the following information: 

 	- Class: **display-label**
    - Content: **@Html.DisplayNameFor(model => model.FishType)**

9. After the **P** element, add a **P** element with the following information:

	- Class: **display-field**
    - Content: ***@Html.DisplayFor(model => model.FishType)**

10. Save all the changes.

11. Run **Add-Migration** for the FishType property.

12. Open **Solutions Explorer**, right click **Underwater** and click **Publish**.


### Exercise 2: Upload the Image to Azure Blob Storage

In this exercise, you will modify the web application you created, to start storing the images in Azure Blob Storage. The images will also be displayed from the storage using the URL generated for each image. The image URL will be stored in the database. For this you will run migrations. You will then create an Azure Storage Account in Microsoft Azure. You will then create a container in the storage account. You will then configure your application to use the Azure Storage Account. You will then change the code to . After that you will deploy your application to Microsoft Azure. 

The main tasks for this exercise are as follows:

1.  Create a blob storage account

2.	Prepare the application for working with Azure Storage.

3.	Write the code to upload an image.

4.	Deploy and Run the application in Microsoft Azure


#### Task 1:    Create a blob storage account

1. Open **Microsft Edge**

2. In **Microsoft Edge**, in the address bar type **http://portal.azure.com**, and then press Enter.

3. Sign-in, and then navigate to **Storage Accounts**.

4. Create a storage account in **Storage Account** by providing all the necessary details.

5. After creation of the storage account, navigate to **Blobs** in the storage account page.

6. Create a container by providing the necessary information.


#### Task 2:    Prepare the application for working with Azure Storage.

1. In **Underwater** application, in Microsoft Visual Studio 2017, add **Connected Services** to be project.

2. While adding **Connected Services**, choose **Cloud Storage with Azure Storage**, and then choose the storage account created in the step above.
    >**Note:** You might have to sign in to Microsoft Azure.

3. In **Underwater** application, in **Solution Explorer**, go to **appsettings.json**

4. To **appsettings.json**, add a new section **ContainerSetttings**, and to this section add **ContainerName** key with value being name of container created in previous section.


#### Task 3:	Write the code to upload an image.

1. In the **Underwater** application, in the **Fish** class, add a new property with the following information
    - Scope: **public**
    - Name: **ImageURL**
    - Type: **string**
    - Access: **Read and write**

2. In the **Underwater** application, in the **UnderwaterRepository** class, modify the constructor to do the following:
    - Accept Configuration as input
    - Using the configuration, get connection string for AzureStorageConnection.
    - Using the configuration, get container name.

2. In the **Underwater** application, in the **UnderwaterRepository** class, add a member variable with following information
    - Scope: **private**
    - Name: **_container**
    - Type: **CloudBlobContainer**

3. In the **Underwater** application, in the **UnderwaterRepository** class, add the following functionality to the constructor
    - Obtain an instance of **CloudStorageAccount** by invoking it's **parse** method.
    - Using the **CloudStorageAccount** instance, create a **CloudBlobClient**
    - Using the **CloudBlobClient**, get a reference to the **Container** that was created in the first task, and assign it to member variable **_container**.

4.  In the **Underwater** application, in the **UnderwaterRepository** class, add a method **UploadImageAsync** with following details
    - Input parameter type: **IFormFile**
    - Scope: **private**
    - Additional qualifier: **async**
    - Returns: **Task<string>**

5.  In the **Underwater** application, in the **UnderwaterRepository** class, in the method **UploadImageAsync** , using the **_container** member variable, invoke **GetBlockBlobReference** passing the **FileName** of input parameter **IFormFile**, and store the result in a variable of type **CloudBlockBlob**.

6.  In the **Underwater** application, in the **UnderwaterRepository** class, in the method **UploadImageAsync** , using the **CloudBlockBlob** variable, invoke **UploadFromStreamAsync**. Pass the read stream of the input parameter **IFormFile** to this method.

7. After completion of the upload stream, return the **Uri** property of the **CloudBlockBlob** variable.


8.  In the **Underwater** application, in the **UnderwaterRepository** class, add a method **DeleteImageAsync** , with following details:
    - Input parameter type: **string**
    - Scope: **private**
    - Additional qualifier: **async**
    - Returns: **Task<bool>**

9.  In the **Underwater** application, in the **UnderwaterRepository** class, in the method **DeleteImageAsync** , using the **_container** member variable, invoke **GetBlockBlobReference** passing the **photoFileName** input parameter , and store the result in a variable of type **CloudBlockBlob**.

10.  In the **Underwater** application, in the **UnderwaterRepository** class, in the method **UploadImageAsync** , using the **CloudBlockBlob** variable, invoke **DeleteAsync**. 


11.  In the **Underwater** application, in the **UnderwaterRepository** class, in the method **AddFish(Fish fish)**, in the **IF** block, call the **UploadImageAsync** method, wait for its completion and store the result, in ImageURL property of Fish object.


12.  In the **Underwater** application, in the **UnderwaterRepository** class, in the method **RemoveFish(int it)**, after fetching **fish** instance using the **id**, add an **IF** block, to check if **ImageURL** of the fish instance is not null and if not null, invoke **DeleteImageAsync** method passing the **ImageName**. 


13. In the **Underwater** application, in the **Edit.cshtml**, change image display in the following way:
    - If ImageURL is not present, fetch image URL by calling GetImage action.
    - If ImageURL is present, set the src of the image to ImageURL.

14. In the **Underwater** application, in the **Eelete.cshtml**, change image display in the following way:
    - If ImageURL is not present, fetch image URL by calling GetImage action.
    - If ImageURL is present, set the src of the image to ImageURL.

15. In the **Underwater** application, in the **Index.cshtml**, change image display in the following way:
    - If ImageURL is not present, fetch image URL by calling GetImage action.
    - If ImageURL is present, set the src of the image to ImageURL.

16. In the **Underwater** application, in the **Details.cshtml**, change image display in the following way:
    - If ImageURL is not present, fetch image URL by calling GetImage action.
    - If ImageURL is present, set the src of the image to ImageURL.

17. Save all the changes.

18. Run **Add-Migration** for the ImageURL property.


#### Task 4:	Deploy and Run the application in Microsoft Azure

1. In Microsoft Visual Studio 2017, open **Solutions Explorer**, right click **Underwater** and click **Publish**

2. In **Microsoft Edge**, explore the application by adding, editing and deleting a fish.




©2018 Microsoft Corporation. All rights reserved.

The text in this document is available under the  [Creative Commons Attribution 3.0 License](https://creativecommons.org/licenses/by/3.0/legalcode), additional terms may apply. All other content contained in this document (including, without limitation, trademarks, logos, images, etc.) are  **not**  included within the Creative Commons license grant. This document does not provide you with any legal rights to any intellectual property in any Microsoft product. You may copy and use this document for your internal, reference purposes.

This document is provided &quot;as-is.&quot; Information and views expressed in this document, including URL and other Internet Web site references, may change without notice. You bear the risk of using it. Some examples are for illustration only and are fictitious. No real association is intended or inferred. Microsoft makes no warranties, express or implied, with respect to the information provided here.
