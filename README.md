## SQLite Unity3d For ( Android , Wondows Phone , Windows , IOS, WINRT )
#### Connection Database Sqlite how to Create Table  , Select , Insert , Update , Delete , Search


[![Watch the video](https://img.youtube.com/vi/dezAuScV9ZY/0.jpg)](https://youtu.be/dezAuScV9ZY)

Show Tutorial SQLite Unity3d ( Android , Windows Phone , Windows , IOS, WINRT )

https://www.youtube.com/watch?v=dezAuScV9ZY 

### Must Be Add this Refranace


using Mono.Data.Sqlite;

using System;

using System.Data;

using System.IO;

using UnityEngine.UI;



### interface enables you to implement a Connectionclass

private string conn, sqlQuery;

IDbConnection dbconn;

IDbCommand dbcmd;

private IDataReader reader;
	
### Database 	Name
string DatabaseName = "Yourdatabasename.s3db";

### For all platform
string filepath = Application.dataPath + "/Plugins/" + DatabaseName;

###  Application database Path android
 string filepath = Application.persistentDataPath + "/" + DatabaseName;
 
 
### DatabBase Path for Android 

           // If not found on android will create Tables and database

            Debug.LogWarning("File \"" + filepath + "\" does not exist. Attempting to create from \"" +
                             Application.dataPath + "!/assets/Employers");



            // #UNITY_ANDROID
            WWW loadDB = new WWW("jar:file://" + Application.dataPath + "!/assets/Employers.s3db");
            while (!loadDB.isDone) { }
            // then save to Application.persistentDataPath
            File.WriteAllBytes(filepath, loadDB.bytes);
            
        
            
### Databasde SQLite Path IOS 

// #UNITY_IOS


    var loadDb = Application.dataPath + "/Raw/" + DatabaseName;
    // this is the path to your StreamingAssets in iOS
    // then save to Application.persistentDataPath
    File.Copy(loadDb, filepath);




### DatabBase Path for Windows Phone :

 // #UNITY_WP8
 
    var loadDb = Application.dataPath + "/StreamingAssets/" + DatabaseName;  
    // this is the path to your StreamingAssets in iOS
    //then save to Application.persistentDataPath
    File.Copy(loadDb, filepath);         



### DatabBase Path for  WINRT

 // #UNITY_WINRT
 
      var loadDb = Application.dataPath + "/StreamingAssets/" + DatabaseName;  
     // this is the path to your StreamingAssets in iOS
    // then save to Application.persistentDataPath
    File.Copy(loadDb, filepath);
      
 

### open Database SQlite Unity  connection
        conn = "URI=file:" + filepath;

        Debug.Log("Stablishing connection to: " + conn);
        dbconn = new SqliteConnection(conn);
        dbconn.Open();

### To Create Table  Sqlite
This Example:

Table Name (Staff) Have ID is PRIMARY KEY , Name, Address

        string query;
        query = "CREATE TABLE Staff (ID INTEGER PRIMARY KEY   AUTOINCREMENT, Name varchar(100), Address varchar(200))";
        try
        {
            dbcmd = dbconn.CreateCommand(); // create empty command
            dbcmd.CommandText = query; // fill the command
            reader = dbcmd.ExecuteReader(); // execute command which returns a reader
        }
        catch (Exception e)
        {

            Debug.Log(e);

        }
	
### To Select  Sqlite


    string Name_readers, Address_readers;
    using (dbconn = new SqliteConnection(conn))
        {
            dbconn.Open(); //Open connection to the database.
            IDbCommand dbcmd = dbconn.CreateCommand();
            string sqlQuery = "SELECT  Name, Address " + "FROM Staff";// table name
            dbcmd.CommandText = sqlQuery;
            IDataReader reader = dbcmd.ExecuteReader();
            while (reader.Read())
            {
              
                Name_readers = reader.GetString(0);
                Address_readers = reader.GetString(1);

               
                Debug.Log(" name =" + Name_readers + "Address=" + Address_readers);
            }
            reader.Close();
            reader = null;
            dbcmd.Dispose();
            dbcmd = null;
            dbconn.Close();
	    
	    
### To Insert  Sqlite

    using (dbconn = new SqliteConnection(conn))
        {
            dbconn.Open(); //Open connection to the database.
            dbcmd = dbconn.CreateCommand();
            sqlQuery = string.Format("insert into Staff (name, Address) values (\"{0}\",\"{1}\")", name, Address);// table name
            dbcmd.CommandText = sqlQuery;
            dbcmd.ExecuteScalar();
            dbconn.Close();
        }
    
        Debug.Log("Insert Done  ");
	
### To Update  Sqlite	

      using (dbconn = new SqliteConnection(conn))
        {
            dbconn.Open(); //Open connection to the database.
            dbcmd = dbconn.CreateCommand();
            sqlQuery = string.Format("UPDATE Staff set name = @name ,address = @address where ID = @id ");

            SqliteParameter P_update_name = new SqliteParameter("@name", update_name);
            SqliteParameter P_update_address = new SqliteParameter("@address", update_address);
            SqliteParameter P_update_id = new SqliteParameter("@id", update_id);

            dbcmd.Parameters.Add(P_update_name);
            dbcmd.Parameters.Add(P_update_address);
            dbcmd.Parameters.Add(P_update_id);

            dbcmd.CommandText = sqlQuery;
            dbcmd.ExecuteScalar();
            dbconn.Close();
            Search_function(t_id.text);
        }
	
### To Search By ID
  
    using (dbconn = new SqliteConnection(conn))
         {
            string Name_readers_Search, Address_readers_Search;
            dbconn.Open(); //Open connection to the database.
            IDbCommand dbcmd = dbconn.CreateCommand();
            string sqlQuery = "SELECT name,address " + "FROM Staff where id =" + Search_by_id;// table name
            dbcmd.CommandText = sqlQuery;
            IDataReader reader = dbcmd.ExecuteReader();
            while (reader.Read())
            {
                
                Name_readers_Search = reader.GetString(0);
                Address_readers_Search = reader.GetString(1);
                data_staff.text += Name_readers_Search + " - " + Address_readers_Search + "\n";

                Debug.Log(" name =" + Name_readers_Search + "Address=" + Address_readers_Search);

            }
            reader.Close();
            reader = null;
            dbcmd.Dispose();
            dbcmd = null;
            dbconn.Close();
