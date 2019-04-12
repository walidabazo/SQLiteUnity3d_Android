## SQLite Unity3d For ( Android , Wondows Phone , Windows , IOS, WINRT )

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
	
### Database 	Name And Path
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
