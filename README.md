# Harmony Chat Backend API Documentation/Whitepaper

## About

Harmony Chat is meant as a simple *discord alternative. Meaning, we are not going to REPLACE discord or replicate it's API, however we will have easay to understand endpoints to make transitioning easy. The API will also be using open technologies such as GUN.js to make our systems decentralized. API server-side will be written using express.js The express.js port will be, by default in the .env.example, 7664, but is changable via .env variable. Servers used as GUN.js distrobution endpoints in the GUN.js initalization will also be in the .env variable. For example, if the array of GUN.js distrobution endpoints is ```['http://localhost:8765/gun', 'https://gun-manhattan.herokuapp.com/gun']``` in the .env (which will be the deafault in .env.example), the running code will be ```let gun = Gun(['http://localhost:8765/gun', 'https://gun-manhattan.herokuapp.com/gun']);``` for starting gun after we require all dependencies*

## Current BaseURL Example

https://example.com/harmonychat-api/v1 (so the BasePath is "/harmonychat-api/v1", this will be stored in the global const variable "basePath", thus the path for the enpoint for registration, not including arguments, is: "/harmonychat-api/v1/auth/register", or in our code: `${basePath}/auth/register`, which will be the basic technique we use in express.js to combine our base path and our endpoints while not being afraid to change version numbers or base paths.)

## Planned endpoints for v1

## Login/regeister system (auth)

* /auth/register?uname="sans"&passwd="sha256EncodedPassword" - Registers username "sans" with a sha256 encoded password. (Password should be encoded client-side using modern JS standards!)
* /auth/login?uname="serif"&passwd="sha256EncodedPassword" - attempts to log in as serif

### Messages read (get)

* /msgs/get?id=channelOrFriendshipID - gets messages from the channel/friend id provided
* /msgs/get?id=channelOrFriendshipID&q="Example" - Searches for message "Example" with the provided id.

### Messages post

* /msgs/post?id=channelOrFriendshipID&uname="sans"&passwd="sha256EncodedPassword"&postIsCall=false&post="Hello world!" - Posts as sans the message "Hello, world!", verifies by checking the global "let" variable "currentPassword" in the serverside code.
* /msgs/post?id=channelOrFriendshipID&uname="sans"&passwd="sha256EncodedPassword"&postIsCall=true - Posts a Jitsi call to the channel ID. Uses "https://meet.jit.si" instance followed by a random 6 word menomic, seperated by "-" symbols. Example output: "https://meet.jit.si/doge-lord-node-sparky-elon-rocket"

### Server and person details

* /common-details/get?detailType="friendsOrChannels"&unameOrServerID="tux"&passwd="sha256EncodedPassword" - Get list of channels for server or friends. Passwd is not needed for the case of listing server channels, as demonstrated here, but *is* needed for listing a persons friends with friendship IDs next to them (Example `tux - ${randomizedFriendshipID}`). 
* /common-details/create?detailType="friendsOrChannels"&serverIdOrUname="tux"&selfPasswd="sha256EncodedPassword"&friendUsernameOrChannelName="penny" - creates a random friendship/channel ID (friendship and channel IDs are interchangeable in the API), freindship/channel same for the friend username and self username if "friendUsernameOrChannelName" is a person's username. if it's actually a channel name, add the channel named with the nammed passed through "friendUsernameOrChannelName" to the server "serverIdOrUname" but only if it's a valid server and the user credentials match those of the a server admin.

### Server

* /server/create?serverName="tux's igloo"&uname="tux"&passwd="sha256EncodedPassword" - Creates server with random server ID and server name of the serverName variable, sets the first admin in the admins dictionary to a username (tux) and a password (sha256EncodedPassword, as always this should be encoded on the client side, replacing sha256EncodedPassword with the encoded password!)