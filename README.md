# Home_Assistant_Roomba
**Using Home Assistant to send iOS notifications to run Roomba 980 when away.**

I decided I wanted Home Assistant to send a notification to my iOS device whenever I was away. For me, this meant whenever my phone was not connected to my WiFi, home assistant would show me as 'Away'. 

After 10 minutes of being away, I would get a custom iOS notification from Home Assistant asking me if I'd like to start Roomba. Clicking "Start Cleaning" would send an API call to my Roomba to start cleaning.

Below are the steps and excerpts from my config files that allowed this to work for me. 

1. Home Assistant needs to be able to determine if you are Home or Away. I use nmap to have my Home/Away status be based on whether or not I      am connected to my home WiFi. Typically, if I am home, I will be connected to my WiFi. If I leave I get disconnected and nmap has        Home Assistant mark me as Away. This works pretty well for the most part. You'll need to add nmap to your configuration.yaml file        (See device_tracker in configuration.yaml).

   I set a static IP for my phone through my router. This way, my phone gets the same IP each time I connect/disconnect. I have the home_interval property set up for 10 minutes. This device will only be scanned every 10 minutes to help relieve traffic on my network and        preserve battery life on my device. This is also good because if I happen to turn my phone on/off, take out the garbage, or              disconnect from my WiFi for less than 10 minutes then I won't be marked as Away. This cuts down on fale positive notifications later    on. Adjust as you see fit. I've also included an exclude on the host in which Home Assistant is running. I've read this can cause        issues.

2. nmap should now be scanning your network for the hosts you've included in your configuration.yaml file. A file called                    known_devices.yaml should be created with information on your device. See known_devices.yaml for what my file looks like. You can        change the seemingly random string to something more readable. I changed mine to matt_phone. You can also update the name property to    something more friendly. I chose Matt. This is what will show up in Home Assistant if you have your Home/Away status displayed as a      badge. Feel free to add a picture as well which will be used in the badges too. 

   You should now be able to see this device in Developer tools > States in Home Assistant. My device is displayed as                      device_tracker.matt_phone with the source as my router and other details. 

3. This is where Roomba comes into play now. You'll need to grab the BLID and password of your Roomba device. I used the following          library in order to grab this information. I spun up a docker with ubuntu but feel free to run this however you can.                    https://github.com/koalazak/dorita980#how-to-get-your-usernameblid-and-password

4. You now need to add roomba as a device in your configuration.yaml file. Again, I assigned a static IP to my roomba since I did run      into an issue where DHCP assigned it's IP to another device and everything went haywire until I fixed that. You'll need to paste the    BLID into username and the password into password. 

5. Now we'll work on the iOS portion. You'll need to download Home Assistant from the app store. I ran into some roadblocks with getting    Home Assistant to work on a remote network. I used Duck DNS to get a free dynamic DNS. iOS does not like when you don't use SSL and      it threw a ton of errors until I got some SSL certs. I used Let's Encrypt (https://letsencrypt.org/) since it's a trusted CA and that    allowed me to get connected securely. 

6. Once connected to your Home Assistant server in the iOS app you can update the Device ID to something more friendly. I chose            app_matts_iphone. This allowed me identify my device more easily. 

7. In the iOS app you need to enable notificaitons in the "Notification Settings" of the app. Update push settings will pull the latest    changes from your Homse Assistant instance. You'll need to do this each time you make a change to notifications. 

8. You should now see a change in Home Assistant > Developer tools > State. You should have a new Entity called                            device_tracker.app_matts_iphone or whatever you Device ID was in your iOS app. This allows you to create automations against that        device.

9. You will now need to add iOS push notifications to your configuration.yaml. See the ios portion of my conifguration.yaml file for        details. 

10. You will now need to add to your automations.yaml file to trigger and then run an action. See my automations.yaml file for what is       in my file. I'll try to break down the 2 jobs as best as I can.

    Request cleaning when not home -
    When the state of my device goes from 'home' to 'not_home' (which is essentially from Home to Away) then send an iOS notification to     app_matts_iphone that states "Would you like to start the vacuum?" 
   
    Start cleaning - 
    When the notification has been fired it calls RUN_VACUUM which was defined in configuration.yaml. 
    The title in the ios part of the configuration.yaml file shows what the button on the iOS notification will say. 
    Going back to automations.yaml the action will be to start_pause on the entity vacuum.roomba

11. You should now have 2 new automation entities in Developer tools > State. They are called automation.request_cleaning_when_not_home     and automation.start_cleaning. You can test to see if these work by clicking the More Info box and triggering the job. Otherwise,       you'd need to disconnect from your WiFi and wait 10 minutes. 

   These notifications are really useful on the Apple Watch and come through very clean. 
   
I hope that helps some folks. 
