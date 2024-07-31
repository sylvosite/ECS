# ECS
This will provide an easy tutorial on how to host ECS.

# NOTE
This is just a simplified tutorial, you can install ECS from other github pages, you choose if you want ECS.org, Project X or ECS.com

# HOW TO RUN/BUILD/COMPILE

You will need the following dependencies:
- Windows + WSL or Linux (Currently Windows is more recommended and is easier but if you're hosting a revival for not personal use, you should stick with linux.)
- PostgreSQL (Version 13+)
- Redis-Server (WSL or Linux)
- Node.js (Version 18.XX+)
- Go/GoLang (1.18+)
- .NET/dotnet 6.0 (7.0 or above not supported)

  To download WSL on Windows, please go to Microsoft Store and install Ubuntu and configure there and then install redis from here https://developer.redis.com/create/windows/

  Download PostgreSQL from here https://www.postgresql.org/download/ and after install, search after pgadmin on your device.
  Install node.js, goland and dotnet.

  Step 1. Create a user and database in PostgreSQL, make sure login is enabled for it. Once that's done, put a file in services/api named config.json. Put this in it (replacing the DB, User, and Pass with your credentials):
```
{
    "knex": {
	"client": "pg",
        "connection": {
        "host": "127.0.0.1",
        "user": "postgres",
        "password": "postgres",
        "database": "db_name_here"
        }
    }
}
```
If you don't know your password, it should be 'postgres' and user is 'postgres'

Step 2. Go to services/api then write on file explorer searchbar CMD, then run ```npm i```. This will install the node modules needed for the next command. Now run ```npx knex migrate:latest```. If EVERYTHING was sucessful, all the migrations to the database should work.

Step 3. Go to services/Roblox/Roblox.Website, and rename appsettings.example.json to appsettings.json. Put your database information there, and also change the OwnerUserId to 1, as that is the account that will be automatically created. At bottom of the file, you can find path. Make sure your paths are like this, with the double backslash \\ ```C:\\Users\\username\\srclocation\\services\\api\\storage\\```.

Required: Go to api/public/images, if you don't have a folder called thumbnails then create it, same with groups.

Step 4. Go into services/Roblox/Roblox.Website and go on searchbar and write CMD, now run ```dotnet run```. If everything is successful, you should be able to visit the site at http://localhost:5000. If not, check the errors. If it doesn't work, there could be a folder missing called UnsecuredContent which you can create in api/public. To run in Release mode, use this command ```dotnet run -c Release```.

Step 5. Build the admin panel by going into services/admin. Do the same thing on the searchbar and write cmd, and then running ```npm i``` to install node modules, then ```npm run build```. If everything is successful, restart the dotnet terminal. You can do this super easily by pressing Ctrl + C then doing the same command again.

Step 6. Start the frontend by going into services/2016-roblox-main/docs/get-started.md and read the guide there. It should explain easily to you how to start up the frontend. Do not use dev mode if you don't know what you are doing at all, use ```npm run build``` && ```npm run start``` as it is MUCH faster and better.

Before we continue with setting up the site steps, it's time to register the first account. Go to http://localhost:5000/auth/application, now here's the tricky part.
Without editing code, the only way to get autoaccepted is to have a Roblox account that meets the requirements to get autoaccepted, which are:
- At least 1 year account age
- Either: 20K+ followers, has admin badge, 3 non free roblox made items, 1k place visits, has premium, or 2 past usernames

  # Now after this simplified tutorial, you should understand the rest now.

  Once you made the application and then refreshed a second or two later, it should be accepted. Make SURE to name it ```ROBLOX``` in ALL CAPS! Not doing this can cause some issues.

Now that you've set everything up, go to the admin panel at http://localhost:5000/admin, and now set up the accounts you need to set up. 
- Go to create player, create an account with id 2, name it "UGC", then nullify its password.
- Create another account with create player with any id (I prefer id 12), with the username "BadDecisions". This is the account used to hold items that get moved automatically from GDPR deletions, and removing items from inventories. This account is **USERNAME SPECIFIC**, so make SURE it's spelt exactly right! Just copy paste mine. Once it's made, permanently ban it. (This is so some other stuff works properly).

Now we shall continue setting up the site.

Step 7. To start up the AssetValidationService, go to services/AssetValidationServiceV2 in a cmd/terminal, then run ```go run main.go```. This will start up asset validation service, and will allow you to upload items.

Step 8. RCC/Rendering setup time: Go to services/RCCService, in a command prompt, then run ```RCCService.exe -console -placeid:1818```.  If everything was correctly, RCCService should start up. Go to services/game-server, and add a file named config.json. Put this in it:
```
{
    "rcc": "rcc path here",
    "authorization": "render auth here",
    "baseUrl": "http://localhost:5000",
    "rccPort": 64989,
    "port": 3040,
    "websiteBotAuth": "bot auth here",
    "thumbnailWebsocketPort": 3189,
    "dockerDisabled": true
}
```

Once that's done, in services/game-server, run ```npm i```, then ```npm run build```, then ```npm run start```. If everything is correctly, in a few seconds you should see "[info] new connection" in the console of game-server. Request a render, and if everything went properly, it will work.

Note that you will need to configure a lot more things, these are just the basics.
