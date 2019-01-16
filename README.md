# Base4Entity

Nuget:
Install-Package Base4Entity

What is Base4Entity?

Base4 Entity is a winforms Search & CRUD framework that works with entity framework.

Why use Base4Entity?

- It'll help you apply a design pattern to your applications, thus keeping your code tidy & clean

- Works fast and is very easy to use.

- You will code much less there fore your applications will be very easy to maintain.

- Has a very beautiful user interface thanks to Dennis Magno for creating MetroFramework UI tools

- Keeps record of changed entities.

Setup: 

  **install on nuget: Install-Package Base4Entity**  or

  **Import the following .dll files into your program:**
  
- Base4Controls.dll
- Base4Entity.dll
- MetroFramework.Design.dll
- MetroFramework.Fonts.dll

and add **Base4ConfigurationFile.cs** to your solution folder.

Tutorial:

To start with following is the list of things to kno before we go any further on the tutorial section:

**1- There are 3 kind of Forms**

   - Base4Form (should be used when There is no Create read update or delete action on that form, these actions should be manually done by the    programmer)
   
   - Base4Crud (should be used when we have create read update delete operations which are all handled by the form)
   
   - Base4Browse (this form is used to search records on a table, create/update/delete are done over the BrowseForm to be more precise when we click on the New/edit/ button on the browse form it opens a CRUD form, I'll briefly explain how things operate later in the tutorial.)
   
**2- The Framework automatically embeds the following tables into your database in order to be able to keep the changes made on the records.**

**- ACTIONS** (keeps the list of the states that a record can be: insert/deleteupdate) MUST

**- LANGUAGES** (keeps the list of the languages that the software will support) MUST

**- CODE_YESNO** ( keeps yes/no/null)  MUST

**- STYLES** (keeps styles list so that the software will operate depending on useres preference) OPTIONAL

**- TRANS_CODES AND TRANS_AUTHORIZATION** ( keeps autorization information whom which users have access on what part of the program) OPTIONAL

**- CHANGE_LOG** (changes made in entities are stored here) MUST

**- USERS**   users table is designed to keep the following information

  a) Username
  
  b) User Password 
  
  c) User is Admin or not
  
  d) UI preference (style)
  
  e) Language preference 
  
  d) Authorization

  below are the list of tables that are autofilled when embeded
  
  **ACTIONS** filled with insert/update/delete 
  
  **LANGUAGES** English language is added by default
  
  **USERS** an admin user is created username:admin password sysdba (which can be changed later) 
  
**3- Framework Attributes there are two attributes that comes with the framework**

  a) **[Base4IgnoreAudit]**   by default the framework keeps records of changes made on every table unless you add the ignoreaudit attribute
  
  b) **[Base4LogDescription]** When tracking the changes if the change is made on a combobox it stores the valuemember on the new/old value   field on the CHANGE_LOGS table, to be able to store the display member we add the LogDescription attribute on the description           property. 
  
 Below are sample usages:
 
  **IgnoreAudit Usage:** 
  /* When added will not keep record of any created or updated records on the language class */
   ![alt text](https://github.com/agunduz88/Base4Entity/blob/master/Tutorial%20Images/ignoreAudit.png)
   
   **LogDescription Usage:**
   
  /* We will have a language selection combobox on users table, when we change the user language it will store L_DESC value to the         New/old values field. if not used it will store L_CODE Value. */
   ![alt text](https://github.com/agunduz88/Base4Entity/blob/master/Tutorial%20Images/Base4LogDescription.png)
 
  
 Now that the "Things to Know" part is over, we can start with the tutorial
 
**If you are at this point of the tutorial Entity Framework and Base4Entity Should be installed.** 
 
 **1- Go to your DBContext and edit it accordingly.** 
 
 - The context file inherits from DbContext as you will notice from the attached image, we will change it to Base4Context
 When done it will embed the 8 tables I've mentioned above.
 
 - I have also added modelBuilder.Conventions.Remove<PluralizingTableNameConvention>(); 
  because we don't want to pluralize our table names unless we create them that way!
  
 - add override SaveChanges() and edit it exactly how it is on the image. If you don't do this your application will still work however it will not keep changes made on the entities.
 
 ![alt text](https://github.com/agunduz88/Base4Entity/blob/master/Tutorial%20Images/DbContext.png)
 
 **2-)When we create a new Winforms project it comes with a default Form named Form1**
