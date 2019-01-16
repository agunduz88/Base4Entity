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


**Configuration:**

1- go to Base4ConfigurationFile.cs and change it accordingly, I've provided you some free Icons I've found on the internet, ofcourse you can use your own icons..

    class Base4ConfigurationFile: IBase4Configuration
    {

        public Base4ConfigurationFile(IUSERS user)
        {
            USERS = user;
        }

        public ResourceManager Base4Resources { get; set; } = Properties.Resources.ResourceManager;
        public Base4ColorStyle Style { get; set; }
        public Bitmap SearchBtnImage { get; set; } = Properties.Resources.Search;
        public Bitmap NewBtnImage { get; set; } = Properties.Resources.New;
        public Bitmap EditBtnImage { get; set; } = Properties.Resources.Edit;
        public Bitmap WatchBtnImage { get; set; } = Properties.Resources.Watch;
        public Bitmap RemoveBtnImage { get; set; } = Properties.Resources.Remove;
        public Bitmap CrudSaveBtn { get; set; } = Properties.Resources.Save;
        public Bitmap HistoryBtn { get; set; } = Properties.Resources.History;
        public IUSERS USERS { get; set; }
    }


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
 
  1- we will make this our login page so rename it to Login.cs and change Form name accordingly
  
  2- our Login.cs class inherits from Form, change it to Base4Form so it would look like this
  
  3- Create a New Form, name it MainForm.cs and change it's parent class to Base4Form. We will be locating our menu and everything on     this form.( we will use it later)
  
  ![alt text](https://github.com/agunduz88/Base4Entity/blob/master/Tutorial%20Images/Base4LoginBack.png)
  
  4- Rebuild your solution, right click and View designer if you get a designer error rebuild again and it will be fixed.
  if the designer still complains restart visual studio and I guarantee it is fixed:)
  
  so we will place a username and password textbox among with a login button on our login form 
  
  go to the toolbox and write Base4, you will now see all the available Base4Controls. we will be using all of these in our projects and 
  I'll show the usage in the tutorial, it's really very simple, infact simpler than the original controls!
  
  now go to the toolbox and type Base4Textbox and drag it on the form twice (1 for username and 1 for password)
  and drag a Base4Button on the form.
  
  I have named the controls USERNAME, PASSWORD and BtnLogin 
  
  remember earlier I've mentioned that when we do our first migration the Base4Context would embed some tables and fill some of them?
  it creates an admin user named **Admin** and password **sysdba**
  
  before saving the password I encrypt it so that anyone who has access to the database cannot see it 
  
  so inside the LoginBtn click event we'll be encrypt the entered password and check if it matches the encrypted password in the database.
  
     private void BtnLogin_Click(object sender, System.EventArgs e)
        { 
            using (var mf = new MainForm())
            {
                if (string.IsNullOrWhiteSpace(USERNAME.Text) || string.IsNullOrWhiteSpace(PASSWORD.Text)) return;
                using (var db = new AppStructMahkeme())
                {
                    var encrypteddPass = Base4Security.Encrypt(PASSWORD.Text);
                     
                    var user = db.USERS.FirstOrDefault(x => x.U_PASSWORD.Equals(encrypteddPass) && x.U_NAME.Equals(USERNAME.Text));

                    if (user == null) return;

                    //The configuration file we have configured earlier.
                    Base4Configurations.SetConfiguration(new Base4ConfigurationFile(user));
            
                    Hide();
                    mf.ShowDialog();
                }
            }
        }
  
