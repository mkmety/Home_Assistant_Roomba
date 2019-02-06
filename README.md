# Home_Assistant_Roomba
Using HA to send iOS notifications to run when away.

I decided I wanted Home Assistant to send a notification to my iOS device whenever I was 'Away'. For me, this meant whenever my phone was not connected to my WiFi, home assistant would show me as 'Away'. 

After 10 minutes of being away, I would get a custom iOS notification from Home Assistant asking me if I'd like to start Roomba.

Below are the steps and excerpts from my config files that allowed this to work for me. 

1. Home assistant needs to be able to see if you are Home or Away. I use nmap to have my Home/Away status be based on whether or not I am connected to my home WiFi. Typically, if I am home, I will be connected to my WiFi. If I leave I get disconnected and nmap has Home Assistant mark me as Away. This works pretty well for the most part. You'll need to add nmap to your configuration.yaml file (See device_tracker in configuration.yaml).

   I set a static IP for my phone through my router. This way, my phone gets the same IP each time I connect/disconnect. I have              home_interval set up for 10 minutes. This device will only be scanned every 10 minutes to help relieve traffic on my network and          preserve battery life on my device. This is also good because if I happen to turn my phone on/off, take out the garbage, or disconnect    from my WiFi for less than 10 minutes then I won't be marked as Away. This cuts down on fale positive notifications later on. Adjust as    you see fit. I've also included an exclude on the host in which Home Assistant is running. I've read this can cause issues.

2. nmap should now be scanning your network for the hosts you've included in your configuration.yaml file. A file called known_devices.yaml should be created with information on your device. See known_devices.yaml for what my file looks like. You can change the seemingly random string to something more readable. I changed mine to matt_phone. You can also update the name property to something more friendly. I chose Matt. This is what will show up in Home Assistant if you have your Home/Away status displayed as a badge. Feel free to add a picture as well which will be used in the badges too. 

   You should now be able to see this device in Developer tools > States in Home Assistant. My device is displayed as                      device_tracker.matt_phone with the source as my router and other details. 

3. This is where Roomba comes into play now. You'll need to grab the BLID and password of your Roomba device. I used the following          library in order to grab this information. I spun up a docker with ubuntu but feel free to run this however you can.                    https://github.com/koalazak/dorita980#how-to-get-your-usernameblid-and-password

4. You now need to add roomba as a device in your configuration.yaml file. Again, I assigned a static IP to my roomba since I did run into an issue where DHCP assigned it's IP to another device and everything went haywire until I fixed that. You'll need 


You first need to add roomba to your configuration.yaml file (see configuraiton.yaml).
2. The username and password can be retreived using the following library: https://github.com/koalazak/dorita980#how-to-get-your-usernameblid-and-password
3. You then need to add the ios portion to your configuration.yaml file (see configuraiton.yaml).
